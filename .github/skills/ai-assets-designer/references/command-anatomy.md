# Command Anatomy – Command / Stored Prompt Design Reference

Target path per harness: see the Harness Storage Mapping in `SKILL.md`. This is the asset type with the **largest** frontmatter divergence between harnesses — read both blocks before drafting.

## Frontmatter — Claude Code (slash command)

```yaml
---
description: 1 sentence, human-facing hint of what to provide
argument-hint: what the human should type after the command (omit if no input expected)
model: <resolved tier alias, optional — omit to inherit the caller's model>
---
```

No `agent:` field. It runs directly in whichever loop invokes it — there is no routing to a separate named persona. If the work genuinely needs an isolated persona or its own tool boundary, that's a sign the request is actually an **Agent** (re-check the Asset Type Classification in `SKILL.md`), not a Command.

## Frontmatter — GitHub Copilot (stored prompt)

```yaml
---
name: 'kebab-case-action-name'
description: 'Human-facing hint, phrased as a question or imperative'
agent: 'Exact Agent Display Name'
model: '<resolved tier alias, optional override>'
argument-hint: 'what to provide, optional'
---
```

`agent:` is **required** — a Copilot stored prompt always routes to one existing agent by its exact display name; the prompt itself carries no judgment beyond that routing. If no suitable agent exists yet, that's a gap to raise, not a reason to skip the field.

## Body

- Imperative instructions the invoking loop (Claude Code) or routed agent (Copilot) follows directly
- Claude Code: `$ARGUMENTS` placeholder wherever free-form input should land, or omit entirely for no input
- Copilot: `${input:ParamName}` for named parameters, or a trailing open sentence for free-form input
- Push anything shared with other commands or agents into a skill or rule file and reference it by name; do not duplicate it inline

## No Size Limit — But Not License to Inline

The body is unlimited because it's "user-like" input, read once per invocation. That budget covers instructions specific to this one command — not shared boilerplate. If the same paragraph would appear in two commands, it belongs in a skill or rule instead.

## Self-Check (in addition to core-principles.md)

- [ ] Frontmatter matches the active harness's shape above, not the other one (`agent:` present for Copilot, absent for Claude Code)?
- [ ] Copilot only: `agent` names an agent that actually exists?
- [ ] Shared instructions extracted to a skill/agent/rule, not duplicated?
- [ ] Input placeholder used correctly for the active harness (`$ARGUMENTS` vs `${input:Name}`/trailing sentence), or omitted if none is needed?
- [ ] `argument-hint` present whenever the human must supply something specific?
