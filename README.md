# @compilr-dev Examples

```
      \|/
    ╭══════════╮     ___  ___  _ __ ___  _ __ (_) |_ __
    ║'  ▐▌  ▐▌ │    / __|/ _ \| '_ ` _ \| '_ \| | | '__|
    ║          │   | (__| (_) | | | | | | |_) | | | |
    ╰─═──────═─╯    \___|\___/|_| |_| |_| .__/|_|_|_|
      \________\                        | | .dev
                                        |_| examples
```

> Example projects using the compilr.dev agent libraries

> [!WARNING]
> These libraries are in beta. APIs may change between minor versions.

This repository contains example projects demonstrating how to use [@compilr-dev/agents](https://www.npmjs.com/package/@compilr-dev/agents) and [@compilr-dev/agents-coding](https://www.npmjs.com/package/@compilr-dev/agents-coding).

## Examples

| Example | Description |
|---------|-------------|
| `basic-agent` | Minimal agent with streaming output |
| `multi-llm` | Switch between Claude, OpenAI, Gemini, Ollama |
| `custom-tools` | Define and register custom tools |
| `sub-agents` | Spawn specialized sub-agents for tasks |
| `context-management` | Token budgeting and compaction |
| `coding-assistant` | Git, testing, and code search tools |

## Quick Start

```bash
# Clone the repo
git clone https://github.com/compilr-dev/examples.git
cd examples

# Install dependencies
npm install

# Set your API key (or use @compilr-dev/cli's /keys command)
export ANTHROPIC_API_KEY="sk-ant-..."

# Run an example
npx ts-node basic-agent/index.ts
```

## Example: Basic Agent

```typescript
import { Agent, ClaudeProvider } from '@compilr-dev/agents';

const agent = new Agent({
  provider: new ClaudeProvider({ apiKey: process.env.ANTHROPIC_API_KEY }),
  systemPrompt: 'You are a helpful assistant.',
});

for await (const event of agent.run('Hello!')) {
  if (event.type === 'text') {
    process.stdout.write(event.content);
  }
}
```

## Example: Multi-LLM

```typescript
import {
  Agent,
  ClaudeProvider,
  OpenAIProvider,
  GeminiProvider,
  OllamaProvider
} from '@compilr-dev/agents';

// Choose your provider
const providers = {
  claude: new ClaudeProvider({ apiKey: process.env.ANTHROPIC_API_KEY }),
  openai: new OpenAIProvider({ apiKey: process.env.OPENAI_API_KEY }),
  gemini: new GeminiProvider({ apiKey: process.env.GOOGLE_API_KEY }),
  ollama: new OllamaProvider({ model: 'llama3' }),
};

const agent = new Agent({
  provider: providers[process.argv[2] || 'claude'],
  systemPrompt: 'You are a helpful assistant.',
});
```

## Example: Custom Tools

```typescript
import { Agent, ClaudeProvider, defineTool } from '@compilr-dev/agents';

const weatherTool = defineTool({
  name: 'get_weather',
  description: 'Get current weather for a location',
  parameters: {
    type: 'object',
    properties: {
      location: { type: 'string', description: 'City name' },
    },
    required: ['location'],
  },
  execute: async ({ location }) => {
    // Your weather API call here
    return { temperature: 72, condition: 'sunny', location };
  },
});

const agent = new Agent({
  provider: new ClaudeProvider({ apiKey: process.env.ANTHROPIC_API_KEY }),
  tools: [weatherTool],
  systemPrompt: 'You are a weather assistant.',
});
```

## Requirements

- **Node.js** 20 or higher
- **API Key** for your chosen provider

## Related Packages

- [@compilr-dev/agents](https://www.npmjs.com/package/@compilr-dev/agents) - Core agent library
- [@compilr-dev/agents-coding](https://www.npmjs.com/package/@compilr-dev/agents-coding) - Coding-specific tools
- [@compilr-dev/cli](https://www.npmjs.com/package/@compilr-dev/cli) - AI-powered CLI assistant

## Links

- [Website](https://compilr.dev)
- [Documentation](https://compilr.dev/docs)
- [GitHub Issues](https://github.com/compilr-dev/examples/issues)

## License

[FSL-1.1-MIT](https://fsl.software/) - See [LICENSE](LICENSE) for details. Converts to MIT after 2 years per version.

---

<p align="center">
  <strong>Built with care by <a href="https://compilr.dev">compilr.dev</a></strong>
</p>
