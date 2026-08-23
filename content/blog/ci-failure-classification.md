---
title: "Your CI Failed — But Was It Your Fault? How /review-pr Separates Signal from Noise"
description: "When CI fails on a PR, developers waste time investigating failures that predate their changes. /review-pr classifies each failure as PR-caused or pre-existing — one command replaces hours of forensic debugging."
lead: "Not your fault? Not your problem. One command classifies every CI failure as PR-caused or pre-existing, so you stop debugging test flakes you didn't create."
slug: "ci-failure-classification"
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 101
toc: true
categories: ["Engineering"]
tags: ["review-pr", "CI", "code-review", "automation"]
contributors: ["Unbound Force"]
---

## The Red X That Wastes Your Morning

You open your pull request. CI is red. Your stomach drops — and then you spend the next hour discovering that the failure has nothing to do with your changes. A flaky integration test that fails one run in ten. A lint violation introduced three PRs ago. A dependency warning that nobody addressed last sprint. You didn't break anything, but you're the one staring at log output trying to prove it.

This is the hidden tax of continuous integration. CI pipelines report pass or fail, but they don't tell you *whose* fault it is. Every red build lands on the PR author's desk, regardless of whether the failure existed before their branch diverged from main. The result is a forensic debugging ritual: compare the failing test against the base branch, check if the lint rule was already violated, search Slack for "is this test flaky?" — all before you can address the actual review feedback on your code.

The cost compounds across a team. Senior engineers develop intuition for which failures to ignore, but junior developers investigate every red check. Across an organization, hundreds of engineering hours per quarter evaporate into diagnosing failures that predate the current PR.

## What If CI Told You Who Broke It?

The `/review-pr` command takes a different approach. Instead of dumping a wall of failures and leaving you to sort them out, it fetches CI results from your pull request and classifies each failure into one of two buckets: **PR-caused** or **pre-existing**.

The classification works by comparing the failure against the base branch. If a test also fails on main, it's pre-existing — not your regression. If a lint violation exists in files you didn't touch, it's pre-existing. If a check fails only on your branch, in code your commits modified, it's PR-caused and needs your attention.

```text
/review-pr
```

That's the entire invocation. One command fetches the CI results from GitHub, runs local analysis tools against your changes, applies AI judgment to classify ambiguous cases, and produces a structured report. No manual log diving. No cross-referencing against the base branch. No asking teammates if a test is "known flaky."

## Two Buckets, Two Workflows

For **PR-caused failures**, `/review-pr` doesn't stop at classification. It runs the same local tools that CI uses — linters, test suites, static analyzers — and produces actionable findings with file paths, line numbers, and explanations. The AI layer adds context that raw tool output lacks: why the failure matters, what the fix looks like, and whether the issue is a real bug or a style violation.

For **pre-existing failures**, the command offers to create a fix branch. Instead of ignoring the problem or filing a ticket that sits in the backlog for months, `/review-pr` proposes a concrete path forward: a separate branch that addresses the pre-existing issue without polluting your PR's diff. This is the "fix it forward" philosophy — acknowledge the problem, isolate it from the current work, and make progress on both fronts.

The separation matters because it changes the conversation in code review. Reviewers no longer need to ask "is this CI failure related to your changes?" The classification is already done. Review cycles get shorter. Authors spend time on the feedback that matters — the design questions, the edge cases, the architectural concerns — instead of defending themselves against failures they didn't introduce.

## Lessons from Building It: 12 Tool Calls and 6 Reliability Fixes

The `/review-pr` command itself went through the gauntlet it was designed to solve. During PR #139, the original implementation wasted 12 tool calls due to 6 reliability issues — GitHub API pagination edge cases, malformed check-run queries, and timeout handling gaps. Every one of those issues was discovered, classified, and fixed in a reliability follow-up.

The experience validated the core design principle: CI causality analysis is harder than it looks, but the pattern is broadly useful. The same classification logic that separates "your fault" from "not your fault" applies to any project with a CI pipeline and a pull request workflow. The technique isn't specific to Unbound Force's toolchain — it's a general-purpose pattern for any team drowning in CI noise.

Three specific reliability improvements emerged from that process. First, pagination: GitHub's check-runs API paginates at 30 results, and the initial implementation silently dropped failures beyond the first page. Second, rate limiting: aggressive parallel API calls triggered GitHub's secondary rate limits, requiring exponential backoff. Third, ambiguous classification: some failures (like timeout-based flakes) required heuristic judgment rather than deterministic comparison, which the AI layer handles by examining failure patterns across recent runs.

## The Complete Review Lifecycle

The `/review-pr` command is one half of a review lifecycle. The other half is `/review-council`, which runs before you push — a multi-persona review that catches issues before they reach CI. Together, the two commands form a complete loop:

1. **Before pushing**: Run `/review-council` to get pre-PR feedback from multiple review perspectives (security, architecture, testing, operations). Fix findings before your code reaches the remote.
2. **After creating the PR**: Run `/review-pr` to classify CI results, separate signal from noise, and get actionable findings on any regressions your changes introduced.

The pre-push review catches design issues, missing tests, and convention violations while the code is still local and cheap to change. The post-PR review handles the integration reality — how your changes interact with CI, with the base branch, and with the broader test suite. Neither command replaces the other. They address different failure modes at different points in the development cycle.

## Stop Debugging Someone Else's Failures

Every minute spent investigating a pre-existing CI failure is a minute not spent shipping the feature your team is waiting for. The `/review-pr` command eliminates that waste by answering the question CI never answers: *was this your fault?*

The pattern is straightforward. Run one command after creating your PR. Get a classified report that separates your regressions from inherited problems. Fix what's yours, forward what isn't, and move on.

If your team loses hours per week to CI forensics, try `/review-pr` on your next pull request. Run `/review-pr` after creating a PR to get a classified report that separates your regressions from inherited problems. See the [Quick Start guide](/docs/getting-started/quick-start/) for installation, or explore the [Unbound Force repository](https://github.com/unbound-force/unbound-force) to see how causality classification works under the hood.
