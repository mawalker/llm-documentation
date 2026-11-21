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