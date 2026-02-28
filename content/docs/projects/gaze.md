---
title: "Gaze"
description: "Test quality analysis via side effect detection for Go — Contract Coverage, Over-Specification Score, and GazeCRAP metrics that go beyond line coverage."
lead: "Test quality analysis via side effect detection for Go."
date: 2026-02-23T00:00:00+00:00
draft: false
weight: 20
toc: true
---

## What Gaze Does

Line coverage tells you which lines ran. It does not tell you whether your tests actually verified anything meaningful.

A function can have 90% line coverage and tests that assert on nothing contractually important — logging calls, goroutine lifecycle, internal stdout writes — while leaving the return values, error paths, and state mutations completely unverified. That function is dangerous to change, and traditional coverage metrics will not warn you.

Gaze fixes this by working from first principles:

1. **Detect** every observable side effect a function produces — return values, error returns, mutations, I/O, channel sends, and more.
2. **Classify** each effect as _contractual_ (part of the function's public obligation), _incidental_ (an implementation detail), or _ambiguous_.
3. **Measure** whether your tests actually assert on the contractual effects — and flag the ones they do not.

Gaze requires no annotations, no test framework changes, and no restructuring of your code. It analyzes your existing Go packages as-is.

## Key Metrics

### Contract Coverage

The percentage of a function's contractual side effects that at least one test assertion verifies.

```text
ContractCoverage% = (contractual effects asserted on / total contractual effects) x 100
```

A function with 90% line coverage but 20% contract coverage has tests that run code without checking correctness. Gaze surfaces the specific effects that have no assertion — the exact gaps you need to close.

| Range         | Status  |
| ------------- | ------- |
| 80% or higher | Good    |
| 50-79%        | Warning |
| Below 50%     | Bad     |

### Over-Specification Score

The count and ratio of test assertions that target _incidental_ effects — implementation details that are not part of the function's contract.

```text
OverSpec.Ratio = incidental assertions / total mapped assertions
```

Tests that assert on log output, goroutine lifecycle, or internal stdout will break during refactoring even when the function's actual contract is preserved. Gaze identifies each over-specified assertion and explains why it is fragile.

| Count       | Status  |
| ----------- | ------- |
| 0           | Good    |
| 1-3         | Warning |
| More than 3 | Bad     |

### GazeCRAP

A composite risk score that replaces line coverage with contract coverage in the CRAP (Change Risk Anti-Patterns) formula.

```text
CRAP(m)     = complexity^2 x (1 - lineCoverage/100)^3 + complexity
GazeCRAP(m) = complexity^2 x (1 - contractCoverage)^3 + complexity
```

Functions are placed in a quadrant based on both scores:

|               | Low GazeCRAP       | High GazeCRAP             |
| ------------- | ------------------ | ------------------------- |
| **Low CRAP**  | Safe               | Simple but underspecified |
| **High CRAP** | Complex but tested | **Dangerous**             |

The **Dangerous** quadrant — complex functions whose tests do not verify their contracts — is the highest-priority target for remediation.

## Using Gaze

Gaze integrates with [OpenCode](https://opencode.ai) through the `/gaze` slash command. This is the primary way to use Gaze for test quality analysis.

### Setup

First, install the Gaze binary:

```bash
brew install unbound-force/tap/gaze
```

Or via Go:

```bash
go install github.com/unbound-force/gaze/cmd/gaze@latest
```

Then scaffold the OpenCode integration in your Go project:

```bash
gaze init
```

This creates four files in `.opencode/` that wire up the `/gaze` command and a quality reporting agent.

### The `/gaze` Command

Inside OpenCode, use `/gaze` to get AI-assisted quality reports:

```text
/gaze                           # Full report for entire module
/gaze crap ./internal/store     # CRAP scores only
/gaze quality ./pkg/api         # Contract Coverage metrics only
```

### Three Modes

| Mode           | What You Get                                                                |
| -------------- | --------------------------------------------------------------------------- |
| Full (default) | CRAP Summary + Quality Summary + Classification Summary + Health Assessment |
| `crap`         | CRAPload count, top 5 worst scores, GazeCRAP quadrant distribution          |
| `quality`      | Contract coverage, gaps, over-specification, worst tests                    |

The `/gaze` command routes to a quality reporting agent that runs the analysis, interprets the JSON output, and produces human-readable markdown summaries with tables, key metrics, and actionable recommendations.

A companion command, `/classify-docs`, enhances classification by combining mechanical signals with project documentation analysis.

## Architecture

Gaze is structured as a set of focused packages:

| Package    | Purpose                                                           |
| ---------- | ----------------------------------------------------------------- |
| `analysis` | Side effect detection engine — the core analysis orchestrator     |
| `taxonomy` | Side effect type system with stable IDs and tier classification   |
| `classify` | Contractual classification engine                                 |
| `config`   | Configuration file handling (`.gaze.yaml`)                        |
| `loader`   | Go package loading wrapper                                        |
| `report`   | JSON and text output formatters                                   |
| `crap`     | CRAP score computation and reporting                              |
| `quality`  | Test quality assessment — contract coverage and assertion mapping |
| `docscan`  | Documentation file scanner for enhanced classification            |
| `scaffold` | OpenCode file scaffolding for `/gaze` command setup               |

The analysis engine detects side effects across three implemented tiers (P0, P1, P2), covering the most common and impactful effect types — from return values and error returns through mutations, I/O, channel operations, and more.

## Current Limitations

Gaze is actively developed. The current scope has known boundaries:

- **Direct function body only.** Gaze analyzes the immediate function body. Transitive side effects (effects produced by called functions) are out of scope for v1.
- **P3-P4 side effects not yet detected.** The taxonomy defines types for stdout/stderr writes, environment mutations, mutex operations, reflection, and unsafe, but detection logic for these tiers is not yet implemented.
- **Assertion mapping accuracy is ~78.8%.** The target is 90%. Accuracy is primarily limited by helper function assertions and testify field-access patterns (tracked as [GitHub Issue #6](https://github.com/unbound-force/gaze/issues/6)).
- **No CGo or unsafe analysis.** Functions using `cgo` or `unsafe.Pointer` are not analyzed for their specific side effects.
- **Single package loading.** The `analyze` command processes one package at a time.

## Learn More

- [GitHub Repository](https://github.com/unbound-force/gaze) — source code, issues, and releases
- [Why Contract Coverage?](/blog/why-contract-coverage/) — a deeper look at why line coverage is not enough and how contract coverage changes the way you think about test quality
