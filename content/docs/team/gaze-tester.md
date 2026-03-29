---
title: "Gaze"
description: "Gaze is the Tester of the Unbound Force swarm — a quality analysis tool for Go that measures test quality through side effect detection and contract coverage."
lead: "The Quality Sentinel — Test Quality Analysis for Go"
date: 2026-02-23T00:00:00+00:00
draft: false
weight: 40
toc: true
---

![Gaze — Tester trading card](/images/team/gaze.png)

## The Quality Sentinel

Gaze is the Tester of the Unbound Force swarm -- a static analysis tool for Go that answers a question line coverage cannot: are your tests actually verifying the behavior that matters?

Traditional coverage metrics tell you what code executed during tests. Gaze goes further by detecting the **observable side effects** of each function (return values, state mutations, I/O operations) and checking whether your tests actually assert on those effects. The result is **contract coverage** -- a measure of what was verified, not just what ran.

## What Gaze Does Today

Gaze provides three core capabilities for Go codebases:

- **Contract Coverage** -- Measures the percentage of a function's observable side effects that are verified by assertions in tests. A function that writes to a database but whose tests never check the database state has low contract coverage even with 100% line coverage.
- **GazeCRAP Scores** -- Combines cyclomatic complexity with contract coverage (not line coverage) to identify functions that are both complex and poorly verified. High GazeCRAP scores indicate the riskiest code in your project.
- **Side Effect Classification** -- Categorizes every function by its side effect profile (P0 pure through P4 global state) so you know where to focus testing effort.

See the [Gaze project page](/docs/projects/gaze/) for detailed metrics, CLI usage, and current limitations.

## How Gaze Serves the Team

**For Developers (Cobalt-Crush):** When Cobalt-Crush commits code, Gaze provides immediate feedback on test quality. The quality report identifies functions with high CRAP scores (complex + poorly tested), low contract coverage (side effects not verified), and over-specified tests (assertions on internals rather than behavior). Cobalt-Crush uses this feedback to improve tests before submitting for review.

**For the Product Owner (Muti-Mind):** Muti-Mind uses Gaze's quality reports during acceptance decisions to verify that the delivered increment meets quality standards. Contract coverage and CRAP scores provide objective evidence of test quality beyond simple pass/fail results.

**For the Reviewer and Manager (The Divisor and Mx F):** The Divisor uses Gaze's green light as a prerequisite for review -- the CI pipeline must pass before the review council evaluates the code. Mx F tracks quality trends over time using Gaze's metrics snapshots, identifying patterns that inform process improvements.

## Next Steps

- Read the [Gaze project page](/docs/projects/gaze/) for installation, CLI commands, metrics definitions, and current limitations
- See the [Tester getting-started guide](/docs/getting-started/tester/) for practical usage in your daily workflow
- Read [Why Contract Coverage](/blog/why-contract-coverage/) to understand the theory behind Gaze's approach
- See [Gaze in Practice](/blog/gaze-in-practice/) for a walkthrough of a real quality report
