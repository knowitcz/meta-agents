# Memory / Rule Anatomy – Always-Available Instructions Design Reference

Target path per harness: see the Harness Storage Mapping in `SKILL.md`. This is the asset type with the biggest **scoping-mechanism** gap between harnesses, not just a frontmatter gap — read both models before drafting.

## Claude Code — Directory Scoping Only

- Root `CLAUDE.md`: no frontmatter, injected into every session anywhere in the repo
- Nested `CLAUDE.md` in a subdirectory: no frontmatter, injected only for sessions working within that subtree
- No glob-scoped primitive exists. A rule that should apply to "every file of type X, wherever it lives in the tree" has no exact native home on this harness.

## GitHub Copilot — Two Mechanisms

- `.github/copilot-instructions.md`: no frontmatter, project-wide, always injected
- `.github/instructions/<concern>.instructions.md`: glob-scoped via frontmatter —
  ```yaml
  ---
  applyTo: '<glob pattern, e.g. app/services/*.py>'
  description: 'Optional 1 sentence when the title alone is ambiguous'
  ---
  ```
  This is genuinely more precise than anything Claude Code offers: it scopes by file pattern regardless of directory.

## Crossing Harnesses

If you're designing on Claude Code but the actual need is glob-shaped (matches Copilot's `applyTo` model, not a directory), **say that explicitly** rather than faking coverage with a nested `CLAUDE.md` that only catches part of the tree. Don't silently port a Copilot `.instructions.md` file's `applyTo` scoping into a Claude Code rule and pretend the scoping still holds.

## Style — Extremely Dense (both harnesses)

Model this project's own root `CLAUDE.md`: flat bullets, imperative, no explanatory prose, tables only for genuine lookups (e.g. a routing table). This file (or its Copilot equivalent) is injected into (almost) every session — every line is a recurring cost paid by every future invocation, not a one-time read. The broader the scope (project-wide > glob-scoped > directory-scoped), the stricter the brevity bar.

## Necessity Gate

Every rule must pass all four:

1. **Actionable** — tells the agent to do or avoid something concrete
2. **Scoped correctly** — belongs at this level, not broader or narrower
3. **Non-obvious** — a competent agent would not already do this unprompted
4. **Unique** — not already stated at another level or inside a skill

A rule failing any test gets removed or relocated, not kept "just in case."

## Self-Check (in addition to core-principles.md)

- [ ] Every bullet passes the Necessity Gate?
- [ ] No prose explanation — imperative only, matching the existing style in this harness's rule file?
- [ ] Placed at the correct level — project-wide only if truly universal?
- [ ] On Claude Code, if the actual need is glob-scoped (not directory-scoped), is that gap stated rather than papered over?
- [ ] On Copilot, is `applyTo` the narrowest glob that actually covers the intended files?
