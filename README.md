# Voice Nexus

Universal multi-provider voice architecture. Seamlessly routes between STT (speech-to-text), reasoning (LLM), and TTS (text-to-speech) providers.

## Features

- **3 Voice Modes**: Realtime, Turn-based, Hybrid
- **Provider Agnostic**: Works with any STT, LLM, or TTS service
- **Complexity Routing**: Auto-selects reasoning tier based on query
- **Knowledge Injection**: Enrich queries with external knowledge
- **Real-time Events**: Callbacks for all processing stages

## Installation

```bash
npm install @antigravity/voice-nexus
# or
yarn add @antigravity/voice-nexus
# or
pnpm add @antigravity/voice-nexus
```

## Quick Start

```typescript
import { createVoiceNexus, type ReasoningProvider } from '@antigravity/voice-nexus';

// 1. Define your reasoning provider
const myReasoning: ReasoningProvider = {
    name: 'openai',
    models: {
        fast: 'gpt-3.5-turbo',
        balanced: 'gpt-4',
        deep: 'gpt-4-turbo'
    },
    isAvailable: () => !!process.env.OPENAI_API_KEY,
    generate: async (prompt, config) => {
        const response = await openai.chat.completions.create({
            model: config.model || 'gpt-4',
            messages: [{ role: 'user', content: prompt }]
        });
        return {
            text: response.choices[0].message.content || '',
            model: config.model || 'gpt-4'
        };
    }
};

// 2. Create Voice Nexus instance
const nexus = createVoiceNexus({
    config: {
        mode: 'turn-based',
        knowledgeInjection: false,
        providers: {
            reasoning: myReasoning
        }
    },
    events: {
        onTranscriptUpdate: (t) => console.log(`[${t.role}] ${t.text}`),
        onComplexityAnalyzed: (c) => console.log(`Complexity: ${c.tier}`)
    }
});

// 3. Process text input
const response = await nexus.processTextInput('How do I implement OAuth2?');
console.log(response?.text);
```

## Voice Modes

| Mode | Description | Latency | Use Case |
|------|-------------|---------|----------|
| **Realtime** | Streaming STT + fast LLM + streaming TTS | ~500ms | Live conversation |
| **Turn-based** | Complete STT → LLM → TTS pipeline | ~2-5s | High quality responses |
| **Hybrid** | Auto-switches based on complexity | Variable | Best of both |

## Provider Interfaces

### STT Provider

```typescript
interface STTProvider {
    readonly name: string;
    readonly supportsStreaming: boolean;
    transcribe(audio: Blob): Promise<string>;
    startStreaming?(onPartial: (text: string) => void): Promise<void>;
    stopStreaming?(): Promise<string>;
    isAvailable(): boolean;
}
```

### Reasoning Provider

```typescript
interface ReasoningProvider {
    readonly name: string;
    readonly models: { fast: string; balanced: string; deep: string };
    generate(prompt: string, config: ReasoningConfig): Promise<ReasoningResult>;
    isAvailable(): boolean;
}

interface ReasoningConfig {
    tier: 'fast' | 'balanced' | 'deep';
    systemPrompt?: string;
    maxTokens?: number;
    temperature?: number;
}
```

### TTS Provider

```typescript
interface TTSProvider {
    readonly name: string;
    readonly supportsStreaming: boolean;
    readonly voices: VoiceConfig[];
    synthesize(text: string, voice: string, settings?: TTSSettings): Promise<ArrayBuffer>;
    synthesizeStream?(text: string, voice: string, onChunk: (chunk: ArrayBuffer) => void): Promise<void>;
    getVoiceForAgent(agentName: string): string;
    isAvailable(): boolean;
}
```

## Complexity Router

Voice Nexus automatically analyzes queries to select the optimal reasoning tier.

```typescript
import { analyzeComplexity } from '@antigravity/voice-nexus';

const result = analyzeComplexity('How do I architect a distributed system?');
// {
//   score: 0.72,
//   tier: 'deep',
//   signals: { hasCodeIndicators: false, hasReasoningIndicators: true, ... },
//   recommendedProvider: { reasoning: 'claude-opus', tts: 'elevenlabs' }
// }
```

### Complexity Signals

| Signal | Description | Score Impact |
|--------|-------------|--------------|
| Token count | Longer queries | +0.0-0.3 |
| Code indicators | `implement`, `debug`, `function` | +0.25 |
| Reasoning indicators | `analyze`, `compare`, `why` | +0.2 |
| Creative indicators | `brainstorm`, `imagine` | +0.15 |
| Navigation indicators | `go to`, `open`, `show` | -0.3 |

### Tier Selection

- **Fast** (score < 0.3): Simple queries → GPT-3.5, Gemini Flash
- **Balanced** (0.3-0.7): Standard queries → GPT-4, Claude Sonnet
- **Deep** (> 0.7): Complex reasoning → Claude Opus, GPT-4 Turbo

## Event Callbacks

```typescript
const nexus = createVoiceNexus({
    config: { mode: 'turn-based', knowledgeInjection: false },
    events: {
        onTranscriptUpdate: (transcript) => {
            console.log(`[${transcript.role}] ${transcript.text}`);
        },
        onPartialTranscript: (partial) => {
            // Live streaming updates
            updateUI(partial.text);
        },
        onProcessingStart: () => setLoading(true),
        onProcessingEnd: () => setLoading(false),
        onComplexityAnalyzed: (result) => {
            console.log(`Complexity: ${result.score.toFixed(2)} → ${result.tier}`);
        },
        onProviderSwitch: (providers) => {
            console.log('Switched providers:', providers);
        },
        onKnowledgeInjected: (context) => {
            console.log(`Injected ${context.searchResults.length} results`);
        },
        onError: (error) => {
            console.error('Voice Nexus error:', error);
        },
        onStateChange: (state) => {
            // Full state updates
            updateUIState(state);
        }
    }
});
```

## Knowledge Injection

Enrich queries with external knowledge:

```typescript
const knowledgeInjector: KnowledgeInjector = {
    isAvailable: () => true,
    injectContext: async (query, expertise) => {
        const results = await searchKnowledgeBase(query);
        return {
            searchResults: results,
            injectedPrompt: `Based on this context:\n${results.map(r => r.content).join('\n')}`
        };
    }
};

const nexus = createVoiceNexus({
    config: {
        mode: 'turn-based',
        knowledgeInjection: true,
        agent: {
            id: 'research-agent',
            name: 'Dr. Ira',
            expertise: ['AI', 'systems design']
        }
    },
    knowledgeInjector
});
```

## Multi-Provider Setup

```typescript
import { createVoiceNexus } from '@antigravity/voice-nexus';

const nexus = createVoiceNexus({
    config: {
        mode: 'hybrid',
        knowledgeInjection: true,
        providers: {
            stt: whisperSTT,           // OpenAI Whisper
            reasoning: claudeProvider,  // Anthropic Claude
            tts: elevenLabsTTS         // ElevenLabs
        },
        agent: {
            id: 'assistant',
            name: 'Nova',
            expertise: ['coding', 'research']
        }
    },
    events: {
        onTranscriptUpdate: updateChat,
        onComplexityAnalyzed: showComplexity
    },
    knowledgeInjector: myKnowledgeSource
});

// Start voice session
await nexus.start();

// Or process directly
const response = await nexus.processTextInput('Analyze this code...');
```

## State Management

```typescript
// Get current state
const state = nexus.getState();
// {
//   mode: 'turn-based',
//   isActive: true,
//   isProcessing: false,
//   currentProvider: { stt: 'whisper', reasoning: 'claude', tts: 'elevenlabs' },
//   transcripts: [...],
//   lastComplexityScore: 0.65,
//   error: null
// }

// Get transcripts
const transcripts = nexus.getTranscripts();

// Clear history
nexus.clearTranscripts();

// Change mode
nexus.setMode('realtime');
```

## License

MIT © Dicoangelo
