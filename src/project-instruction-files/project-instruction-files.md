# Project Instruction Files

## What They Are
- Markdown files the harness adds to the model's context automatically: build commands, architecture notes, conventions, things to avoid
- `AGENTS.md` is the common name across harnesses; `CLAUDE.md` is Claude Code's name and is often read as a fallback

## Support in Each Harness
- Pi: global and per-folder `AGENTS.md`, with `CLAUDE.md` as a fallback
- opencode: `AGENTS.md`, with a `CLAUDE.md` fallback ([docs](https://opencode.ai/docs/instructions))
- Hermes Agent: `AGENTS.md` (walks up to the git root) and a personal `AGENTS.override.md`, usually gitignored

## Writing a Good One
- Short and specific: commands, paths, conventions
- Things the model gets wrong without being told
- Keep it current; stale instructions mislead
- Global (personal) vs. project (shared, committed) files
