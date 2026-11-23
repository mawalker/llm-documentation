# Skills

## What a Skill Is
- Definition: a folder containing a `SKILL.md` file (name and description in frontmatter, then instructions), plus optional scripts, reference docs and assets. It packages know-how for one kind of task
- The format is the open [Agent Skills specification](https://agentskills.io/specification). Pi implements it, Hermes Agent is compatible with it, and opencode uses `SKILL.md` folders through its `skill` tool
- Progressive disclosure: the model sees only each skill's name and description until a task calls for it; then it loads the full instructions. Many skills cost few tokens until used
- Relationship: model (brain) → harness (body) → tools (hands) → skills (playbooks for using them)

## Terminology Map
- **Tool / function**: an action the model can call, with a parameter schema (see [Tools](../tools/tools.md))
- **MCP server**: a separate process or remote service that provides tools (see [MCP](../mcp/mcp.md))
- **Extension / plugin**: code that changes the harness itself (Pi extensions, opencode plugins, Hermes plugins)
- **Skill**: instructions and supporting files the model reads when relevant
- Cross-reference table: what each harness calls each of these

## Skill Anatomy
- `SKILL.md` frontmatter: `name` (lowercase, hyphens), `description` (the model reads this to decide when to load the skill)
- Body: direct, step-by-step instructions
- Optional folders: `scripts/`, `references/`, `assets/`
- Where they live:
  - Pi: global and project skill directories (see Pi docs)
  - opencode: skill directories, loaded with the `skill` tool
  - Hermes Agent: `~/.hermes/skills/`

## Writing Skills — Worked Examples
- One example skill per harness (e.g. "release checklist", "write a migration in our house style")
- Self-authored skills: Hermes Agent creates and refines skills from experience

## Managing a Skill Library
- Naming conventions and versioning
- Discoverability: how the model picks the right skill (description quality matters)
- Packaging and sharing: Pi packages (npm / git), skill hubs, internal repos
- Deprecation and backward compatibility

## Skill Quality and Safety
- Writing descriptions the model can actually interpret (avoid ambiguity)
- Keep `SKILL.md` lean; move long reference material into separate files
- **Supply-chain risk**: a third-party skill can include scripts that run with the agent's permissions. Read skills before installing them, and treat skill hubs like package registries
- Cost control: every installed skill's description sits in context
- Audit: track which skills are installed and where they came from
