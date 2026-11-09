---
title: "A Converging Review and Feedback Cycle"
description: "Walk one PR through a full council review, severity-scoped fixes, a light re-review that approves, a learnings PR, and a re-review that escalates."
lead: "One full council review, one round of fixes, one light check, done. Here is how the review and feedback commands keep a pull request from bouncing between them."
slug: "converging-review-cycle"
date: 2026-11-08T00:00:00+00:00
draft: false
weight: 70
toc: true
---

## Why Rounds Have to Converge

`/uf.review-pr` reviews a pull request and posts findings. `/uf.address-feedback` works through those findings and pushes fixes. Run them back to back and you have a loop, and a loop needs something that makes it stop.

On one pull request in `unbound-force/unbound-force`, each command ran five rounds. The PR picked up 35 commits and about 60 inline findings, most of them LOW. Each round fixed every finding, each fix earned a full council review, and each council found new MEDIUM and LOW items in code an earlier round had already passed.

PR #993 changed three things so the rounds converge:

1. **Feedback changes code only for CRITICAL and HIGH findings.** MEDIUM and LOW findings are filed as issues or answered, and their threads are resolved.
2. **One council per cycle.** `/uf.address-feedback` runs the pre-flight checks before pushing, not `/uf.review-council`. The re-review is the cycle's council, and when the push is small and contained it runs a light check instead.
3. **Learnings stay off the PR branch.** Dewey learnings reach `main` through their own `chore/dewey-learnings-*` PR, so storing a lesson never adds a commit to the PR under review.

This tutorial follows one pull request, #42, through a cycle.

## Prerequisites

1. **A repository initialized with `uf init`**, so `/uf.review-pr`, `/uf.address-feedback`, the `review-dispatch` plugin, and the `pre-flight` skill are present.
2. **The `gh` CLI, installed and authenticated** (`gh auth login`), with permission to review and comment on the pull request.
3. **An open pull request** on a branch you can push to.

## Step 1: The First Review Runs the Full Council

```text
/uf.review-pr 42
```

Before it plans any reviewers, Step 3.11 calls the `resolve_review_mode` tool, which decides between the full council and a light check. You have not reviewed this PR yet, so it decides `full`:

```text
Re-review: full (no earlier review by alex on another commit)
```

The council runs as usual. Say it returns REQUEST CHANGES with six findings: two HIGH, one MEDIUM, and three LOW. When you confirm posting in Step 8, each finding becomes an inline comment that starts with its severity label, such as `**HIGH — ...`. Those labels matter in both of the next steps.

## Step 2: Feedback Fixes CRITICAL and HIGH, and Files or Answers the Rest

```text
/uf.address-feedback 42
```

The command fetches the unresolved threads (Ingest), classifies each one (Assess), asks you to decide on each (Triage), and carries out the decisions (Execute).

### Severity sets the options

During Assess, each item's severity decides which recommendations it can get:

| Severity | Recommendation |
|---|---|
| CRITICAL, HIGH, or no label | From the authority matrix (ACCEPT or AUTHOR-DECIDES) |
| MEDIUM or LOW, naming a defect or follow-up work worth tracking | `FILE` |
| MEDIUM or LOW, otherwise | `REPLY` |

A comment with no severity label, which covers most comments from people, counts as HIGH. MEDIUM and LOW items are always assessed directly, without dispatching Divisor agents.

During Triage, the two groups get different options:

| Item | Options |
|---|---|
| CRITICAL, HIGH, or no label | Accept, Modify, Reject, Ask |
| MEDIUM or LOW | File issue, Reply, Ask |

None of the MEDIUM or LOW options changes code in this run. For PR #42, you Accept both HIGH findings, choose File issue for the MEDIUM one, which names a real follow-up, and Reply to the three LOW ones.

### Execute: pre-flight, not the council

After you confirm the execution plan, the command:

1. Commits the two accepted fixes.
2. Runs the `pre-flight` skill in hard-gate mode, the same checks `/uf.unleash` gates on. If a check fails, it fixes the failure in a new commit and runs the checks again, up to 3 attempts in all. If a check still fails after that, it stops and does not push.
3. Pushes.
4. Files the GitHub issue for the MEDIUM finding and queues a reply linking it.
5. Posts the replies, after showing them to you.
6. Offers to resolve the threads of Accept, Modify, File issue, and Reply items. Reject and Ask threads stay open, because they still need an answer.

It does not run `/uf.review-council`. The re-review in the next step is the cycle's one council, so the fixes are not reviewed twice.

Resolving the File issue and Reply threads matters. Left open, they look like unresolved feedback, and the next `/uf.address-feedback` run would pick them up again.

## Step 3: The Re-Review Runs a Light Check and Approves

```text
/uf.review-pr 42
```

`resolve_review_mode` runs again. This time it finds your earlier review on an older commit, compares that commit with the head, and decides `light`:

```text
Re-review: light (3 files, 46 lines since 9f3c2ab)
```

The tool returns `light` only when all of these hold:

- You reviewed an earlier commit, and that commit is still an ancestor of the head.
- The push is at most 200 changed lines, and the compare response lists fewer than 300 files.
- No file was added, removed, renamed, or copied.
- No sensitive path changed: workflows and settings under `.github/`, `AGENTS.md`, the constitution, the convention packs, or any path naming auth, tokens, secrets, or credentials.
- The earlier review had no CRITICAL finding.
- No check is failing.

Any input the tool cannot read decides `full`, and so does an error from the tool. The thresholds live in the plugin as tested constants, so the command never recomputes them.

The light check (Step 3.12) dispatches no reviewers. It treats the diff and the earlier review as untrusted data and does three things:

1. **Verifies the earlier findings.** For each CRITICAL, HIGH, or unlabelled inline finding of the earlier review, it reads the head code and records it as RESOLVED or OPEN, citing the file and line.
2. **Reads only the new commits.** It looks for CRITICAL or HIGH problems they introduce, and does not raise MEDIUM or LOW findings on code an earlier round passed.
3. **Decides.** An earlier CRITICAL or HIGH finding still OPEN, or a failing local check, means REQUEST CHANGES. Otherwise it approves.

Both HIGH findings on PR #42 are fixed, so the report replaces the dispatch table with one line and the verdict is APPROVE:

```text
Light check since 9f3c2ab: 2 prior CRITICAL/HIGH findings resolved, 3 files and 46 lines reviewed.
```

Step 8 posts the verdict as usual. The cycle took one full council and one light check.

## Step 4: Learnings Leave Through Their Own PR

Both commands can store Dewey learnings: the council's lesson proposals in Step 1, and the Tier 2 assessments in Step 2. The light check stores none. Stored learnings are files under `.uf/dewey/learnings/`, and they stay untracked on feature and PR branches:

- `uf_finale_stage`, the staging tool behind `/uf.finale`, stages everything except `.uf/dewey/learnings/`, and unstages anything already staged there.
- The Commit scope rule in `AGENTS.md` says learnings must not be committed on a feature or PR branch.
- Compiled articles under `.uf/dewey/compiled/` still go in the feature PR.

The learnings reach `main` through a separate pull request on a branch named like `chore/dewey-learnings-<checkout>-<date>`, for example `chore/dewey-learnings-merlin-20261108`. In `unbound-force/unbound-force`, gh-hud opens that PR once a day per checkout. Without it, open one by hand from `main`:

```bash
git switch -c chore/dewey-learnings-merlin-20261108 main
git add .uf/dewey/learnings/
git commit -m "chore(dewey): store learnings from unbound-force@merlin"
git push -u origin chore/dewey-learnings-merlin-20261108
gh pr create --fill
```

Because no lesson rides the PR under review, storing one never moves the head, never dismisses an approval, and never starts another review.

## When the Re-Review Escalates to the Full Council

The light check is an optimization. The full council runs whenever the evidence says it should. Three common cases follow, each continuing from Step 2.

### You rebased the branch

You rebased PR #42 onto a newer `main` before pushing the fixes. The commit you reviewed is no longer an ancestor of the head:

```text
Re-review: full (the reviewed commit is no longer an ancestor of the head (compare status diverged))
```

The diff since the earlier review now includes everything the rebase brought in, so the council reviews the PR again.

### The fix touched a sensitive path

One of the accepted fixes added a line to `AGENTS.md`:

```text
Re-review: full (AGENTS.md is a sensitive path)
```

Changes to agent instructions, the constitution, convention packs, workflows, or anything naming auth, tokens, secrets, or credentials always get the full council, however small.

### The light check finds a new HIGH problem

The push qualified, and the light check ran. Both earlier findings are RESOLVED, but one of the new commits builds a shell command from a PR title without quoting it. That is a new HIGH problem in the delta.

The light check does not post it. It sets the review mode to `full` and continues at Discover Divisor Agents, so the council confirms the problem before anything reaches the PR. The council's findings then go through `/uf.address-feedback` like any others, and the next re-review gets its own chance at a light check.

### Other reasons you may see

| Reason | Why it decides full |
|---|---|
| `the prior review raised a CRITICAL finding` | A CRITICAL finding always gets the council's second look |
| `N changed lines exceed 200` | The push is too large for a light check |
| `<file> was added` (or `removed`, `renamed`, `copied`) | A structural change can shift what the earlier review covered |
| `a check is failing` | A red check needs the full review |
| `the compare response may be truncated` | The compare listed 300 files, the most it returns |
| `the changes since the prior review could not be read` | Missing data never earns a light check |

## Summary

| Step | Command | What converges |
|---|---|---|
| 1 | `/uf.review-pr` | First review: full council, severity-labelled findings |
| 2 | `/uf.address-feedback` | Code changes only for CRITICAL and HIGH; MEDIUM and LOW filed or answered and resolved; pre-flight gate, no council |
| 3 | `/uf.review-pr` | Light check verifies the earlier findings and approves, or escalates to the council |
| 4 | gh-hud or by hand | Learnings reach `main` through a `chore/dewey-learnings-*` PR |

## Further Reading

- [Common Workflows: Address Review Feedback](/docs/getting-started/common-workflows/#address-review-feedback-ufaddress-feedback): the command reference for both steps
- [Code Review Tutorial](/docs/getting-started/code-review-tutorial/): `/uf.review-council` and `/uf.review-pr` from the start
- [Verifying Deterministic Instruction](/docs/tutorials/verifying-deterministic-instruction/): the evidence behind instruction-file reviews
