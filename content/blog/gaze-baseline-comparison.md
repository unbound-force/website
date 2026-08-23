---
title: "Gaze Baseline Comparison — Per-Function CRAP Regression Detection"
description: "Gaze now detects per-function CRAP score regressions by comparing against a baseline file. Zero configuration beyond creating the initial baseline — auto-detection handles the rest."
lead: "From external scripts to native regression detection. gaze crap auto-detects a baseline file and classifies every function as regression, improvement, new, or removed — one exit code gates your CI."
slug: "gaze-baseline-comparison"
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 106
toc: true
categories: ["Engineering"]
tags: ["gaze", "CRAP", "baseline", "CI", "testing"]
contributors: ["Unbound Force"]
---

## The 850-Line Comparison Script Problem

Organizations adopting Gaze for CRAP score analysis faced a gap: Gaze could compute per-function CRAP scores for a single run, but it had no way to compare those scores against a previous baseline. Detecting regressions — a function whose complexity increased or whose coverage dropped — required external tooling. The only option was org-infra's reusable GitHub Actions workflow, which embedded an 850-line bash/Python comparison script to diff two JSON reports and flag regressions.

That script worked, but it created a hard dependency on org-infra for every repository that wanted regression detection. Teams that couldn't use the reusable workflow had to rebuild the comparison logic from scratch. The result was fragmented implementations, inconsistent thresholds, and no local development story — engineers couldn't check for regressions before pushing to CI.

## Convention Over Configuration

Gaze now handles baseline comparison natively. The design follows a convention-over-configuration principle: create a `.gaze/baseline.json` file in your repository, and `gaze crap` auto-detects it on the next run. No flags, no config files, no environment variables. The baseline file is the configuration.

Generating the initial baseline is a single command:

```bash
gaze crap --json > .gaze/baseline.json
```

Commit that file to your repository. On subsequent runs, `gaze crap` detects `.gaze/baseline.json`, loads it, and compares every function in the current run against the baseline. Each function receives a classification:

- **regression** — CRAP score increased beyond the epsilon tolerance
- **improvement** — CRAP score decreased beyond the epsilon tolerance
- **new** — function exists in the current run but not in the baseline
- **removed** — function exists in the baseline but not in the current run

If any function is classified as a regression, `gaze crap` exits with code 1. No regressions means exit code 0. One exit code gates your entire CI pipeline.

## Example Output

When `gaze crap` detects a baseline, the output includes a comparison section after the standard CRAP table:

```text
CRAP Score Analysis
===================

  Function                          Complexity  Coverage  CRAP
  ────────────────────────────────  ──────────  ────────  ────
  parseConfig                              12    85.0%     14
  validateInput                             8    92.0%      9
  handleRequest                            15    40.0%     48
  formatOutput                              3   100.0%      3
  processBatch (new)                        6    78.0%      8

Baseline Comparison (.gaze/baseline.json)
=========================================

  Function        Baseline  Current  Delta   Status
  ──────────────  ────────  ───────  ──────  ───────────
  handleRequest       32       48    +16.0   REGRESSION
  parseConfig         18       14     -4.0   improvement
  validateInput        9        9      0.0   unchanged
  formatOutput         3        3      0.0   unchanged
  processBatch         —        8      —     new
  legacyParser        22        —      —     removed

Summary: 1 regression, 1 improvement, 1 new, 1 removed
Exit code: 1 (regression detected)
```

The `handleRequest` function regressed from CRAP 32 to CRAP 48 — its complexity increased while coverage dropped. That single regression triggers exit code 1, blocking the pipeline until the author addresses it.

## Configurable Tolerance

Two thresholds control how Gaze classifies functions during comparison. Both have sensible defaults that work without configuration, and both accept overrides for teams with specific requirements.

**Epsilon (default: 0.5)** absorbs platform noise. CRAP scores can fluctuate by small amounts across operating systems, Go versions, or test execution order. An epsilon of 0.5 means a function must change by more than 0.5 CRAP points to be classified as a regression or improvement. Scores that shift within the epsilon band are classified as unchanged.

```bash
# Tighten epsilon for high-precision environments
gaze crap --epsilon 0.1

# Loosen epsilon for projects with known platform variance
gaze crap --epsilon 1.0
```

**New-function threshold (default: 30)** enforces standards on new code. A function that appears for the first time with a CRAP score above 30 is classified as a regression, not as "new." This prevents authors from introducing high-complexity, low-coverage functions and bypassing the baseline gate because the function has no prior score to regress from.

```bash
# Enforce a stricter threshold for new functions
gaze crap --new-threshold 20
```

## CI Integration

Baseline comparison works identically in CI and local development. The same command, the same exit code, the same output. A GitHub Actions workflow that gates on CRAP regressions requires no special configuration beyond ensuring the baseline file is committed:

```yaml
name: Quality Gate
on: [pull_request]

jobs:
  crap-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-go@v5
        with:
          go-version: "1.23"

      - name: Install Gaze
        run: go install github.com/unbound-force/gaze/cmd/gaze@latest

      - name: Run tests with coverage
        run: go test -coverprofile=coverage.out ./...

      - name: CRAP baseline check
        run: gaze crap
        # Exit code 1 if any function regressed.
        # .gaze/baseline.json is auto-detected from the repo.
```

No wrapper scripts. No reusable workflow dependency. No post-processing step to parse JSON and decide pass/fail. The `gaze crap` command is the entire gate.

To update the baseline after intentional changes (refactoring that temporarily increases a score, or accepting a justified complexity increase), regenerate and commit:

```bash
gaze crap --json > .gaze/baseline.json
git add .gaze/baseline.json
git commit -m "chore: update CRAP baseline"
```

## Constitution Alignment

This feature directly fulfills Principle III (Actionable Output) from the Unbound Force constitution. Principle III requires that tool output be comparable across runs — a single-run CRAP report is informative, but it cannot answer the question "did this change make things worse?" without a reference point. Baseline comparison closes that gap by making cross-run comparability a native capability rather than an external integration concern.

The classification model (regression, improvement, new, removed) also satisfies the actionability requirement. Each classification maps to a concrete action: regressions must be fixed, improvements validate refactoring effort, new functions are evaluated against the threshold, and removed functions confirm cleanup. No classification leaves the engineer wondering "what do I do with this information?"

## Try It

Set up baseline comparison in your own repository with the companion tutorial: [Setting Up CRAP Baseline Comparison in CI](/docs/tutorials/gaze-crap-baseline-ci/). The tutorial walks through generating the initial baseline, configuring thresholds for your project, and integrating the gate into an existing GitHub Actions workflow.

Install Gaze and generate your first baseline:

```bash
go install github.com/unbound-force/gaze/cmd/gaze@latest
go test -coverprofile=coverage.out ./...
gaze crap --json > .gaze/baseline.json
git add .gaze/baseline.json
git commit -m "chore: add CRAP baseline"
```

From that point forward, every `gaze crap` run compares against the baseline and exits non-zero on regression. No configuration files to maintain, no external scripts to keep in sync, no CI-specific behavior that diverges from local development.
