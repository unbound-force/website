---
title: "Your CI Failed — But Was It Your Fault? How /review-pr Separates Signal from Noise"
description: "When CI fails on a pull request, the first question is: did my changes cause this? /review-pr classifies each failure as PR-caused or pre-existing, then offers to fix the pre-existing ones for you."
lead: "Not your fault? Not your problem. One command classifies CI failures, separates your regressions from pre-existing noise, and offers fix branches for failures that predate your PR."
slug: "review-pr-causality"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 60
toc: true
categories: ["Engineering"]
tags: ["review", "ci", "causality", "pull-request"]
contributors: ["Unbound Force"]
---

## The Problem

You open a pull request. CI runs. Two checks fail. You spend 30 minutes investigating, only to discover that both failures predate your changes — a flaky integration test and a lint rule that was added to the base branch after you created your feature branch.

This is the CI triage tax. Every developer pays it, and it scales with the number of CI checks. The more comprehensive your CI pipeline, the more noise you sift through on every PR. Flaky tests, pre-existing lint violations, stale dependency warnings, and intermittent network timeouts create a background hum of failures that have nothing to do with the code you wrote.

The worst outcome is not wasted time — it is learned helplessness. After enough false alarms, developers start ignoring CI failures entirely. "It's probably pre-existing" becomes the default assumption, and genuine regressions slip through alongside the noise.

## The Solution: CI Causality Analysis

`/review-pr` fetches your PR's CI results and answers the first question every developer asks: *did my changes cause this failure?*

```text
/review-pr          # auto-detect PR from current branch
/review-pr 42       # review a specific PR by number
```

For each failing check, the command fetches the same check's status on the base branch and classifies the failure:

| Base Branch | PR Check | Classification |
|-------------|----------|----------------|
| Pass | Fail | **PR-caused** — your changes introduced this failure |
| Fail | Fail | **Pre-existing** — this failure exists independently of your PR |
| No data | Fail | **Unknown** — treated as PR-caused (conservative default) |

**PR-caused failures** are reported as HIGH or CRITICAL findings with the specific change that likely caused the failure. These are your regressions — you need to fix them.

**Pre-existing failures** are reported separately and do not block the PR verdict. They are someone else's problem — or more precisely, they are a problem that existed before your PR and will continue to exist after it.

## The Full Review

CI causality is the first phase. After classifying failures, `/review-pr` continues with a structured review:

1. **CI check results** — fetch and classify every check (pass, fail, pending, skipped)
2. **Local tool pre-flight** — run linters, tests, and build commands that CI does not already cover (skipping tools whose CI checks passed)
3. **Scoped diff** — fetch the PR diff, skipping lock files, auto-generated code, and binary files
4. **Spec alignment** — locate the associated specification (Speckit or OpenSpec) and check intent alignment
5. **AI review** — apply judgment to what deterministic tools cannot check: architectural alignment, security patterns, constitution compliance
6. **Structured report** — severity-classified findings (CRITICAL, HIGH, MEDIUM, LOW) with specific file paths and line references

The key design principle: deterministic tools run first. AI judgment only applies to what tools cannot verify. This keeps the review grounded in facts rather than opinions.

## Fix It Forward

For pre-existing failures, `/review-pr` does not just report the problem — it offers to fix it. After the review, if pre-existing failures were found:

```text
I identified 2 pre-existing CI failure(s) that are NOT caused by this PR:
  - yamllint: trailing whitespace in config/database.yml
  - test-auth-timeout: intermittent timeout in auth_test.go

These failures also occur on the base branch (main).

Would you like me to create a fix branch with a proposed resolution?
```

If you agree, `/review-pr` creates a fix branch (`fix/pr-42-yamllint`) from the base branch, commits a minimal fix, and reports the branch for your review. The fix stays local — you push and create a PR when ready.

**Safety guards**:
- **Dirty-tree guard**: Will not create a fix branch if you have uncommitted changes
- **Collision check**: Will not overwrite an existing fix branch with the same name
- **Scope limit**: Will not attempt non-trivial fixes that span more than 3 files or require understanding business logic — instead, it reports the issue and recommends manual investigation

## In-line PR Comments

If the review finds HIGH or CRITICAL issues, `/review-pr` offers to post them as in-line comments on the PR. Comments are always shown for your approval before posting — nothing goes on the PR without your explicit confirmation.

A cap of 15 in-line comments prevents noisy reviews. If more than 15 findings qualify, CRITICAL findings take priority and the rest are consolidated into a single summary comment.

## The Complete Review Lifecycle

`/review-pr` pairs with `/review-council` to form a complete review lifecycle:

```text
Develop                     Push                        Review
───────                     ────                        ──────
Write code          ───►    Create PR           ───►    Reviewers look at PR
  │                           │                           │
  ▼                           ▼                           ▼
/review-council             /review-pr                  Merge
(pre-PR, local)             (post-PR, GitHub)
5+ Divisor personas         1 agent, CI-informed
```

**`/review-council`** runs before you push. It launches 5+ Divisor review personas in parallel against your local codebase — catching issues before they reach the PR. Think of it as your personal review team that runs in 2 minutes.

**`/review-pr`** runs after the PR is created. It reads CI results, fetches the diff from GitHub, and produces a single structured review informed by what CI already verified. It complements the reviewers who look at the PR on GitHub.

The two commands are independently useful — you do not need both. But together, they catch issues at two different points: before the code leaves your machine, and after it runs through CI.

## Get Started

Install the Unbound Force CLI and review your next PR:

```bash
brew install unbound-force/tap/unbound-force
/review-pr
```

Your CI failures are classified. Your regressions are separated from the noise. Pre-existing failures get fix branches. The signal is clear.

## See Also

- [Common Workflows](/docs/getting-started/common-workflows/) -- `/review-council` vs `/review-pr` comparison table
- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
