---
title: "Verifying Deterministic Instruction — Deltas, Seams, and Delegation Contracts"
description: "Classify steps with the twice-test, measure deltas with instruction-lint --base, find owners and seams with tool-inventory, and classify delegations."
lead: "The Determinist reviews your instruction changes with tool output. Run the same tools before you push, and the review has nothing to find."
slug: "verifying-deterministic-instruction"
date: 2026-11-08T00:00:00+00:00
draft: false
weight: 60
toc: true
---

## Who This Is For

This tutorial is for engineers who write or change commands (`.opencode/commands/*.md`), agents (`.opencode/agents/*.md`), skills (`.opencode/skills/*/SKILL.md`), convention packs, or the plugin tools those files call. It walks through the evidence that the determinism reviewer, The Determinist (`divisor-determinism`), checks against rules DE-001 through DE-008, so you can produce it yourself before review.

The tools in this tutorial are repo-local to `unbound-force/unbound-force`: the instruction linter, `tool-inventory`, the determinism pack, and the persona. `uf init` does not deliver them to other repositories. For the reasoning behind the rules, read [The Determinist](/blog/the-determinist/).

## Prerequisites

1. **A checkout of `unbound-force/unbound-force`** on a feature branch, with the base branch fetched (the examples use `origin/main`).

2. **node** at a version matching `engines` in `.opencode/package.json`. Both tools run TypeScript directly through node's type stripping. The linter wrapper checks this for you and exits 2 with a clear message if node cannot run the `.ts` entry.

3. **The linter's dependencies installed**:

   ```bash
   (cd .opencode && npm install --no-package-lock)
   ```

4. **`make lint-instructions` runs**. With no arguments it lints the whole tree:

   ```bash
   make lint-instructions
   ```

   The exit codes are 0 for clean, 1 when the linter found errors, and 2 when it could not run (missing dependencies, an unreadable tree, bad arguments). Warnings fail the run only with `--strict`.

## Step 1: Classify Each Step With the Twice-Test

Before you write a step into instruction text, ask the question from the `determinism-first` skill: if a competent agent follows this step twice, does it produce the same result both times?

- **Yes**: the step is deterministic. Find the tool that performs it (Step 3) and tell the agent to call that tool. If no tool does, write one with tests. Do not describe the procedure in prose.
- **No**: the step is judgment, and it belongs in prose.

Here is a step that fails the test as prose. In a command file it reads:

````markdown
3. List the changed commands:

   ```bash
   git fetch origin main
   git merge-base origin/main HEAD
   git diff --name-only origin/main...HEAD -- .opencode/commands/
   ```
````

Every run produces the same file list. It is a DE-001 candidate, and the linter flags it as IL-041: a fenced shell block in an agent, command, or skill file that runs three or more commands. Here is a step that stays in prose:

```markdown
4. For each changed command, decide whether the change alters what the
   user sees. If it does, file a website documentation issue.
```

Deciding whether a change is user-facing is judgment: two reviewers can reasonably disagree, and the answer depends on context.

The test also applies one level up. A fixed chain of tool calls, where each call's arguments are fully determined by the previous call's result, is itself deterministic (DE-005). Collapse it into one intent-level tool, and pass judgment products such as composed text or a human's decision in as parameters.

Some steps are never findings, even near code: a single tool call with stated arguments, one or two output examples kept so readers recognize the shape (DE-002), and prose that describes what a tool does.

## Step 2: Measure Your Instruction Deltas (DE-004)

DE-004 asks that change records state line and token deltas when instruction text changes. The linter computes them against a base ref:

```bash
.uf/scripts/instruction-lint --base origin/main --changed
```

The same run through make:

```bash
make lint-instructions ARGS="--base origin/main --changed"
```

`--base REF` adds a delta table for instruction files since `merge-base(REF, HEAD)`. `--changed` limits findings and deltas to the files your branch changed, including uncommitted and untracked ones. `--changed` requires `--base`.

Run over the changes in PR #993, the output ends like this:

```text
deltas since origin/main (merge-base c7ab33fe33e7)
lines          tokens         load           status     file
1510 (+91)     17068 (+1176)  23351 (+1262)  modified   .opencode/commands/uf.address-feedback.md
406 (-7)       3267 (-47)     8344 (+39)     modified   .opencode/commands/uf.finale.md
1229 (+59)     12863 (+625)   33552 (+711)   modified   .opencode/commands/uf.review-pr.md
2032 (+1)      20051 (+7)     31254 (+93)    modified   .opencode/commands/uf.unleash.md
428 (+6)       5077 (+86)     -              modified   AGENTS.md
total: +150 lines, +1847 tokens across 5 files
```

Each cell is the head value with the change in parentheses. The `load` column is the load set: the file plus everything a command or agent pulls into context with it, such as `AGENTS.md`, skills, and convention packs. A file can appear with status `load-only` when its own text did not change but its load set did. Other statuses are `added`, `modified`, and `deleted`.

### Read the determinism hints

The same run reports the three determinism hints. IL-040 and IL-041 are info-level, so the text output hides them unless you add `--all`:

```bash
.uf/scripts/instruction-lint --base origin/main --changed --all
```

| Hint | Severity | Points at |
|------|----------|-----------|
| IL-040 | info | Phrasing that prescribes output, such as "use the following format" or "output exactly" (DE-003 candidate) |
| IL-041 | info | A fenced shell block in procedural prose that runs three or more commands (DE-001 candidate) |
| IL-042 | warning | A delegation call block without `task_classification`, or one that can classify work as deterministic without `skill` or `skill_absent_reason` (DE-008) |

A hint is not a finding. The Determinist turns each one into a DE finding or dismisses it by quoting the text that shows the step is judgment. Do the same: fix the hint, or be ready to point at the line that dismisses it.

### Use the JSON when a tool reads it

`--json` emits the same data as structured output. The reviewer reads two parts of it, the `findings` with rules IL-040 to IL-042 and the `delta` object:

```bash
.uf/scripts/instruction-lint --json --base origin/main --changed
```

```json
"delta": {
  "base": "origin/main",
  "mergeBase": "c7ab33fe33e7…",
  "files": [
    { "file": ".opencode/commands/uf.finale.md", "status": "modified",
      "lines": { "base": 413, "head": 406, "delta": -7 },
      "tokens": { "base": 3314, "head": 3267, "delta": -47 },
      "loadTokens": { "base": 8305, "head": 8344, "delta": 39 } }
  ],
  "totals": { "lines": 150, "tokens": 1847 }
}
```

## Step 3: Find Owners and Seams With `tool-inventory`

`.opencode/lib/tool-inventory.ts` lists every registered plugin tool and repo script from source, without loading a plugin. It has two modes.

### Owner lookup (DE-001, DE-003, DE-005)

For each step you classified as deterministic, look for an existing tool before you write one. Pass two to four words that name what the step does:

```bash
node .opencode/lib/tool-inventory.ts --match "stage finale learnings"
```

```text
uf_finale_stage  [uf-finale]  .opencode/plugins/uf-finale/index.ts:201
  Stage all changes in the working tree except .uf/dewey/learnings/ and return the count of staged files. Learnings stay untracked for the separate learnings PR.
```

Results are ranked by how many of the words appear in each tool's name or description; tools that match none are dropped. If the top entry performs the step, call it from your instruction and cite it by name. If none does, the step has no owner yet, and the next increment writes one. Add `--json` for machine-readable output.

### Seam evidence (DE-007)

A prose-to-tool migration creates seams: the tool's registration, the output shape your instructions document, and any state one call writes and another reads back. DE-007 requires the increment that creates them to test them. List the tools your branch touched and the tests that reference each one:

```bash
node .opencode/lib/tool-inventory.ts --seams --base origin/main
```

Run over the changes in PR #993:

```text
seams since origin/main (merge-base c7ab33fe33e7): 2 tool(s)

resolve_review_mode  [review-dispatch, added]
  registration  generic: .opencode/test/registration-surface.test.ts
  other         .opencode/test/plugin-integration.test.ts  (changed)
  other         .opencode/test/resolve-review-mode.test.ts  (changed)
  other         .opencode/test/scratch-smoke.test.ts  (changed)
  missing: output-shape, round-trip

uf_finale_stage  [uf-finale, modified]
  registration  generic: .opencode/test/registration-surface.test.ts
  output-shape  .opencode/test/golden-output-shape.test.ts  (changed)
  other         .opencode/test/scratch-smoke.test.ts  (changed)
  other         .opencode/test/uf-finale-stage-git.test.ts  (changed)
  other         .opencode/test/uf-finale.test.ts  (changed)
  missing: round-trip
```

`generic` means `registration-surface.test.ts` already covers the tool's registration surface. `(changed)` marks a test file your branch edited. `missing` lists layers no test references. It is evidence, not a verdict; you decide which layers each tool needs:

| Layer | Needed when |
|-------|-------------|
| registration | Every added or modified tool |
| output-shape | An instruction file documents the tool's output fields |
| round-trip | The tool writes state that another tool or a later call reads back |

For each `missing` entry, either add the test or note why the layer does not apply. A `removed` tool whose `tests` list is not empty means tests still reference a tool that no longer exists.

## Step 4: Bind Delegations to the Classification Contract (DE-008)

When a command delegates work through `uf_delegate_command` or `replicator_forge_spawn_subtask`, the classification travels in the call's parameters, not in prose around it:

- `task_classification`: `deterministic` or `judgment`, derived from the nature of the work, never from routing metadata such as the workflow tier.
- `skill`: for deterministic work, a repo skill name or repo-relative script path that performs it.
- `skill_absent_reason`: for deterministic work with no such skill or script, what you searched for and what you found. `n/a` and `not needed` do not count.

`skill` and `skill_absent_reason` are mutually exclusive. A `uf_delegate_command` call that names a `command_path` may omit `task_classification`. The tools fail closed when the contract is missing (`SKILL_REFERENCE_REQUIRED`, `SKILL_NOT_FOUND`) and record the classification, reference, and reason; that record is the audit trail.

This call block draws IL-042, because it delegates a prompt with no classification:

```text
uf_delegate_command
  prompt: [the task description]
  feature_dir: FEATURE_DIR
```

This one passes:

```text
uf_delegate_command
  prompt: [the task description]
  task_classification: [deterministic or judgment, from the nature of the work]
  skill: [repo skill or script that performs the task -- only when deterministic]
  skill_absent_reason: [what was searched and what was found -- only when
    deterministic and no repo skill or script applies; never with skill]
  feature_dir: FEATURE_DIR
```

IL-042 only reads call blocks inside fenced code in agent, command, and skill files. A prose sentence that mentions the tool is not a call site, and naming a skill in prose without the parameters is an interim mitigation, not the contract.

## Step 5: Add the Seam-Verification Task

If your change moves a procedure from prose into a tool, plan the seam tests before you write the code. DE-007 requires the increment's `tasks.md` to include a seam-verification task naming all three layers, and The Determinist's Spec Review Mode reports a missing one as HIGH.

A task in the shape the repository uses:

```markdown
- [ ] 4.2 DE-007 seam verification naming the three layers:
  registration surface (the new tool is registered and its input
  schema matches the documented parameters); documented output
  shape (golden assertions on the fields uf.example.md reads);
  write-then-read round-trips (the state file the tool writes is
  read back by the consuming tool).
```

Name the real tool, fields, and consumers. When the work is done, run `--seams` again and confirm that `missing` lists only layers you have shown do not apply.

## Step 6: Record the Deltas in the Change Record

DE-004 is satisfied when a change record states the deltas: the proposal, `tasks.md`, the PR description, or the CHANGELOG. Paste the totals line and the per-file rows from Step 2. Do not round or recompute them; the reviewer compares against the same tool's output.

PR #993 recorded its deltas in the PR description like this:

```markdown
### Instruction size (DE-004)

`.uf/scripts/instruction-lint --base origin/main --changed`: 0 errors.

| File | Lines | Tokens | Loaded |
|---|---|---|---|
| `uf.address-feedback.md` | 1510 (+91) | 17068 (+1176) | 23351 (+1262) |
| `uf.review-pr.md` | 1229 (+59) | 12863 (+625) | 33552 (+711) |
| `uf.finale.md` | 406 (-7) | 3267 (-47) | 8344 (+39) |
```

When the numbers grow, say why. A change that adds instruction text can be the right change; the record makes the cost visible so reviewers weigh it on evidence.

## What the Review Checks

Once these steps are done, The Determinist's review of your branch has its evidence ready:

| Rule | Your evidence |
|------|---------------|
| DE-001, DE-003, DE-005 | Each mechanical step calls a tool you found with `--match`, or a new tested tool |
| DE-002 | One or two output examples next to the step that reads the tool's output |
| DE-004 | The Step 2 delta rows in the change record |
| DE-006 | Routing logic (verdict mapping, severity sorting, batch scheduling) inside the tool, not in its arguments |
| DE-007 | `--seams` shows each needed layer covered, and `tasks.md` has the seam-verification task |
| DE-008 | Every delegation call block passes the classification contract, and IL-042 is clean |

## Further Reading

- [The Determinist](/blog/the-determinist/): why the DE review moved onto tool-backed evidence
- [The Divisor](/docs/team/the-divisor/): the council roster, including the repo-local personas
- [A Converging Review and Feedback Cycle](/docs/tutorials/converging-review-cycle/): what happens after you push
