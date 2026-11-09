---
title: "The Determinist — Evidence-Based Determinism Review of Agent Instruction"
description: "A reviewer that enforced 'fixed procedures belong in tools' by reading prose broke its own rule. The Determinist reviews DE-001 to DE-008 with tool evidence."
lead: "If a competent agent follows a step twice and gets the same result both times, the step belongs in a tool. We applied that test to the reviewer that enforces it."
slug: "the-determinist"
date: 2026-11-08T00:00:00+00:00
draft: false
weight: 110
toc: true
categories: ["Engineering"]
tags: ["determinism", "code-review", "divisor", "agents", "instruction-quality"]
contributors: ["Unbound Force"]
---

## The Twice-Test

Constitution Principle VI, Deterministic Execution, rests on one question. The `determinism-first` skill puts it to every step an author writes into a command, agent, or skill:

> If a competent agent follows this step twice, does it produce the same result both times?

A yes means the step is a fixed procedure. Parsing frontmatter, checking whether a file exists, computing a merge base, looping over a directory, mapping a verdict to a GitHub review event: each gives the same answer on every run. It belongs in a tested tool, because an agent that re-derives it from prose spends context on every run and can get it wrong on any of them.

A no means the step is judgment. Choosing between designs, triaging a finding, writing a commit message, or asking the user something can legitimately come out differently each time, and that work stays in prose.

The determinism convention pack turns the principle into eight review rules:

| Rule | What it asks |
|------|--------------|
| DE-001 | Mechanical instruction is extracted to a tool with tests |
| DE-002 | Extractions keep one or two output-format examples near the decision that consumes them |
| DE-003 | Prescribed terminal output is tool-rendered, not rebuilt from a template in prose |
| DE-004 | Change records state line and token deltas when instruction text changes |
| DE-005 | A judgment-free chain of tool calls collapses into one intent-level tool |
| DE-006 | Intent-level tools keep routing decisions inside the tool, not in the caller |
| DE-007 | A prose-to-tool migration verifies its seams in the same increment |
| DE-008 | Delegated deterministic work carries a skill or script reference through the delegation tool's parameters |

The [MUST] rules (DE-001, DE-003, DE-005, DE-007, DE-008) are CRITICAL when violated. The [SHOULD] rules (DE-002, DE-004, DE-006) are MEDIUM.

## A Reviewer That Broke Its Own Rule

Until PR #959, the DE rules were part of The Linguist's job. The Linguist is the repo-local Divisor persona in `unbound-force/unbound-force` that reviews instruction text, the prose other agents execute. It ran the instruction linter, then read changed files to decide whether a step was mechanical, whether a delta was recorded, whether a tool had tests.

That reading was the problem. Checking the DE rules by hand means repeating the same work on every run:

- **DE-004**: count lines and tokens in each changed file before and after the change, then check the change record for the numbers.
- **DE-001, DE-003, DE-005**: decide whether an existing tool already performs a mechanical step, which means searching the plugins for one.
- **DE-007**: for each tool the change touched, find the tests that reference it and work out which seam layer each one covers.

Each of those passes the twice-test. Two careful reviewers counting the same tokens get the same count. Two reviewers listing the tests that name `uf_finale_stage` get the same list. A reviewer that did this work in prose would be enforcing "fixed procedures belong in tools" by running fixed procedures in prose.

PR #959 fixed this in two moves: it built the tools the review needed, and it gave the DE rules to a new persona whose instructions say to cite tool output and not re-derive it.

## Moving the Evidence Into Tools

### Hints from the linter: IL-040, IL-041, IL-042

The instruction linter (`.opencode/lib/instruction-lint.ts`, run through `make lint-instructions` or the `.uf/scripts/instruction-lint` wrapper) already knew two determinism hints, and PR #959 added a third:

- **IL-040** (info): phrasing that prescribes output, a candidate for DE-003.
- **IL-041** (info): a fenced shell block in procedural prose that runs three or more commands, a candidate for DE-001.
- **IL-042** (warning): a `uf_delegate_command` or `replicator_forge_spawn_subtask` call block that passes no `task_classification`, or that can classify work as deterministic without passing `skill` or `skill_absent_reason`. That is DE-008. Delegations that name a `command_path` are exempt, matching the tool's own schema.

These are hints, not findings. The persona turns each one into a DE finding or into nothing, and it can dismiss one only by quoting the text that shows the step is judgment, or the later line that supplies the missing parameter. Dismissed hints are listed in the review summary with their quotes, so a reader can check them.

### Deltas from `--base`: DE-004

`instruction-lint --base REF` now reports per-file line, token, and load-set deltas since `merge-base(REF, HEAD)`. Add `--changed` to limit findings and deltas to the files the branch changed. Run over the changes in PR #993, it prints the numbers that PR recorded in its description:

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

The persona's DE-004 rule tells it to compute nothing. If `delta.files` lists a changed file and no change record states the deltas, the finding's recommendation is the tool's own totals line and rows, ready to paste.

### Owners and seams from `tool-inventory`: DE-001, DE-003, DE-005, DE-007

A new script, `.opencode/lib/tool-inventory.ts`, answers the two questions the reviewer used to answer by searching.

**Who owns this step?** `--match WORDS` ranks every registered plugin tool and repo script by how many of the words its name or description contains:

```bash
node .opencode/lib/tool-inventory.ts --match "stage finale learnings"
```

```text
uf_finale_stage  [uf-finale]  .opencode/plugins/uf-finale/index.ts:201
  Stage all changes in the working tree except .uf/dewey/learnings/ and return the count of staged files. Learnings stay untracked for the separate learnings PR.
```

A DE-001 finding now names its owner with a `file:line`, or says that no tool performs the step and describes what the new tool would take and return. Discovery is static, so the script runs under plain node without loading a plugin, and `tool-inventory.test.ts` pins it against the runtime registry so the static view cannot drift from what actually registers.

**Which seams are tested?** `--seams --base REF` lists every plugin tool added, modified, or removed since the merge base, with the tests that reference it, grouped by the three DE-007 layers: registration surface, documented output shape, and write-then-read round trip. Each entry also lists its missing layers. That list is evidence, not a verdict. The persona decides which layers a tool needs: registration for every added or modified tool, output shape when an instruction file documents the tool's output fields, and round trip when the tool writes state that a later call reads back.

## One Owner Per Finding

With the evidence in tools, PR #959 split the review. **The Determinist** (`divisor-determinism`) owns DE-001 through DE-008 and the IL-040 to IL-042 hints. **The Linguist** keeps the rest of the linter's rules (frontmatter, references, RFC 2119 markers, token budgets, duplication) and its instruction-clarity checks, and its instructions now say not to report or classify the determinism hints.

The Divisor's ownership model gives each review dimension exactly one owner, so a problem is reported once. Determinism now follows it: a DE finding has one possible author, and the hints that feed it have one reader.

Each DE finding carries three things:

1. **Evidence**: the changed text, quoted verbatim in five lines or fewer, or the tool output entry for DE-004 and DE-007.
2. **Twice-test result**: one sentence on why the step gives the same result every time.
3. **Owner**: for DE-001, DE-003, and DE-005, the `--match` result with its `file:line`.

The persona's own rule is short: no quote, no finding.

## What It Does Not Flag

A determinism reviewer that flagged every sentence near a code block would be noise. The Determinist's calibration lists what is not a finding:

- **Decisions, composition, and human interaction.** Triage, summaries, commit messages, and questions to the user are judgment.
- **A single tool call with stated arguments.** That is a tool call, not mechanical prose. It becomes DE-005 only when two or more calls chain and each call's arguments are fully determined by the previous result.
- **One or two output examples kept for recognition.** That is what DE-002 asks for, unless the text tells the agent to reproduce them.
- **Prose that describes what a tool does.** Documenting a mechanism is not running a procedure.

The rules also apply to new and changed instruction only. Unchanged prose in a changed file draws no finding unless the change edits the same section, and then it is at most a LOW suggestion to migrate it while you are there.

## Least Privilege for the Reviewer

The Determinist runs tools, so it needs a shell. It gets very little of one. Its frontmatter denies every bash command except the linter wrapper and `node .opencode/lib/tool-inventory.ts`, then adds catch-all denies for redirection, pipes, `;`, `&`, `$(...)`, and backticks, so an allowed prefix cannot be chained into something else. Edits and web fetches are denied. This follows the pattern described in [Why Your AI Code Reviewer Cannot Have Write Access](/blog/ai-code-reviewer-write-access/): the reviewer can produce evidence and findings, and nothing else.

It also treats every input as untrusted data: the diff, the linter output, the seam evidence, and the files it reads. An instruction embedded in any of them is reported as a HIGH finding and never followed.

## Where It Runs

The Determinist, the determinism pack, the instruction linter, and `tool-inventory` are repo-local to `unbound-force/unbound-force`; `uf init` does not deliver them. With The Linguist, they bring the review council in that repository to 11 personas: the 9 canonical personas plus 2 repo-local ones. The Determinist registers with the `documentation` and `standard` scopes, so the council plans it for changes to instruction files and tool code.

The pattern carries to any agent repository. Write down the question your reviewer keeps answering by reading. If the answer would be the same on a second reading, write a tool that produces it, test the tool, and tell the reviewer to cite the tool's output. The reviewer's context is then spent on the judgment calls only it can make.

## See Also

- [Verifying Deterministic Instruction](/docs/tutorials/verifying-deterministic-instruction/): the author's side of the same evidence, step by step
- [The Divisor](/docs/team/the-divisor/): the full council roster and ownership boundaries
- [Five Principles Every AI Agent Harness Discovers](/blog/five-principles-ai-agent-harness/): where "computational before inferential" comes from
