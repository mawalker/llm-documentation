## Agents *(new major section)*

### What an Agent Is (vs. a single prompt)
- The agent loop: perceive → plan → act → observe → repeat
- Autonomy spectrum: single-shot → tool-use → multi-step → multi-agent
- Why you'd build an agent (and when you shouldn't)

### Core Agent Patterns
- ReAct (reason + act)
- Plan-and-execute
- Reflexion (self-critique loop)
- Multi-agent: orchestrator/worker, debate, pipeline
- Tool-use / function-calling as the "hands" of an agent

### Agent Architecture Anatomy
- System prompt as the agent's "personality + rules"
- Memory: short-term (context window) vs. long-term (external store)
- Tool registry: what actions the agent can take
- Guardrails: max iterations, tool allow-lists, human-in-the-loop gates
- State and persistence across sessions

### Building an Agent in Each Harness
- Pi: define a custom loop in an extension, register tools, set iteration limits
- opencode: define named agents in config, assign tool subsets, use built-in multi-agent mode
- Hermes: declarative agent in TOML, bind skills, set pipeline steps, configure human-approval gates

### Operationalizing Agents
- Observability: tracing each step, logging tool calls, cost accounting
- Failure handling: retries, fallback models, graceful degradation
- Testing: unit-test individual skills, integration-test the full loop, eval suites
- Security: sandboxing tool execution, secret management, permission scoping