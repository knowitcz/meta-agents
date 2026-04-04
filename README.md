# AI Meta

This repository contains the meta layer for designing AI customizations: agents, skills, prompts, and instructions.

The core idea is simple: keep every artifact small, single-purpose, and easy for LLMs to load without wasting context.

## Entry Points

- [Meta prompt](.github/prompts/meta.prompt.md) - the starting point for asking about agents, skills, prompts, or instructions.
- [Workspace instructions](.github/copilot-instructions.md) - project-wide rules for how customization artifacts should be shaped.
- [Meta agents](.github/agents/) - specialized designers for agents, skills, instructions, and prompts.

## Design Rules

- One artifact = one responsibility.
- Prefer short, focused text over broad, all-purpose guidance.
- Put reusable domain knowledge in skills.
- Put project-wide behavior in instructions.
- Put reusable workflows in prompts.
- Put named personas and tool access in agents.

If you want to see the structure in more detail, open the agent definitions in [`.github/agents/`](.github/agents/).
