# Context Engineering

## Why Context Matters
- The model only knows what is in its context window on each request
- Quality can drop as context grows, even well below the limit ([Chroma, "Context Rot"](https://research.trychroma.com/context-rot))

## Managing Context in a Harness
- **Compaction**: summarising older parts of the conversation to free space
  - Pi: automatic compaction; opencode: compaction settings; Hermes Agent: `/compress`
- **Subagents**: run a sub-task in a fresh context and return only the result
- **Progressive disclosure**: load skill instructions only when needed (see [Skills](../skills/skills.md))
- Keep tool and MCP schemas lean; each one costs tokens on every request

## Prompt Caching
- Providers can cache a repeated prompt prefix, which cuts cost and latency
- Example: on Anthropic, cache reads cost about 10% of the normal input price on most models, and entries last 5 minutes by default ([docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching))
- Keep the stable parts (system prompt, tools, project instructions) at the start, and changing content at the end
- opencode documents caching options under Models
