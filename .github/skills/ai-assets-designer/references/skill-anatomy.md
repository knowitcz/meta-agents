# Skill Anatomy – Skill Design Reference

Target path per harness: see the Harness Storage Mapping in `SKILL.md` — `<root>/skills/<kebab-name>/SKILL.md` (+ optional `references/*.md`, `scripts/*`). This is the asset type with the least divergence between harnesses: same frontmatter, same file shape, only the root directory (`.claude/` vs `.github/`) differs.

## Frontmatter — Claude Code and GitHub Copilot (identical)

```yaml
---
name: 'kebab-case-name'
description: 'Trigger description with concrete trigger phrases, and a DO NOT USE FOR clause when a neighboring skill could be confused with this one.'
---
```

Only `name` + `description` — no `model` or `tools` field, in either harness. A skill has no privilege boundary of its own; it inherits whichever agent loaded it. That is exactly why model resolution (core-principles.md) never happens inside a skill file — only inside the Agent/Command it's eventually used to craft.

## Body Structure

1. `# Title – Subtitle`
2. One sentence: purpose + who consumes it
3. `## When to Use` — 3–6 bullets
4. Domain content — numbered `Flow` steps for a procedure, or reference tables for static knowledge
5. `## Error Handling` — situation → action table (procedural skills only)

## references/ — Progressive Disclosure

- Move dense tables, templates, and per-branch detail here; link with `[references/x.md](references/x.md)`
- SKILL.md itself must stay **≤120 lines** — it's read whole, every time the skill loads
- references/ files should still be readable in one call — if one grows unwieldy, split it further rather than letting it balloon
- Executable helpers belong in `scripts/`, not inlined as code blocks

## Self-Check (in addition to core-principles.md)

- [ ] Description carries concrete trigger phrases, not a generic label?
- [ ] SKILL.md ≤120 lines — dense data moved to `references/`?
- [ ] Steps are imperative commands, not prose explanations?
- [ ] `When to Use` present (and `Error Handling` for procedural skills)?
- [ ] No `model`/`tools` field smuggled into frontmatter?

## Anti-Patterns

| Anti-pattern | Fix |
|---|---|
| All rules in SKILL.md, no references | Extract tables/templates/schemas to `references/` |
| Description is one generic word | Write 1–3 sentences with specific trigger phrases |
| No "When to Use" section | Add 3–6 bullet trigger conditions |
| Skill covers two unrelated domains | Split into two skills |
| Agent-specific orchestration logic in a skill | Move to the agent file instead |
| Copy-pasting project conventions | Reference the project's rule file (`CLAUDE.md` / `copilot-instructions.md`) instead of duplicating |
| Missing negative triggers | Add "DO NOT USE FOR:" when a neighboring skill exists |
