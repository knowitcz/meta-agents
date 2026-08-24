# Agent Anatomy – Agent Design Reference

Target path per harness: see the Harness Storage Mapping in `SKILL.md`. Limit ≤200 lines regardless of harness — an agent is loaded once per invocation, but carries a standing identity, so the ceiling is generous but bounded.

## Frontmatter — Claude Code

```yaml
---
name: kebab-case-name
description: 1-2 sentences — what it does and when to invoke it
model: <resolved tier alias, e.g. sonnet — see core-principles.md Model Tier Resolution>
tools: Read, Edit, Grep
---
```

`name` is both the file identity and the invocation name. `tools` is a comma-separated list of plain identifiers.

## Frontmatter — GitHub Copilot

```yaml
---
name: 'Display Name'
description: '1-2 sentences — what it does and when to invoke it'
model: '<resolved tier alias, e.g. Claude Opus 4.6>'
tools: ['namespaced/tool', 'another/tool']
user-invocable: true
---
```

File is `<kebab-case-name>.agent.md`; `name` is a human-facing display string, separate from the kebab-case filename. `tools` is a YAML list of namespaced identifiers (e.g. `search/codebase`, `edit/editFiles`) — check the harness's current tool registry rather than assuming Claude Code's plain names transfer. `user-invocable` (optional) states whether a human can address it directly by name.

## Shared Rule — Tool Boundary

Whichever harness, the `tools`/access field is a **hard boundary** — unlike a skill or command, which inherit the caller's tools, an agent literally cannot use a capability outside this list. Put access control here, not in a skill. Grant only what the job requires, and check the active harness's own existing agent files for the alias/tool-name convention already in use before picking one (Model Tier Resolution, step 3).

## Body Structure

1. **Identity line** — "You are the X. Your responsibility is…"
2. **Skill pointers** — "Read the `<skill>` skill for …" — reference domain knowledge, never inline it
3. **Decision rules** — what it evaluates, reviews, or produces, and how it decides
4. **Output format** — the exact contract its caller can rely on
5. **Constraints** — what it must NOT do (defers to which other agent, if any)

## When to Choose Agent Over Skill

- The work needs its own tool allowlist or model tier, distinct from whatever is calling it
- It has a standing identity invoked by name across many sessions, not a one-off procedure
- Its judgment must run in an isolated context window, separate from the orchestrating loop

If none of these hold, it's very likely a **skill** instead — re-check the Asset Type Classification in `SKILL.md`.

## Self-Check (in addition to core-principles.md)

- [ ] Tool/access list is minimal, not "whatever might be needed"?
- [ ] Domain reference data moved to a skill, not inlined?
- [ ] Output contract exact enough that its caller doesn't have to guess the format?
- [ ] File ≤200 lines?
- [ ] `model` matches the active harness's own convention (alias vs display name), resolved per core-principles.md, not guessed?
- [ ] Frontmatter matches the active harness's shape above, not the other one?

## Anti-Patterns

| Anti-pattern | Fix |
|---|---|
| Orchestrator + implementor combined | Split into two agents |
| Domain reference data inlined | Move to a skill |
| Project conventions copy-pasted | Reference the project's rule file (`CLAUDE.md` / `copilot-instructions.md`) or a skill instead |
| "Also does X, Y, Z" in the description | Extract each concern into its own agent |
| Vague description | Write a precise trigger condition |
| All tools listed "to be safe" | Allowlist only what's actually needed |
| Copilot frontmatter shape used in a Claude Code file, or vice versa | Match Frontmatter above for the active harness |
