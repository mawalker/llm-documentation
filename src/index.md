# llm-documentation

Documentation for LLMs 

---

## Full Revised Outline

## Foundations
- What an LLM is (plain-language; no code required)
- How it differs from traditional software (probabilistic, generative, non-deterministic)
- Key vocabulary: tokens, context window, prompt, completion, model, temperature, embeddings, harness, agent, skill, tool
- Where LLMs fit in the broader AI landscape

## Core Concepts
- How the model works conceptually (pattern prediction, training, inference)
- Context windows and memory — what the model sees, what it forgets
- Sampling settings in plain terms: temperature, top-p, max tokens
- Why outputs vary and how to make results more predictable
- What LLMs are good at vs. poor at

## Prompting
- What makes a good prompt
- Common patterns: instructions, few-shot, role framing, output formatting
- Iterating and debugging prompts
- Chaining and decomposition
- Anti-patterns

## Structured and Reliable Output
- Requesting JSON, tables, machine-readable formats
- Validation and error handling
- Determinism strategies
- Post-processing

---

## LLM Harnesses *(new major section)*

### What a Harness Is
- Definition: the runtime layer that wraps a model — manages the loop, context, tool dispatch, streaming, session state
- Why you need one (vs. raw API calls): orchestration, persistence, multi-turn state, tool execution, safety rails
- Anatomy of a harness: agent loop, context manager, tool registry, provider adapter, config layer, extension/skill system
- How a harness differs from an "agent framework" vs. a "chatbot backend" vs. a "RAG pipeline"

### Choosing a Harness
- Comparison dimensions: extensibility, model-agnosticism, deployment target (terminal / server / embedded), language, community, license
- When to use a harness vs. raw SDK
- Decision matrix for the three tools below

### Pi
- *Overview*
  - What it is: minimalist, extensible terminal-based AI agent (TypeScript)
  - Design philosophy: small core, maximal extensibility, provider-agnostic
  - Key differentiators vs. opencode / hermes (footprint, extension model, config simplicity)
- *Setup Guide*
  - Prerequisites: Node.js ≥ 18, an API key (Anthropic / OpenAI / local model endpoint)
  - Install: `npm install -g pi-agent` (or `bun install -g pi-agent`)
  - First run: `pi` in a project directory; interactive REPL
  - Configuration: `pi.config.ts` — model selection, provider base-URL, system prompt override, max tokens
  - Provider setup examples:
    - Anthropic (API key via env or config)
    - OpenAI (API key, model ID)
    - Local model via Ollama / LM Studio (OpenAI-compatible endpoint)
  - Verifying the install: run a simple one-shot prompt, confirm streaming
- *Extending Pi*
  - Extension anatomy: a TypeScript module exporting a named skill/tool
  - Writing your first custom tool (e.g., a `fetchUrl` function the model can call)
  - Loading extensions: project-level (`./pi/extensions/`) vs. global
  - Hot-reload behavior during a session

### opencode
- *Overview*
  - What it is: open-source, multi-model terminal AI coding agent (by SST)
  - Design philosophy: provider-agnostic, multi-agent support, built-in tool suite, TUI-first
  - Key differentiators: native MCP support, agent "modes" (build/plan/ask), built-in file/code tools
- *Setup Guide*
  - Prerequisites: Go 1.21+ (or prebuilt binary), an API key or OAuth provider
  - Install options:
    - One-liner: `curl -fsSL https://opencode.ai/install | bash`
    - npm: `npm install -g opencode-ai`
    - Homebrew: `brew install sst/tap/opencode`
  - First run: `opencode` in a project; TUI launches, model picker
  - Authentication:
    - `opencode auth login` (OAuth for Anthropic / OpenAI / Google)
    - Or set `ANTHROPIC_API_KEY` / `OPENAI_API_KEY` env vars
    - Local models: configure `baseURL` in config
  - Configuration: `opencode.json` (project) or `~/.config/opencode/config.json` (global)
    - Model defaults, temperature, max steps
    - Custom agent definitions (name, system prompt, allowed tools, model)
    - MCP server registration
  - Verifying the install: ask it to list files, run a simple code task
- *Extending opencode*
  - Built-in tools available out of the box (read/write/edit, bash, web search, etc.)
  - Defining custom tools via MCP (Model Context Protocol) servers
  - Writing a minimal MCP server (Node or Python) and registering it in config
  - Agent profiles: create a "reviewer" agent vs. a "coder" agent with different prompts/toolsets

### Hermes
- *Overview*
  - What it is: LLM harness focused on structured agent workflows and skill orchestration
  - Design philosophy: skill-first architecture, declarative agent definitions, pipeline-oriented
  - Key differentiators: emphasis on composable "skills" as first-class units; workflow/pipeline primitives; lighter on TUI, heavier on programmatic use
- *Setup Guide*
  - Prerequisites: Python ≥ 3.11 (or Rust toolchain for native builds), model API key
  - Install:
    - PyPI: `pip install hermes-agent`
    - Source: `git clone … && cd hermes && cargo build --release`
  - First run: `hermes init` (scaffold config + skill directory)
  - Configuration: `hermes.toml`
    - Provider block (name, model, base_url, api_key env var)
    - Default agent block (system prompt, max iterations, allowed skills)
    - Logging and tracing settings
  - Model setup examples (same pattern: Anthropic, OpenAI, Ollama)
  - Verifying the install: `hermes run "summarize this file"` against a test doc
- *Extending Hermes*
  - Skill anatomy: a skill = declarative metadata (name, description, parameters schema) + executable handler
  - Writing a skill (Python handler example: a `translate` skill, a `query_db` skill)
  - Loading skills: project `./skills/` directory vs. shared package
  - Composing skills into a pipeline (e.g., fetch → extract → summarize → write)
  - Declarative agent definitions in `hermes.toml`: bind a named agent to a subset of skills + model + prompt

### Side-by-Side Comparison Table

| Dimension | Pi | opencode | Hermes |
|---|---|---|---|
| Primary target | Terminal REPL, extensibility | Terminal TUI, multi-agent | Programmatic pipelines, skill composition |
| Language | TypeScript | Go (binary) / TS extensions | Python / Rust |
| Extension model | TS modules | MCP servers, built-in tools | Declarative skills + pipelines |
| Multi-model | ✅ | ✅ | ✅ |
| Config format | TS file | JSON | TOML |
| Best for | Custom terminal workflows | Interactive coding, multi-agent | Structured batch/agent pipelines |

---

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

---

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

---

## Advanced Patterns (revised from old §5)
- RAG: what it is, when to use it, basic architecture (embedding store, retriever, context injection)
- Fine-tuning vs. prompting vs. RAG — decision framework
- Function/tool calling (now referenced to §7 for the "how")
- Multi-agent orchestration (now referenced to §6)
- Streaming, batching, caching

## Practical Integration
- Calling a harness from a backend service (HTTP API, CLI subprocess, SDK)
- Authentication, rate limits, token accounting
- Cost and latency modeling
- Deployment options: local terminal, internal server, CI pipeline, embedded in an app

## Quality, Safety, and Evaluation
- Evaluating agent/skill quality (task success rate, step efficiency, hallucination rate)
- Failure modes: loop, tool misuse, context overflow, prompt injection via retrieved content
- Guardrails: iteration caps, tool allow-lists, output filters, human approval gates
- PII, confidentiality, compliance
- Logging, monitoring, audit trails

## Ethics and Responsible Use
- Disclosure
- Bias and limitations
- Legal / organizational policy
- User feedback and escalation

## Operational Concerns
- Versioning harnesses, skills, and models
- Fallbacks and degradation
- Testing strategy: unit (skill) → integration (agent loop) → e2e (user task)
- Rollout: shadow mode, canary, human-in-the-loop

## Appendices
- Glossary (updated with harness/agent/skill terms)
- Model comparison cheat-sheet
- Harness comparison cheat-sheet (§5.6 expanded)
- Prompt templates
- Skill templates (one per harness)
- Troubleshooting checklist
- Setup quick-reference cards (one page per harness: install → config → verify → extend)

---

## A Few Structural Notes

- **Prerequisites box at the top of §5.3–5.5**: each setup guide should open with a one-line "you need X, Y, Z" so readers can self-select whether they're ready.
- **Runnable examples**: every setup section should end with a "verify it works" snippet (a command they paste and the expected output). This is the single most common source of confusion when people follow setup docs.
- **Cross-reference map**: a small table early in the doc mapping the terminology (tool / skill / extension / MCP server / function) to what each harness calls the same concept, so readers don't get lost in jargon switches.
- **Progressive disclosure**: §5.1–5.2 for everyone; §5.3–5.5 for the reader who picks a harness; §6–7 for those building agents. Non-technical readers can stop after §4 and still be productive.

Want me to draft the full setup-guide text for any one of the three harnesses, or flesh out the skill-writing worked examples?