# Contributing to Voice Nexus

Thank you for your interest in contributing to Voice Nexus!

## Development Setup

```bash
# Clone the repo
git clone https://github.com/Dicoangelo/voice-nexus.git
cd voice-nexus

# Install dependencies
npm install

# Run in watch mode
npm run dev

# Run tests
npm test

# Type check
npm run typecheck

# Lint
npm run lint
```

## Project Structure

```
src/
├── index.ts        # Main exports
├── types.ts        # TypeScript interfaces (STT, TTS, Reasoning providers)
├── router.ts       # Complexity analysis and tier selection
└── orchestrator.ts # Voice pipeline coordinator
```

## Adding a Provider Example

Create example providers in `examples/providers/`:

```typescript
// examples/providers/openai-reasoning.ts
import type { ReasoningProvider } from '../../src';

export const openaiReasoning: ReasoningProvider = {
    name: 'openai',
    models: {
        fast: 'gpt-3.5-turbo',
        balanced: 'gpt-4',
        deep: 'gpt-4-turbo'
    },
    isAvailable: () => !!process.env.OPENAI_API_KEY,
    generate: async (prompt, config) => {
        // Implementation
    }
};
```

## Code Style

- TypeScript strict mode
- Provider-agnostic design (no hardcoded services)
- Document public APIs with JSDoc
- Keep the core library dependency-free

## Commit Messages

Follow conventional commits:

```
feat: Add streaming TTS support
fix: Handle audio context lifecycle
docs: Add ElevenLabs provider example
test: Add complexity router tests
```

## Pull Request Process

1. Ensure tests pass: `npm test`
2. Ensure types check: `npm run typecheck`
3. Ensure lint passes: `npm run lint`
4. Update documentation if needed
5. Request review from maintainers

## Provider Guidelines

When adding provider examples:

1. Keep them in `examples/` not `src/`
2. Document required environment variables
3. Handle errors gracefully
4. Support both streaming and non-streaming where applicable

## Questions?

Open an issue for questions or discussions.
