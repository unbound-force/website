---
title: "Setting Up CRAP Baseline Comparison in CI"
description: "Step-by-step tutorial for setting up per-function CRAP regression detection using gaze's baseline comparison feature in GitHub Actions."
lead: "Detect CRAP score regressions on every PR. Create a baseline, add a CI step, and gaze handles the rest — zero wrapper scripts required."
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 30
toc: true
---

## Prerequisites

Before you begin, make sure you have the following in place:

1. **gaze installed** — Install via Homebrew or Go:

   ```bash
   # Homebrew (macOS/Linux)
   brew install unbound-force/tap/gaze

   # Or install from source
   go install github.com/unbound-force/gaze/cmd/gaze@latest
   ```

2. **A Go project with tests** — gaze computes CRAP scores from coverage data, so your project needs a test suite that produces a Go coverage profile.

3. **A CI pipeline using GitHub Actions** — The examples in this tutorial target GitHub Actions, but the baseline comparison feature works in any CI system that can run shell commands.

## Create the Initial Baseline

The baseline is a JSON snapshot of every function's CRAP score in your project. gaze compares future runs against this snapshot to detect regressions.

1. Run your tests and generate a coverage profile:

   ```bash
   go test -coverprofile=coverage.out ./...
   ```

2. Generate the baseline file:

   ```bash
   mkdir -p .gaze
   gaze crap --format=json --coverprofile=coverage.out ./... > .gaze/baseline.json
   ```

3. Commit the baseline to version control:

   ```bash
   git add .gaze/baseline.json
   git commit -m "chore: add gaze CRAP baseline"
   git push
   ```

The baseline file captures the CRAP score for every function in your codebase at this point in time. All future CI runs compare against these scores to flag regressions.

## CI Integration

gaze auto-detects the baseline file at `.gaze/baseline.json`. When the file exists, gaze automatically compares current scores against it and reports regressions — no extra flags or wrapper scripts required.

Add a CRAP regression check step to your GitHub Actions workflow:

```yaml
name: CRAP Regression Check

on:
  pull_request:
    branches: [main]

permissions:
  contents: read

jobs:
  crap-check:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2

      - name: Set up Go
        uses: actions/setup-go@d35c59abb061a4a6fb18e82ac0862c26744d6ab5 # v5.5.0
        with:
          go-version-file: go.mod

      - name: Install gaze
        run: go install github.com/unbound-force/gaze/cmd/gaze@latest

      - name: Run tests with coverage
        run: go test -coverprofile=coverage.out -count=1 -race ./...

      - name: Check CRAP regressions
        run: gaze crap --coverprofile=coverage.out ./...
```

The final step runs gaze with the coverage profile. Because `.gaze/baseline.json` exists in the repository, gaze automatically loads it and compares every function's current CRAP score against the baseline. If any function regressed, gaze exits with a non-zero status code and the CI step fails.

## Understanding the Output

When gaze detects differences between the current run and the baseline, it classifies each function into one of four categories:

- **Regression** — The function's CRAP score increased beyond the epsilon threshold. This fails CI.
- **Improvement** — The function's CRAP score decreased. This is informational and does not fail CI.
- **New** — The function did not exist in the baseline. gaze applies the new-function threshold (default: 30) to decide whether this fails CI.
- **Removed** — The function existed in the baseline but no longer exists. This is informational.

Here is an example of gaze output with baseline comparison:

```text
CRAP Baseline Comparison
========================

Regressions (score increased):
  internal/parser.Parse        12.4 → 38.7  (+26.3)  FAIL
  internal/parser.Validate      8.1 → 15.2  (+7.1)   FAIL

Improvements (score decreased):
  internal/lexer.Tokenize      22.0 → 6.3   (-15.7)

New functions:
  internal/parser.Normalize    4.2           PASS (below threshold 30)
  internal/parser.Transform    42.8          FAIL (above threshold 30)

Removed functions:
  internal/parser.OldParse     (was 18.3)

Summary: 3 regressions, 1 improvement, 2 new, 1 removed
Exit code: 1 (regressions detected)
```

Regressions and new functions above the threshold cause a non-zero exit code. Improvements and removals are reported but do not block the pipeline.

## Tuning Sensitivity

gaze uses two thresholds to control sensitivity:

- **Epsilon** (default: `0.5`) — The minimum score increase that counts as a regression. Small fluctuations below this value are ignored. Raise this value if you see false positives from minor refactors.
- **New function threshold** (default: `30`) — The maximum CRAP score allowed for functions that do not appear in the baseline. Lower this value to enforce stricter standards on new code.

Configure both values in a `.gaze.yaml` file at the root of your repository:

```yaml
crap:
  baseline:
    epsilon: 1.0
    new_threshold: 20
```

With this configuration, a function must increase by more than 1.0 CRAP points to trigger a regression, and new functions must score below 20 to pass. Commit `.gaze.yaml` alongside your baseline so that all contributors and CI use the same thresholds.

## Refreshing the Baseline

The baseline represents your accepted CRAP scores at a point in time. Refresh it when the current scores no longer reflect reality:

- **After a release** — Lock in the post-release scores as the new standard.
- **After intentional complexity changes** — If you deliberately added complexity (e.g., expanded a parser), update the baseline so future runs compare against the new expected scores.
- **After large-scale refactoring** — A refactor that improves many functions should be captured so you get credit for the improvements.

To refresh the baseline:

1. Run the tests and regenerate the baseline:

   ```bash
   go test -coverprofile=coverage.out ./...
   gaze crap --format=json --coverprofile=coverage.out ./... > .gaze/baseline.json
   ```

2. Commit and push the updated baseline:

   ```bash
   git add .gaze/baseline.json
   git commit -m "chore: refresh gaze CRAP baseline"
   git push
   ```

Do not refresh the baseline on every PR. The baseline should change deliberately, not as a side effect of routine development. Frequent refreshes defeat the purpose of regression detection.

## Troubleshooting

### Stale Baseline

**Symptom**: gaze reports many "removed" functions and unexpected "new" functions.

**Cause**: The baseline was generated from a significantly older version of the codebase. Function signatures, package paths, or file locations changed since the baseline was created.

**Fix**: Regenerate the baseline from the current `main` branch and commit it. Going forward, refresh the baseline after major structural changes.

### Config Drift

**Symptom**: CI fails with regressions that pass locally, or vice versa.

**Cause**: The `.gaze.yaml` file on your local machine differs from the one in the repository, or you have local environment variables that override gaze settings.

**Fix**: Ensure `.gaze.yaml` is committed to the repository and that you pull the latest version before running gaze locally. Do not rely on local-only configuration for thresholds.

### Function Renames

**Symptom**: A renamed function appears as both "removed" (old name) and "new" (new name). The new entry may fail if it exceeds the new-function threshold.

**Cause**: gaze identifies functions by their fully qualified name (`package.Function`). A rename creates a new identity that has no baseline entry.

**Fix**: Refresh the baseline after renaming functions. If you rename functions frequently during a refactoring PR, refresh the baseline in a preparatory commit on `main` before opening the refactoring PR.
