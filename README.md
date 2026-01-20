<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:00d9ff&height=200&section=header&text=Voice%20Nexus&fontSize=50&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=Universal%20Multi-Provider%20Voice%20Architecture&descSize=20&descAlignY=55" />
</p>

<p align="center">
  <strong>Seamlessly routes between STT, reasoning, and TTS providers</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Version-1.0.0-00d9ff?style=for-the-badge" alt="Version" />
  <img src="https://img.shields.io/badge/TypeScript-5.0+-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License" />
  <img src="https://img.shields.io/badge/Tier-ELITE-gold?style=for-the-badge" alt="ELITE" />
</p>

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           VOICE NEXUS ORCHESTRATOR                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         INPUT LAYER (STT)                            │   │
│  │                                                                      │   │
│  │   🎤 Audio ──→ [STT Provider] ──→ Transcribed Text                  │   │
│  │                      │                                               │   │
│  │         ┌────────────┼────────────┐                                 │   │
│  │         │            │            │                                 │   │
│  │     Gemini      Whisper      Browser                                │   │
│  │     (Live)      (Batch)      (Fallback)                             │   │
│  │     ~200ms      ~500ms       ~300ms                                 │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                     │                                       │
│                                     ▼                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      COMPLEXITY ROUTER                               │   │
│  │                                                                      │   │
│  │   Text ──→ [Signal Extraction] ──→ [Tier Selection] ──→ Provider    │   │
│  │                     │                      │                         │   │
│  │         ┌───────────┴───────────┐   ┌─────┴─────┐                   │   │
│  │         │ Code:      +0.25      │   │   FAST    │ → Sonnet          │   │
│  │         │ Reasoning: +0.20      │   │   <0.2    │   (ELITE)         │   │
│  │         │ Creative:  +0.15      │   ├───────────┤                   │   │
│  │         │ Navigation: -0.30     │   │ BALANCED  │ → Opus            │   │
│  │         │ Question:   -0.10     │   │  0.2-0.5  │   (ELITE)         │   │
│  │         └───────────────────────┘   ├───────────┤                   │   │
│  │                                     │   DEEP    │ → Opus            │   │
│  │                                     │   >0.5    │   (ELITE)         │   │
│  │                                     └───────────┘                   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                     │                                       │
│                                     ▼                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    KNOWLEDGE LAYER (Optional)                        │   │
│  │                                                                      │   │
│  │   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │   │
│  │   │   Semantic   │  │   Research   │  │    Agent     │             │   │
│  │   │   Search     │  │   Findings   │  │   Expertise  │             │   │
│  │   │              │  │   Injection  │  │   Context    │             │   │
│  │   └──────────────┘  └──────────────┘  └──────────────┘             │   │
│  │                              │                                       │   │
│  │              [Enriched Context → Reasoning]                         │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                     │                                       │
│                                     ▼                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    REASONING LAYER                                   │   │
│  │                                                                      │   │
│  │   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │   │
│  │   │   CLAUDE     │  │   GEMINI     │  │    GROK      │             │   │
│  │   │  Sonnet/Opus │  │  Flash/Pro   │  │  (Creative)  │             │   │
│  │   │              │  │              │  │              │             │   │
│  │   │ • Reasoning  │  │ • Realtime   │  │ • Chat       │             │   │
│  │   │ • Code       │  │ • Grounding  │  │ • Creative   │             │   │
│  │   │ • Analysis   │  │ • Speed      │  │ • Humor      │             │   │
│  │   └──────────────┘  └──────────────┘  └──────────────┘             │   │
│  │                              │                                       │   │
│  │              [Response Text]                                        │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                     │                                       │
│                                     ▼                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      OUTPUT LAYER (TTS)                              │   │
│  │                                                                      │   │
│  │   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │   │
│  │   │  ELEVENLABS  │  │   GEMINI     │  │   BROWSER    │             │   │
│  │   │  (Premium)   │  │  (Native)    │  │  (Fallback)  │             │   │
│  │   │              │  │              │  │              │             │   │
│  │   │ • Emotional  │  │ • Fast       │  │ • Free       │             │   │
│  │   │ • 9 Voices   │  │ • Integrated │  │ • Universal  │             │   │
│  │   │ • Streaming  │  │ • Low-latency│  │ • Basic      │             │   │
│  │   └──────────────┘  └──────────────┘  └──────────────┘             │   │
│  │                              │                                       │   │
│  │              [Audio Output → 🔊]                                    │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Features

- **3 Voice Modes**: Realtime, Turn-based, Hybrid
- **Provider Agnostic**: Works with any STT, LLM, or TTS service
- **ELITE Complexity Routing**: Auto-selects reasoning tier (Opus-first)
- **Knowledge Injection**: Enrich queries with external knowledge
- **Real-time Events**: Callbacks for all processing stages
- **ElevenLabs Integration**: 9 premium voices with emotional range

---

## Installation

```bash
npm install @metaventionsai/voice-nexus
# or
yarn add @metaventionsai/voice-nexus
# or
pnpm add @metaventionsai/voice-nexus
```

---

## Quick Start

```typescript
import { createVoiceNexus, type ReasoningProvider } from '@metaventionsai/voice-nexus';

// 1. Define your reasoning provider
const claudeProvider: ReasoningProvider = {
    name: 'claude',
    models: {
        fast: 'claude-sonnet-4-20250514',      // ELITE: Sonnet even for fast
        balanced: 'claude-opus-4-20250514',    // ELITE: Opus for balanced
        deep: 'claude-opus-4-20250514'         // ELITE: Opus for deep
    },
    isAvailable: () => !!process.env.ANTHROPIC_API_KEY,
    generate: async (prompt, config) => {
        const response = await anthropic.messages.create({
            model: config.model || 'claude-opus-4-20250514',
            messages: [{ role: 'user', content: prompt }],
            max_tokens: 4096
        });
        return {
            text: response.content[0].text,
            model: config.model || 'claude-opus-4-20250514'
        };
    }
};

// 2. Create Voice Nexus instance
const nexus = createVoiceNexus({
    config: {
        mode: 'turn-based',
        knowledgeInjection: false,
        providers: {
            reasoning: claudeProvider
        }
    },
    events: {
        onTranscriptUpdate: (t) => console.log(`[${t.role}] ${t.text}`),
        onComplexityAnalyzed: (c) => console.log(`Complexity: ${c.score.toFixed(2)} → ${c.tier}`)
    }
});

// 3. Process text input
const response = await nexus.processTextInput('How do I architect a distributed system?');
console.log(response?.text);
```

---

## ELITE TIER Configuration

| Setting | ELITE Value | Standard Value | Description |
|---------|-------------|----------------|-------------|
| Fast Threshold | 0.2 | 0.3 | Lower = more orchestration |
| Deep Threshold | 0.5 | 0.7 | Lower = more Opus usage |
| Fast Model | Sonnet | Flash | Quality even for simple |
| Balanced Model | Opus | Sonnet | Opus by default |
| Deep Model | Opus | Opus | Maximum reasoning |
| TTS Provider | ElevenLabs | Gemini | Premium audio always |

### Use Standard Tier

```typescript
import { createVoiceNexus, STANDARD_THRESHOLDS } from '@metaventionsai/voice-nexus';

const nexus = createVoiceNexus({
    config: {
        mode: 'turn-based',
        thresholds: STANDARD_THRESHOLDS  // 0.4/0.75 thresholds
    }
});
```

---

## Voice Modes

| Mode | Flow | Latency | Use Case |
|------|------|---------|----------|
| **Realtime** | Streaming STT → Fast LLM → Streaming TTS | ~500ms | Live conversation |
| **Turn-based** | Complete STT → LLM → TTS pipeline | ~2-5s | High quality responses |
| **Hybrid** | Auto-switches based on complexity | Variable | Best of both |

### Mode Selection

```typescript
// Realtime: Fast responses, live feel
nexus.setMode('realtime');

// Turn-based: Higher quality, full processing
nexus.setMode('turn-based');

// Hybrid: System decides based on query
nexus.setMode('hybrid');
```

---

## Complexity Router

### Signal Extraction

| Signal | Pattern | Impact |
|--------|---------|--------|
| **Token count** | Length of query | +0.0 to +0.25 |
| **Code indicators** | `implement`, `debug`, `function` | +0.25 |
| **Reasoning** | `analyze`, `compare`, `why` | +0.20 |
| **Creative** | `brainstorm`, `imagine` | +0.15 |
| **Navigation** | `go to`, `open`, `show` | -0.30 |
| **Question** | `what is`, `how do` | -0.10 |
| **Domain** | `architecture`, `distributed` | +0.30 |

### Tier Selection (ELITE)

| Tier | Complexity | Model | TTS |
|------|------------|-------|-----|
| **Fast** | <0.2 | Claude Sonnet | ElevenLabs |
| **Balanced** | 0.2-0.5 | Claude Opus | ElevenLabs |
| **Deep** | >0.5 | Claude Opus | ElevenLabs |

### Usage

```typescript
import { analyzeComplexity, ELITE_THRESHOLDS } from '@metaventionsai/voice-nexus';

const result = analyzeComplexity('Design a microservices architecture');
// {
//   score: 0.55,
//   tier: 'deep',
//   signals: { hasCodeIndicators: false, hasReasoningIndicators: true, ... },
//   recommendedProvider: { reasoning: 'claude-opus', tts: 'elevenlabs' }
// }
```

---

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

---

## Event Callbacks

```typescript
const nexus = createVoiceNexus({
    config: { mode: 'turn-based', knowledgeInjection: false },
    events: {
        // Transcript updates
        onTranscriptUpdate: (transcript) => {
            console.log(`[${transcript.role}] ${transcript.text}`);
        },

        // Live streaming (realtime mode)
        onPartialTranscript: (partial) => {
            updateUI(partial.text);
        },

        // Processing state
        onProcessingStart: () => setLoading(true),
        onProcessingEnd: () => setLoading(false),

        // Complexity analysis
        onComplexityAnalyzed: (result) => {
            console.log(`Complexity: ${result.score.toFixed(2)} → ${result.tier}`);
            console.log(`Provider: ${result.recommendedProvider.reasoning}`);
        },

        // Provider changes
        onProviderSwitch: (providers) => {
            console.log('Switched to:', providers);
        },

        // Knowledge injection
        onKnowledgeInjected: (context) => {
            console.log(`Injected ${context.searchResults.length} results`);
        },

        // Errors
        onError: (error) => {
            console.error('Voice Nexus error:', error);
        },

        // Full state changes
        onStateChange: (state) => {
            updateUIState(state);
        }
    }
});
```

---

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
            expertise: ['AI', 'systems design', 'research']
        }
    },
    knowledgeInjector
});
```

---

## Multi-Provider Setup

```typescript
import { createVoiceNexus } from '@metaventionsai/voice-nexus';

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
const response = await nexus.processTextInput('Analyze this architecture...');
```

---

## ElevenLabs Voice Mapping

ELITE tier always uses ElevenLabs with agent-specific voices:

| Agent | Voice | Character |
|-------|-------|-----------|
| Dr. Ira | Clyde | Deep, authoritative |
| Mike | Liam | Narrative, American |
| Caleb | Drew | News anchor |
| Paramdeep | Charlie | Casual |
| Bilal | Fin | Energetic |
| Noah | Dorothy | Gentle |
| Helen | Domi | Expressive |
| Perri | Rachel | Clear |

---

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

// Stop session
await nexus.stop();
```

---

## Related Packages

- **[@metaventionsai/cpb-core](https://github.com/Dicoangelo/cpb-core)** - CPB precision orchestration
- **[ResearchGravity](https://github.com/Dicoangelo/ResearchGravity)** - Research framework

---

## License

MIT © Dicoangelo

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:00d9ff&height=100&section=footer" />
</p>
