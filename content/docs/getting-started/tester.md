---
title: "Getting Started: Tester"
description: "Use Gaze for quality analysis, understand contract coverage and CRAP scores, and integrate quality checks into your CI pipeline."
lead: "Analyze code quality with Gaze, run the review council, and enforce coverage ratchets in CI."
date: 2026-03-22T00:00:00+00:00
draft: false
weight: 40
toc: true
---

## Using Gaze

[Gaze](/docs/projects/gaze/) is the Tester hero -- a static analysis tool that answers the question line coverage cannot: _are your tests actually verifying what matters?_

Gaze detects every observable side effect a function produces, classifies each as contractual (public obligation) or incidental (implementation detail), and measures whether tests assert on the contractual effects.

### Core Commands

```bash
# Detect all observable side effects in a package
gaze analyze --classify ./pkg

# Assess test quality (contract coverage, over-specification)
gaze quality ./pkg

# Compute CRAP and GazeCRAP risk scores
gaze crap ./...

# Generate an AI-powered comprehensive quality report
gaze report ./... --ai=claude
```

| Command        | What It Produces                                                               |
| -------------- | ------------------------------------------------------------------------------ |
| `gaze analyze` | Side effect inventory (30+ types across 5 priority tiers)                      |
| `gaze quality` | Contract coverage percentage and over-specification score                      |
| `gaze crap`    | CRAP score (complexity + coverage risk) and GazeCRAP (using contract coverage) |
| `gaze report`  | Combined analysis formatted as a human-readable markdown report via AI         |

Install Gaze via Homebrew:

```bash
brew install unbound-force/tap/gaze
```

## Quality Metrics

Gaze introduces three metrics that go beyond line coverage.

### Contract Coverage

Traditional line coverage tells you which lines ran during tests. Contract coverage tells you whether tests _verified the function's actual obligations_.

**Formula**: `(contractual effects asserted on / total contractual effects) x 100`

A function that writes to a database, returns a result, and logs a message has three side effects. If tests assert on the return value and the database write but not the log message, contract coverage considers only the first two (contractual) -- the log is incidental. Coverage: 100% (2 of 2 contractual effects verified).

| Threshold | Meaning                                            |
| --------- | -------------------------------------------------- |
| >= 80%    | Good -- tests verify the function's obligations    |
| 50-79%    | Warning -- some contractual effects are unverified |
| < 50%     | Bad -- tests are missing critical assertions       |

### Side Effects

Gaze categorizes side effects into 5 priority tiers:

- **P0** (highest): Return values, error returns, receiver mutations, pointer argument mutations
- **P1**: Slice/map mutations, global mutations, writer output, HTTP response writes, channel operations
- **P2**: File system operations, database writes, goroutine spawns, panics, context cancellation
- **P3-P4**: Standard output, environment variables, unsafe operations (detection planned for future versions)

Each effect is classified as **contractual** (part of the function's public API -- tests should assert on these) or **incidental** (implementation detail -- tests should not depend on these).

### CRAP Scores

CRAP (Change Risk Anti-Patterns) combines cyclomatic complexity with test coverage to identify risky code:

**Formula**: `complexity^2 x (1 - coverage)^3 + complexity`

A function with complexity 5 and 0% coverage scores CRAP 30 (risky). The same function with 100% coverage scores CRAP 5 (safe). The default threshold is 15.

**GazeCRAP** replaces line coverage with contract coverage in the formula, providing a more accurate risk assessment. High GazeCRAP means the function is complex _and_ its tests don't verify its obligations.

## The Hero Workflow

Gaze is stage 3 (validate) in the [hero lifecycle](/docs/getting-started/common-workflows/#new-feature-end-to-end). After the developer implements a feature, Gaze validates the code quality before review.

### How Quality Reports Flow

1. **Gaze produces** a quality report with contract coverage, CRAP scores, and over-specification findings
2. **The Divisor consumes** the report during code review -- the Testing persona uses it as evidence for coverage assessment
3. **Muti-Mind consumes** the report during acceptance -- the PO compares quality metrics against the backlog item's acceptance criteria
4. **Mx F consumes** the report during the reflect stage -- quality trends feed into the retrospective analysis, sprint health dashboards, and coaching observations

## Running the Review Council

As a tester, you can invoke the review council to get a comprehensive code review:

```
/review-council
```

The council launches 5 Divisor personas in parallel. The **Testing persona** is particularly relevant -- it evaluates:

- Test architecture and organization
- Coverage strategy (is the right code being tested?)
- Assertion depth (are assertions meaningful?)
- Test isolation (no shared mutable state?)
- Regression protection (would these tests catch common regressions?)

See [Common Workflows: Code Review](/docs/getting-started/common-workflows/#code-review) for the full review process.

## Coverage Ratchets and CI

Coverage ratchets prevent test quality from degrading over time. Once a coverage threshold is reached, CI fails if it drops below that level.

### CI Integration Pattern

```bash
# 1. Run tests with coverage profile
go test -race -count=1 -coverprofile=coverage.out ./...

# 2. Run Gaze with the coverage profile and enforcement thresholds
gaze report ./... --ai=claude \
  --coverprofile=coverage.out \
  --max-crapload=15 \
  --min-contract-coverage=50

# 3. Gaze auto-appends to GitHub Actions step summary
```

### How Ratchets Work

- **`--max-crapload=N`**: Fail if more than N functions exceed the CRAP threshold. Start generous (e.g., 15) and tighten over time.
- **`--min-contract-coverage=N`**: Fail if contract coverage drops below N%. Start at 50% and ratchet up as tests improve.
- Without threshold flags, Gaze always exits 0 (report-only mode) -- useful for initial assessment before enforcing thresholds.

### GitHub Actions Integration

Gaze detects the `$GITHUB_STEP_SUMMARY` environment variable and automatically appends its report to the GitHub Actions job summary. No additional configuration needed.

## Knowledge Retrieval with Dewey

When [Dewey](/docs/getting-started/knowledge/) is configured, Gaze uses it to inform quality analysis with cross-repository context. Instead of evaluating code quality in isolation, Gaze can reference patterns and baselines from across the organization.

Dewey enables three capabilities for the tester role:

- **Quality pattern discovery**: Search for test patterns and quality strategies used in other repositories — how did another project achieve high contract coverage for a similar code structure?
- **CRAP score baselines**: Compare CRAP scores and complexity trends across repositories to establish organizational quality baselines and identify outliers
- **Known failure modes**: Surface past test failures, bug reports, and quality findings for similar code patterns — if a particular design pattern has historically caused issues, Gaze can flag it proactively

When Dewey is not available, Gaze operates on local code analysis and coverage data. All core capabilities — side effect detection, contract coverage, CRAP scoring, and quality reports — work without Dewey. Cross-repo quality context is an enhancement that improves the depth of analysis but is never required. See the [graceful degradation tiers](/docs/getting-started/knowledge/#graceful-degradation) for details.

## Next Steps

- Explore the full [Gaze documentation](/docs/projects/gaze/) for detailed command reference
- Read [Common Workflows](/docs/getting-started/common-workflows/) to see how validation fits the full lifecycle
- Try `gaze report ./... --format=json` for machine-readable output in CI pipelines
