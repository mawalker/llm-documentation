## Skills *(new major section)*

### What a Skill Is
- Definition: a reusable, self-contained unit of capability the agent can invoke
- Relationship: model (brain) → harness (body) → skills (hands/senses)
- Skill vs. tool vs. function call vs. MCP server (terminology mapping)

### Skill Anatomy (General)
- Metadata: name, description (the model reads this to decide when to use it), parameter schema, return type
- Implementation: the executable logic (script, API call, DB query, file operation)
- Lifecycle: load → schema registration → invocation → result formatting → error reporting
- Conventions: idempotency, timeout, idempotent vs. side-effectful, streaming support

### Writing Skills — Worked Examples (per harness)
- Pi: TypeScript extension with a typed function + docstring (model reads docstring)
- opencode: MCP server exposing a tool (JSON-RPC, input/output schema)
- Hermes: TOML metadata block + Python handler function with type hints
- For each: a concrete example (e.g., "search internal docs" skill, "create a Jira ticket" skill)

### Managing a Skill Library
- Naming conventions and versioning
- Discoverability: how the model picks the right skill (description quality matters)
- Packaging and sharing: publishing to a registry, internal npm/pip/cargo package
- Deprecation and backward compatibility

### Skill Quality and Safety
- Writing descriptions the model can actually interpret (avoid ambiguity)
- Parameter validation and error messages the model can act on
- Sandboxing: network access, filesystem scope, credential isolation
- Cost control: token overhead of skill schemas, rate-limiting expensive calls
- Audit logging: every skill invocation recorded with input/output