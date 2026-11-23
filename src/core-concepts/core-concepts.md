# Core Concepts
- How the model works conceptually (pattern prediction, training, inference)
- Context windows and memory — what the model sees, what it forgets (more in [Context Engineering](../context-engineering/context-engineering.md))
- Sampling settings in plain terms: temperature, top-p, max tokens
- Why outputs vary and how to make results more predictable
- What LLMs are good at vs. poor at

## Reasoning ("Thinking") Models
- Models that reason internally before answering; slower and costlier, better on multi-step problems
- Controls: effort levels and thinking budgets
  - Pi: `defaultThinkingLevel` (off → max)
  - opencode: model variants for reasoning effort or token budgets
  - Hermes Agent: `--reasoning`
- When to use them, and when a fast model is enough
- Prompting changes: skip "think step by step" — OpenAI's guidance is to avoid chain-of-thought prompts for reasoning models ([reasoning best practices](https://platform.openai.com/docs/guides/reasoning-best-practices))
