# LLM Harnesses

> Setup details below were checked against upstream docs on 2026-11-23. These tools change often, so re-check before relying on exact commands.

## What a Harness Is
- Definition: the runtime layer that wraps a model. It manages the loop, context, tool dispatch, streaming and session state
- Why you need one (vs. raw API calls): orchestration, persistence, multi-turn state, tool execution, safety rails
- Anatomy of a harness: agent loop, context manager, tool registry, provider adapter, config layer, extension/skill system
- How a harness differs from an "agent framework" vs. a "chatbot backend" vs. a "RAG pipeline"

## Common Harness Features
- Permissions and approval modes: what the agent may do without asking (edit files, run shell commands, use the network)
- Sandboxing: containers, remote backends, restricted directories
- Hooks / lifecycle events: run your own code before or after tool calls, on session start, on compaction
  - Pi: extension events (`pi.on("tool_call", …)`); opencode: plugin hooks; Hermes: event hooks
- Sessions: resume, fork/branch, compaction
  - Pi: `pi --resume` / `/resume`, session fork; Hermes: `hermes --resume <id>`, `/compress`
- Project instruction files (`AGENTS.md`). See [Project Instruction Files](../project-instruction-files/project-instruction-files.md)

## Choosing a Harness
- Comparison dimensions: extensibility, model-agnosticism, deployment target (terminal / server / embedded), language, community, license
- When to use a harness vs. raw SDK
- The wider landscape, for orientation: Claude Code, Codex CLI ([openai/codex](https://github.com/openai/codex)), Gemini CLI ([google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli)), Cursor, plus the three covered below
- Decision matrix for the three tools below

## Pi
- *Overview*
  - What it is: a minimal, extensible agent harness written in TypeScript ([earendil-works/pi](https://github.com/earendil-works/pi), [pi.dev](https://pi.dev))
  - Design philosophy: small core, maximal extensibility, provider-agnostic. Deliberately ships **without** sub-agents or plan mode; add them through extensions or packages
  - Modes: interactive, print/JSON mode, RPC, and a TypeScript SDK
- *Setup Guide*
  - Prerequisites: Node.js **≥ 22.19** (the installer can install it). A `legacy-node20` npm tag exists for older Node
  - Install:
    - `curl -fsSL https://pi.dev/install.sh | sh` (pins dependencies; update with `pi update`)
    - or `npm install -g --ignore-scripts @earendil-works/pi-coding-agent`
    - (The old package name `@mariozechner/pi-coding-agent` is deprecated.)
  - First run: `pi` in a project directory, then `/login` to connect a subscription or API key
  - Configuration: `settings.json` in `~/.pi/agent/` (global) and `.pi/` (project). Project settings override global ones
    - e.g. `defaultProvider`, `defaultModel`, `defaultThinkingLevel`
  - Provider setup examples: Anthropic, OpenAI, local model via Ollama / LM Studio (OpenAI-compatible endpoint)
  - Verifying the install: run a simple one-shot prompt in print mode, confirm streaming
- *Extending Pi*
  - **Extensions** (TypeScript code): custom tools, commands, event hooks
    - Locations: `~/.pi/agent/extensions/` (global), `.pi/extensions/` (project, loads only after the project is trusted)
    - Quick test: `pi -e ./my-extension.ts`
    - Reload with `/reload` (auto-discovered locations only; not while the agent is running)
  - Writing your first custom tool: `pi.registerTool({ name, description, parameters: Type.Object(...), execute })`. The model reads the `description` and the TypeBox parameter schema
  - **Skills**: `SKILL.md` folders following the [Agent Skills spec](https://agentskills.io/specification)
  - **MCP**: built in (`pi mcp add …`, `/mcp`, `~/.pi/agent/mcp.json` / `.pi/mcp.json`)
  - **Pi packages**: bundle extensions, skills, prompt templates and themes; share via npm or git

## opencode
- *Overview*
  - What it is: open-source AI coding agent with a terminal UI and a desktop app ([anomalyco/opencode](https://github.com/anomalyco/opencode), formerly `sst/opencode`; [opencode.ai](https://opencode.ai))
  - Written in TypeScript (plus Rust); distributed as prebuilt binaries, so no Go or other toolchain is needed
  - Design philosophy: provider-agnostic, built-in tool suite, TUI-first
  - Two major versions: **V2 is current**. V1 commands (`opencode-ai` npm package, `opencode.ai/install`, the `sst/tap` / `anomalyco/tap/opencode` formula) still exist but conflict with V2. Use one or the other
- *Setup Guide (V2)*
  - Install options:
    - Script: `curl -fsSL https://opencode.ai/v2/install | bash`
    - npm: `npm i -g @opencode/cli`
    - Homebrew: `brew install anomalyco/tap/opencode-v2` (or `brew install opencode` from homebrew-core, updated less often)
  - First run: `opencode` in a project; TUI launches
  - Authentication:
    - `/connect` in the TUI, or `opencode auth login` (e.g. `opencode auth login anthropic --method key`)
    - Or set provider API keys as environment variables
    - Local models: Ollama, LM Studio, vLLM, or any OpenAI-compatible endpoint
  - Configuration: `opencode.json(c)` (project) or `~/.config/opencode/opencode.json(c)` (global). V2 ignores the old V1 `config.json`
    - Model defaults; reasoning effort / token budgets through model **variants**
    - Agent definitions (description, prompt, permissions, model, `steps`). The old agent fields `maxSteps` and `temperature` are legacy in V2
    - MCP server registration
  - Verifying the install: ask it to list files, run a simple code task
- *Extending opencode*
  - Built-in agents: **build** (default, full access) and **plan** (read-only) as primary agents; **general** and **explore** as subagents. Switch primary agents with `Tab`
  - Built-in tools: read, edit/write/patch, glob, grep, shell, webfetch, websearch, skill, subagents and more
  - Custom tools and hooks: TypeScript **plugins** (`@opencode/plugin`, `.opencode/plugins/`)
  - MCP servers: register in config
  - Skills: `SKILL.md` folders loaded with the `skill` tool
  - Agent profiles: e.g. a "reviewer" agent vs. a "coder" agent with different prompts and permissions

## Hermes Agent
- *Overview*
  - What it is: Nous Research's open-source, self-improving agent ([NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent), [docs](https://hermes-agent.nousresearch.com/docs/)). Not to be confused with Nous Research's *Hermes* model family
  - Written in Python
  - Design philosophy: a persistent personal agent that learns. It writes and improves its own skills, keeps memory across sessions, and runs anywhere (local, Docker, SSH, serverless backends)
  - Interfaces: full TUI, plus a messaging gateway (Telegram, Discord, Slack, WhatsApp, Signal, email), a cron scheduler and a desktop app
- *Setup Guide*
  - Install:
    - Linux / macOS / WSL2: `curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash`
    - Windows: `iex (irm https://hermes-agent.nousresearch.com/install.ps1)`
    - (`pip install hermes-agent` exists, but PyPI lags behind the current release.)
  - First run: `hermes setup` (setup wizard), then `hermes` to start chatting
  - Configuration: `~/.hermes/config.yaml` for settings, `~/.hermes/.env` for API keys
    - `hermes config get/set/edit`, `hermes model` to switch provider and model
  - Model setup examples: Nous Portal, OpenRouter, OpenAI, Anthropic, custom endpoints
  - Verifying the install: `hermes doctor`, then a one-shot prompt with `hermes chat -q "…"`
- *Extending Hermes*
  - Skills: `SKILL.md` folders in `~/.hermes/skills/`, compatible with the [agentskills.io](https://agentskills.io) standard. Hermes can create and refine skills itself
  - Tools: `hermes tools` to enable or disable toolsets
  - MCP servers: `hermes mcp`
  - Plugins: `hermes plugins`
  - Subagents for parallel work; Python scripts that call tools over RPC
  - Scheduled automations: `hermes cron`

## Side-by-Side Comparison Table

| Dimension | Pi | opencode | Hermes Agent |
|---|---|---|---|
| Primary target | Minimal terminal harness; SDK / RPC embedding | Terminal TUI coding agent (+ desktop app) | Persistent personal agent: TUI, messaging gateway, cron |
| Language | TypeScript | TypeScript (+ Rust) | Python |
| Extension model | TS extensions, Agent Skills, MCP, Pi packages | TS plugins, MCP servers, Agent Skills | Agent Skills (self-authored), plugins, MCP |
| Multi-model | ✅ | ✅ | ✅ |
| Config format | `settings.json` | `opencode.json(c)` | `config.yaml` + `.env` |
| Best for | Building your own workflow on a small core | Interactive coding with built-in agents | Long-running assistant across devices and chat apps |
