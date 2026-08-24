---
name: 'ai-assets-designer'
description: 'Factory for crafting AI assets — subagents, skills, slash commands, and rules. Classifies a requested capability into the right artifact type via one routing test, then runs a single shared design procedure (assess existing → classify → challenge intent → design → self-check → save) with per-type anatomy and limits. Triggers on "create an agent", "add a skill", "new slash command", "add a rule to CLAUDE.md", "design an AI asset", "which artifact type should this be". DO NOT USE FOR: editing product code, refining issues, or auditing existing tech debt.'
---

# Design AI Assets – AI Asset Factory

This skill classifies a requested capability into the correct AI asset type, then runs one shared design procedure regardless of type or harness. Classification (below) is supplier-agnostic; where the resulting file actually lives, and what its frontmatter looks like, depends on which harness is in use — see the Harness Storage Mapping. Type-specific anatomy and self-checks live in `references/`.

## When to Use

Use this skill when:
- A human or agent wants to add or change an agent, skill, command, or rule — in Claude Code, GitHub Copilot, or another harness
- It's unclear which asset type a new capability should become
- An existing AI asset needs a lifecycle change (extend, split, merge, deprecate)

## Asset Type Classification

| Test | Asset type | Limit |
|---|---|---|
| Does it need its own scope, model, or tool-privilege boundary? | Agent | ≤200 lines — see [references/agent-anatomy.md](references/agent-anatomy.md) |
| Is it reusable knowledge/procedure loaded semi-dynamically, possibly by several agents? | Skill | Entry file ≤120 lines; split dense content into references — see [references/skill-anatomy.md](references/skill-anatomy.md) |
| Is a human expected to invoke it directly, once per use? | Command | No hard limit, but extract anything shared into a skill/agent/rule instead of inlining — see [references/command-anatomy.md](references/command-anatomy.md) |
| Is it expected to be (almost) always in context? | Rule | Extremely dense, no prose — see [references/memory-anatomy.md](references/memory-anatomy.md) |

If a request satisfies more than one row, split it into one asset per row — do not let one asset cover several rows.

## Harness Storage Mapping

Classification (above) never depends on the harness. Where a classified asset is written, and its exact frontmatter shape, does. Determine which harness is active before Steps 1 and 7 of the Flow below — ask the human if it isn't obvious from the repo (presence of `.claude/` vs `.github/agents`, `.github/prompts`, etc.).

| Asset type | Claude Code | GitHub Copilot |
|---|---|---|
| Agent | `.claude/agents/<kebab-name>.md` | `.github/agents/<kebab-case-name>.agent.md` |
| Skill | `.claude/skills/<kebab-name>/SKILL.md` (+ `references/`) | `.github/skills/<kebab-case-name>/SKILL.md` (+ `references/`) |
| Command | `.claude/commands/<kebab-name>.md` | `.github/prompts/<name>.prompt.md` |
| Rule | `CLAUDE.md` (root, project-wide) or a nested `CLAUDE.md` (directory-scoped only — no glob scoping) | `.github/copilot-instructions.md` (project-wide) or `.github/instructions/<concern>.instructions.md` with `applyTo: <glob>` (glob-scoped) |

Frontmatter fields differ per harness even for the same asset type (e.g. Claude Code agents use a short `model` alias and a comma-separated `tools` list; Copilot agents use a full model display name and a YAML `tools` array) — each anatomy file in `references/` calls out the divergence where it matters.

## Flow

### 1. Assess Existing Assets

Determine the active harness, then read all existing assets at the paths the Harness Storage Mapping gives for that harness. Present the candidate table from [references/core-principles.md](references/core-principles.md). Wait for confirmation that a *new* asset — not an edit to an existing one — is the right call.

### 2. Classify

Apply the Asset Type Classification above. If the request doesn't cleanly fit one row, say so explicitly and propose the split rather than forcing a fit.

### 3. Challenge Intent

Ask the Socratic design questions from [references/core-principles.md](references/core-principles.md) one at a time — job, reuse, tools/access, model (Agent/Command only), output contract. Do not proceed until each is answered.

### 4. Resolve the Model (Agent / Command only)

Follow the Model Tier Resolution order in [references/core-principles.md](references/core-principles.md). Never hardcode a guessed model name without confirming it against the current environment or asking the human.

### 5. Design

Follow the matching per-type anatomy file: [agent](references/agent-anatomy.md), [skill](references/skill-anatomy.md), [command](references/command-anatomy.md), [rule](references/memory-anatomy.md).

### 6. Self-Check

Run the shared checklist in [references/core-principles.md](references/core-principles.md) plus the type-specific checklist in the matching anatomy file. Any failure blocks saving until resolved.

### 7. Save

Write the file at the path the Harness Storage Mapping gives for the active harness and the classified asset type. These files are expected to be git-tracked — present the resulting diff and rely on the human's normal review/commit flow; do not bypass that review, and do not commit on the human's behalf.

## Error Handling

| Situation | Action |
|---|---|
| Active harness is unclear or unconfirmed | Ask before Step 1 — never guess which storage paths to read or write |
| Request doesn't fit exactly one classification row | Propose the split; never force one asset to cover two rows |
| Human insists on merging two responsibilities into one asset | Refuse per the template in [references/core-principles.md](references/core-principles.md); offer the split |
| Model can't be resolved from context, environment docs, or the human | Ask the human explicitly with a tier-based suggestion; never silently pick one |
| An existing asset already covers the request | Recommend extending it; do not create a duplicate |
| Self-check fails | Fix before saving; never save a known-failing asset "temporarily" |
