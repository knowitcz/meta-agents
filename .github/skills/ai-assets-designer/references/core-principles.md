# Core Principles – Shared AI Asset Design Procedure

These apply to every asset type crafted by `design-ai-assets`. Per-type files layer their own anatomy and checklist on top — they never restate these.

## Single Responsibility

Name the asset's job in 2–3 words. If it needs a conjunction ("and", "then", "also") to describe → split it before designing either half.

## Minimal Necessary Content

| Belongs in the asset itself | Belongs elsewhere |
|---|---|
| Identity, decision rules, output contract | Reusable domain knowledge → a **skill** |
| Tool/model boundary (Agent only) | Project-wide conventions → the **rule** file (`CLAUDE.md` / `copilot-instructions.md`) |
| What triggers it | Templates shared across assets → a **skill** |

## Candidate Table (Flow Step 1)

```markdown
| Existing asset | Relationship | Recommendation |
|---|---|---|
| [name] | [overlap / extension / split target / unrelated] | [extend / split / create new / no change] |
```

## Socratic Design Questions (ask one at a time; do not batch)

1. **Job** — what is the one thing this asset does?
2. **Reuse** — does this already belong inside an existing asset instead of a new one?
3. **Trigger** — who or what invokes it, and when?
4. **Tools/access** — (Agent only) the minimum tool grant, not "whatever might be needed"
5. **Model** — (Agent/Command only) see Model Tier Resolution below
6. **Output contract** — what does it produce, in what format, for whom?

## Shared Self-Check

- [ ] Job nameable in 2–3 words without a conjunction?
- [ ] No domain knowledge inlined that belongs in a skill?
- [ ] No project-wide convention duplicated that belongs in the rule file?
- [ ] Output contract stated?
- [ ] Within the type's limit (see the Asset Type Classification in `SKILL.md`)?
- [ ] Frontmatter/path match the active harness (see the Harness Storage Mapping in `SKILL.md`), not assumed from habit?

## Model Tier Resolution

Model names go stale fast and differ per harness (Claude Code, Codex, Copilot all expose different rosters). Reason in tiers, not vendor names, and only resolve to a concrete name at design time:

| Tier | Shape of task |
|---|---|
| Fast / bounded | Simple, deterministic, low-ambiguity (retrieval, formatting, classification) |
| Standard | Everyday reasoning + code, most agents |
| Deep reasoning | Architecture, ambiguous or unbounded problems |
| Frontier / maximum | Rare — ultra-complex, cross-cutting synthesis |

Treat this as a sliding scale — some harnesses expose two tiers, some more. Resolve in this order, stopping at the first that applies:

1. The human already specified a model → use it.
2. The current session/environment already names its live roster in context → use that.
3. Other existing agent/command files in the active harness's own asset directories (see the Harness Storage Mapping in `SKILL.md`) already reference a model → match that convention (e.g. Claude Code's short aliases vs Copilot's full display names — see the per-type anatomy files).
4. Reachable harness documentation names the current roster → use it, but flag the answer as time-of-design, not guaranteed current.
5. None of the above → **ask the human**, proposing the tier-appropriate guess for confirmation. Never silently commit a guessed name.

Never let a resolved vendor/version name leak into shared skill content (this file, the anatomy files) — it only ever belongs in the specific output artifact being crafted, where staleness is cheap to fix per file.

## Refusal Template

```
I can't make that change as requested — it would violate [principle] by [specific reason].

Consequence: [what breaks or becomes harder as a result].

What I can do instead: [alternative that achieves the underlying goal].
```

Do not implement the violation, not even partially or "temporarily" — a temporary violation becomes the baseline.

## Observability & Git Discipline

- Every produced or modified asset file is expected to be git-tracked. Treat the working-tree diff as the review surface — present it and rely on the human's normal commit flow. Do not commit on the human's behalf unless separately instructed to.
- If you hit a difficulty (missing tool, incomplete task, unclear instruction) — say so, rather than guessing silently, so a systematic fix can be found.
