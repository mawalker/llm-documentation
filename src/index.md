# LLM Documentation

A structured guide to large language models: how they work, how to prompt them, and how to build with them using harnesses, agents, tools and skills.

## How This Guide Is Organised

- **Everyone:** [Foundations](foundations/foundations.md), [Core Concepts](core-concepts/core-concepts.md), [Prompting](prompting/prompting.md). Non-technical readers can stop here and still be productive.
- **Getting reliable results:** [Structured and Reliable Output](structured-and-reliable-output/structured-and-reliable-output.md), [Context Engineering](context-engineering/context-engineering.md)
- **Picking and setting up a harness:** [LLM Harnesses](llm-harnesses/llm-harnesses.md) (Pi, opencode, Hermes Agent), [Project Instruction Files](project-instruction-files/project-instruction-files.md)
- **Building agents:** [Agents](agents/agents.md), [Tools](tools/tools.md), [MCP](mcp/mcp.md), [Skills](skills/skills.md), [Local Models](local-models/local-models.md)
- **Going further:** [Advanced Patterns](advanced-patterns/advanced-patterns.md), [Practical Integration](practical-integration/practical-integration.md)
- **Running it responsibly:** [Quality, Safety, and Evaluation](quality-safety-and-evaluation/quality-safety-and-evaluation.md), [Ethics and Responsible Use](ethics-and-responsible-use/ethics-and-responsible-use.md), [Operational Concerns](operational-concerns/operational-concerns.md)
- **Reference:** [Appendices](appendices/appendices.md)

## Structural Notes (for writing the chapters)

- **Prerequisites box at the top of each harness setup guide:** a one-line "you need X, Y, Z" so readers can tell whether they're ready.
- **Runnable examples:** every setup section should end with a "verify it works" snippet (a command to paste and the expected output). This is the most common source of confusion when people follow setup docs.
- **Last-verified dates:** harnesses change monthly, so every setup section should say when its commands were last checked.
- **Cross-reference map:** a small table mapping the terminology (tool / skill / extension / plugin / MCP server / function) to what each harness calls the same concept, so readers don't get lost in jargon switches. A first version is in [Skills](skills/skills.md).
- **Progressive disclosure:** the chapter groups above go from general to specialised; readers can stop at the group that covers what they need.
