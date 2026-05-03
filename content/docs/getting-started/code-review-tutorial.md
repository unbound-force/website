---
title: "Code Review Tutorial"
description: "Step-by-step walkthrough of the complete code review lifecycle — /review-council before pushing, /review-pr after creating a PR."
lead: "Two commands, one review lifecycle. Use /review-council to validate locally before pushing, then /review-pr to review the PR with CI results."
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 72
toc: true
---

## Prerequisites

Before starting this tutorial, ensure:

1. **`uf init` completed** — your project has Divisor review agents deployed (`.opencode/agents/divisor-*.md`)
2. **`gh` CLI installed and authenticated** — `/review-pr` uses `gh` to fetch PR metadata and CI results

```bash
which gh && gh auth status
```

If `gh` is not installed, get it from [cli.github.com](https://cli.github.com/). If not authenticated, run `gh auth login`.

## Step 1: Pre-PR Review with `/review-council`

Before you push your changes and create a PR, run `/review-council` to catch issues locally:

```text
/review-council
```

### What Happens

The review council runs in two phases:

**Phase 1: CI Gate** — runs your project's build, test, and lint commands (derived from `.github/workflows/`). If the build fails, the review stops immediately. This is a hard gate — no point reviewing code that does not compile.

**Phase 2: Divisor Review** — launches 5+ review personas in parallel, each with a different focus:

| Persona | Focus |
|---------|-------|
| **Adversary** | Security, resilience, edge cases |
| **Architect** | Structure, conventions, patterns |
| **Guard** | Intent drift, scope discipline |
| **Testing** | Test coverage, isolation, assertions |
| **SRE** | Deployment, operational readiness |
| **Curator** | Documentation gaps |

### Expected Output

```text
Phase 1: CI Gate
  ✓ npm run build — passed (536ms)

Phase 2: Divisor Review
  Adversary:  APPROVE (0 findings)
  Architect:  APPROVE (1 LOW — weight inconsistency)
  Guard:      APPROVE (0 findings)
  Testing:    APPROVE (0 findings)
  SRE:        APPROVE (0 findings)
  Curator:    APPROVE (0 findings)

Council Verdict: APPROVE
```

If any persona returns **REQUEST CHANGES**, the council verdict is REQUEST CHANGES. Fix the findings and re-run. The review council iterates up to 3 times, auto-fixing LOW and MEDIUM findings where possible.

**Gaze integration** (optional): If [Gaze](/docs/projects/gaze/) is installed, the review council runs quality analysis (CRAP scores, test coverage) as Phase 1b. This is informational — it does not block the verdict.

## Step 2: Push and Create a PR

Once the review council approves, push your changes and create a PR:

```bash
/finale
```

Or manually:

```bash
git push -u origin my-branch
gh pr create --title "feat: add user auth" --body "..."
```

## Step 3: Post-PR Review with `/review-pr`

After the PR is created and CI has run, review the PR:

```text
/review-pr
```

This auto-detects the open PR for your current branch. To review a specific PR (including someone else's):

```text
/review-pr 42
```

### What Happens

`/review-pr` runs a structured review informed by CI results:

1. **Resolve PR** — auto-detect or use the provided PR number
2. **Fetch metadata** — title, description, changed files, branch info
3. **CI check results** — fetch pass/fail status for every check
4. **Causality classification** — for each failing check, determine if it is PR-caused or pre-existing
5. **Local tool pre-flight** — run tools only for checks CI did not already cover
6. **Scoped diff** — fetch the PR diff (skipping lock files, generated code, binaries)
7. **Spec alignment** — locate the associated spec for intent checking
8. **AI review** — judgment on alignment, security, and architecture
9. **Structured report** — severity-classified findings

### Expected Output

```text
PR #42: feat: add user auth
Branch: feature/user-auth → main
Files changed: 8 (+342, -12)

CI Checks:
  ✓ Build & Test — passed
  ✓ Lint — passed
  ✗ yamllint — FAILED (pre-existing)

Causality Analysis:
  yamllint: base branch FAIL, PR FAIL → Pre-existing
  (This failure exists independently of your PR)

Local Pre-flight:
  Skipped: build (CI passed), lint (CI passed)

Review Findings:
  [HIGH] Missing error handling in auth.go:42
  [MEDIUM] Consider extracting helper function at auth.go:87

Verdict: 2 findings (1 HIGH, 1 MEDIUM)
```

## Step 4: Understanding CI Causality

When CI checks fail on your PR, the key question is: *did my changes cause this?*

`/review-pr` answers this by checking whether the same check also fails on the base branch:

| Base Branch | PR Check | Classification |
|-------------|----------|----------------|
| Pass | Fail | **PR-caused** — your changes introduced this |
| Fail | Fail | **Pre-existing** — failure exists independently |
| No data | Fail | **Unknown** — treated as PR-caused (conservative) |

**PR-caused failures** are reported as HIGH or CRITICAL findings. These are your regressions.

**Pre-existing failures** are reported separately and do not block the PR verdict. They are not your problem — but `/review-pr` can help fix them.

## Step 5: Fix Branches for Pre-existing Failures

When pre-existing failures are found, `/review-pr` offers to create a fix branch:

```text
I identified 1 pre-existing CI failure:
  - yamllint: trailing whitespace in config/database.yml

Would you like me to create a fix branch?
```

If you agree, it creates `fix/pr-42-yamllint` from the base branch with a minimal fix. The branch stays local — you review and push when ready.

**Safety guards**:
- Will not create a fix branch if you have uncommitted changes (dirty-tree guard)
- Will not overwrite an existing branch with the same name (collision check)
- Will not attempt non-trivial fixes spanning more than 3 files

## The Complete Loop

The two review commands fit into the standard development workflow:

```text
/speckit.specify          # define the work
       ↓
/unleash                  # implement autonomously
       ↓
/review-council           # validate locally (pre-PR)
       ↓
/finale                   # commit, push, create PR
       ↓
/review-pr                # review with CI data (post-PR)
       ↓
Merge                     # after reviewer approval
```

You can use either command independently — they do not depend on each other. But together they catch issues at two different points: before the code leaves your machine and after it runs through CI.

## Decision Table

| Situation | Command | Why |
|-----------|---------|-----|
| Before pushing | `/review-council` | Catch issues locally with 5+ parallel reviewers |
| After creating a PR | `/review-pr` | Review with CI results and causality analysis |
| Reviewing someone else's PR | `/review-pr 42` | Works on any PR by number |
| CI failed, unsure if my fault | `/review-pr` | Causality classification separates your regressions from noise |
| Want maximum coverage | Both in sequence | `/review-council` pre-push, `/review-pr` post-PR |

## See Also

- [Common Workflows](/docs/getting-started/common-workflows/) -- `/review-council` vs `/review-pr` comparison table and full command reference
- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
