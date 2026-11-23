# Tools

## What a Tool Is
- An action the model can ask the harness to perform: a name, a description, and a parameter schema (usually JSON Schema)
- The model decides when to call a tool; the harness runs it and returns the result to the model
- Also called "function calling"

## Tool Anatomy
- Metadata: name, description (the model reads this to decide when to call it), parameter schema
- Implementation: the executable logic (script, API call, DB query, file operation)
- Lifecycle: registration → model calls it → harness validates input → execution → result back to the model → error reporting
- Conventions: timeouts; idempotent vs. side-effectful; streaming partial results

## Defining Tools in Each Harness
- Pi: `pi.registerTool({ name, description, parameters: Type.Object(...), execute })` in an extension
- opencode: TypeScript plugins (`@opencode/plugin`)
- Hermes Agent: built-in toolsets (`hermes tools`), plugins
- All three: tools from MCP servers (see [MCP](../mcp/mcp.md))

## Tool Quality and Safety
- Descriptions the model can interpret without ambiguity
- Parameter validation, and error messages the model can act on
- Strict schemas: OpenAI `strict: true` and Anthropic strict tool use guarantee that tool inputs match the schema
- Sandboxing: network access, filesystem scope, credential isolation
- Cost control: every tool schema costs tokens on every request; rate-limit expensive calls
- Audit logging: record every call with its input and output
