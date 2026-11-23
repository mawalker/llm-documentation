# Agents

## What an Agent Is (vs. a single prompt)
- The agent loop: perceive → plan → act → observe → repeat
- Autonomy spectrum: single-shot → tool-use → multi-step → multi-agent
- Why you'd build an agent (and when you shouldn't)

## Core Agent Patterns
- ReAct (reason + act): Yao et al. 2022, [arXiv:2210.03629](https://arxiv.org/abs/2210.03629)
- Plan-and-execute: popularised by LangChain / BabyAGI (2023); draws on Plan-and-Solve prompting (Wang et al. 2023, [arXiv:2305.04091](https://arxiv.org/abs/2305.04091))
- Reflexion (self-critique loop): Shinn et al. 2023, [arXiv:2303.11366](https://arxiv.org/abs/2303.11366)
- Multi-agent: orchestrator/worker, debate, pipeline
- Subagents: delegate a sub-task to a fresh context and get back only the result, which keeps the main context small
- Tool-use / function-calling as the "hands" of an agent (see [Tools](../tools/tools.md))

## Agent Architecture Anatomy
- System prompt as the agent's "personality + rules"
- Project instructions: `AGENTS.md` and similar files (see [Project Instruction Files](../project-instruction-files/project-instruction-files.md))
- Memory: short-term (context window) vs. long-term (external store)
- Tool registry: what actions the agent can take
- Guardrails: max iterations, tool allow-lists, human-in-the-loop gates
- State and persistence across sessions

## Building an Agent in Each Harness
- Pi: no built-in subagents or plan mode. Build them as extensions (custom tools, event hooks, commands) or install a Pi package that provides them
- opencode: define agents in config (prompt, permissions, model, `steps`); use the built-in build/plan agents and general/explore subagents
- Hermes Agent: built-in subagents and scheduled (cron) runs; configure toolsets, approvals and skills; Python scripts can call tools over RPC

## Operationalizing Agents
- Observability: tracing each step, logging tool calls, cost accounting
- Failure handling: retries, fallback models, graceful degradation
- Testing: unit-test individual tools, integration-test the full loop, eval suites
- Security: sandboxing tool execution, secret management, permission scoping
- Verify results: an agent's report that it succeeded is not proof. Check the actual files, output or state (see the example in [Prompting](../prompting/prompting.md))
