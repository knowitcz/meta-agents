# AI Meta

This repository contains the meta layer for designing AI customizations: agents, skills, commands, and rules.

The core idea is simple: keep every asset small, single-purpose, and easy for LLMs to load without wasting context.

## Entry Point

Everything lives in one skill:

- [`ai-assets-designer`](.github/skills/ai-assets-designer/SKILL.md) - classifies a requested capability into the right asset type, then runs a single shared design procedure: assess existing assets → classify → challenge intent → resolve model → design → self-check → save.

Supporting references, loaded only when the flow needs them:

- [core-principles.md](.github/skills/ai-assets-designer/references/core-principles.md) - shared procedure: single responsibility, Socratic design questions, model tier resolution, refusal template.
- [agent-anatomy.md](.github/skills/ai-assets-designer/references/agent-anatomy.md) - agent frontmatter, body structure, tool boundary, anti-patterns.
- [skill-anatomy.md](.github/skills/ai-assets-designer/references/skill-anatomy.md) - skill frontmatter, progressive disclosure via `references/`.
- [command-anatomy.md](.github/skills/ai-assets-designer/references/command-anatomy.md) - slash command / stored prompt frontmatter and body.
- [memory-anatomy.md](.github/skills/ai-assets-designer/references/memory-anatomy.md) - always-in-context rules and their scoping mechanisms.

## Asset Types

| Test | Asset type | Limit |
|---|---|---|
| Needs its own scope, model, or tool-privilege boundary? | Agent | ≤200 lines |
| Reusable knowledge/procedure loaded semi-dynamically? | Skill | Entry file ≤120 lines |
| A human invokes it directly, once per use? | Command | No hard limit |
| Expected to be (almost) always in context? | Rule | Extremely dense, no prose |

If a request satisfies more than one row, it becomes one asset per row.

## Harness Support

Classification is supplier-agnostic; storage paths and frontmatter are not. The skill carries a mapping for both Claude Code (`.claude/`) and GitHub Copilot (`.github/`), and asks which harness is active before reading or writing anything.

## Design Rules

- One asset = one responsibility.
- Prefer short, focused text over broad, all-purpose guidance.
- Put reusable domain knowledge in skills.
- Put project-wide behavior in rules.
- Put reusable workflows in commands.
- Put named personas and tool access in agents.
