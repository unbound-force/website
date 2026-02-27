---
title: "Why Contract Coverage: How Gaze Redefines What It Means to Have Good Tests"
description: "Line coverage tells you what code ran, not what was verified. Contract Coverage measures whether tests check what functions are actually responsible for."
lead: "Line coverage tells you what code ran. Contract Coverage tells you what was verified."
slug: "why-contract-coverage"
date: 2026-02-27T00:00:00+00:00
draft: false
weight: 100
toc: true
categories: ["Engineering"]
tags: ["gaze", "testing", "go", "static-analysis"]
contributors: ["Unbound Force"]
---

## The Problem with Line Coverage

Most teams gate their CI pipelines on line coverage. The metric is simple: what percentage of your code was executed during tests? But execution is not verification. A test suite can achieve 90% line coverage without containing a single meaningful assertion — every function runs, but nothing checks whether the results are correct.

This is not a theoretical concern. Research by Zhang and Mesbah analyzing 6,700 test suites across major open-source Java projects found that assertion coverage — the fraction of code whose output is actually evaluated by an assertion — is the strongest predictor of fault-detection effectiveness, far more than line or branch coverage alone. Separate work on "checked coverage" (which traces backward from assertions to the code that influenced them) has shown that a codebase boasting 85% statement coverage can have as little as 20% checked coverage. The gap between "code that ran" and "code whose behavior was verified" is enormous.

Line coverage creates two additional problems:

**It punishes refactoring.** When you restructure internal logic — extract a helper, merge functions, optimize an algorithm — line coverage numbers shift even if external behavior is unchanged. Worse, tests that assert on implementation details (mock call counts, internal method invocations, intermediate state) break on every refactor, even when the function still does exactly what it is supposed to do.

**It incentivizes the wrong behavior.** When teams are told to raise coverage by 10%, the easiest path is writing assertion-free tests that exercise code paths without checking outcomes. The coverage number goes up. Confidence in the test suite should not.

## What Gaze Measures Instead

Gaze introduces **Contract Coverage**: the ratio of a function's _design responsibilities_ that its tests actually verify.

```
Contract Coverage = contractual side effects asserted on / total contractual side effects
```

The key insight is that not every observable effect of a function matters equally. Some effects are the reason the function exists — its contractual obligations to the rest of the system. Others are implementation details that could change during a refactor without affecting correctness. Gaze distinguishes between them.

### The Pipeline

Gaze operates as a static analysis tool for Go. Its core side effect detection and contract classification require no test execution — they work entirely from source code. (The `crap` subcommand can optionally invoke `go test -coverprofile` to generate line coverage data for the traditional CRAP formula, but the contract analysis itself is purely static.)

Gaze works in three stages:

1. **Side Effect Detection.** Gaze analyzes a function's AST and SSA representation to enumerate every observable side effect: return values, error returns, pointer mutations, file I/O, database writes, channel operations, goroutine spawns, logging, and more — organized into a 5-tier taxonomy (P0 through P4) covering 38 distinct effect types.

2. **Contract Classification.** Each detected side effect is classified as _contractual_ (part of the function's design responsibility), _incidental_ (implementation detail), or _ambiguous_. Classification uses deterministic mechanical signals — interface satisfaction, caller dependency analysis within the module, exported API surface visibility, naming conventions, and godoc comments — each contributing a weighted score to a 0-100 confidence value. Optionally, an external workflow using OpenCode agents can analyze project documentation (architecture docs, specs, READMEs) to refine classification confidence further — this is not a built-in feature of the Gaze binary, but a supplementary process that feeds refined classifications back into the analysis.

3. **Assertion Mapping.** Gaze maps each test assertion back to the side effect it verifies, then computes the ratio. It also computes an **Over-Specification Score**: the count of _incidental_ side effects the test asserts on. These are the assertions that will break during a refactor even when the function's contract is preserved.

### What This Gets You

- **Contract Coverage** tells you whether the test verifies what the function is _supposed to do_. A score of 100% means every contractual obligation is checked. A score of 50% means half of the function's responsibilities are unguarded.

- **Over-Specification Score** tells you how fragile the test is. A score of 0 means the test only asserts on contractual effects — it will survive any refactor that preserves behavior. A score of 3 means three assertions are tied to implementation details and will break when internals change.

Neither metric cares how many lines of code were executed.

## A Concrete Example

Consider a Go function:

```go
func SaveUser(ctx context.Context, db *sql.DB, user *User) error {
    log.Printf("saving user %s", user.ID)

    _, err := db.ExecContext(ctx, "INSERT INTO users ...", user.Name, user.Email)
    if err != nil {
        return fmt.Errorf("save user: %w", err)
    }

    go invalidateCache(user.ID)

    return nil
}
```

Gaze detects four side effects:

| Side Effect                     | Type          | Classification |
| ------------------------------- | ------------- | -------------- |
| Return `nil` on success         | ReturnValue   | Contractual    |
| Return wrapped error on failure | ErrorReturn   | Contractual    |
| Database INSERT                 | DatabaseWrite | Contractual    |
| Log message                     | LogWrite      | Incidental     |

The goroutine spawn and log write are implementation details — the system's callers depend on the database write happening and the error being returned, not on whether a log line was emitted or how cache invalidation is scheduled internally.

**Scenario A:** The test checks the error return and queries the database to verify the row exists.

- Contract Coverage: **100%** (3/3 contractual effects verified)
- Over-Specification: **0**

**Scenario B:** The test only checks `err == nil`.

- Contract Coverage: **33%** (1/3 — the database write and success return are unverified)
- Over-Specification: **0**

**Scenario C:** The test checks `err == nil` and also asserts on the log output.

- Contract Coverage: **33%** (still 1/3 contractual)
- Over-Specification: **1** (the log assertion will break if you change the log format or remove logging)

Line coverage would report the same number for all three scenarios. Contract Coverage reveals that Scenario B and C are dangerously undertested, and that Scenario C is also brittle.

### CRAP vs GazeCRAP

Gaze also computes composite risk scores. The industry-standard CRAP formula combines cyclomatic complexity with line coverage:

```
CRAP(m) = complexity^2 * (1 - lineCoverage/100)^3 + complexity
```

Gaze introduces **GazeCRAP**, which substitutes contract coverage for line coverage:

```
GazeCRAP(m) = complexity^2 * (1 - contractCoverage/100)^3 + complexity
```

For `SaveUser` with complexity 5:

| Metric   | Scenario A (100% contract cov, 90% line cov) | Scenario B (33% contract cov, 90% line cov) |
| -------- | -------------------------------------------- | ------------------------------------------- |
| CRAP     | 5.0                                          | 5.0                                         |
| GazeCRAP | 5.0                                          | 12.5                                        |

Traditional CRAP says both scenarios look safe. GazeCRAP reveals that Scenario B carries real risk — the function is complex enough to matter, and its contractual obligations are mostly unverified.

Gaze classifies functions into four quadrants based on both scores:

- **Q1 — Safe:** Low CRAP, low GazeCRAP. Simple and well-tested.
- **Q2 — Complex but contract-tested:** High CRAP, low GazeCRAP. May benefit from refactoring, but tests are solid.
- **Q3 — Simple but under-specified:** Low CRAP, high GazeCRAP. Easy to fix — just add the missing contractual assertions.
- **Q4 — Dangerous:** High CRAP _and_ high GazeCRAP. Complex code with tests that don't verify its responsibilities. Highest priority.

## How Gaze Differs from Existing Approaches

| Approach                        | What It Measures                       | Refactoring Resilience                | Runtime Cost                                                         | Scope                   |
| ------------------------------- | -------------------------------------- | ------------------------------------- | -------------------------------------------------------------------- | ----------------------- |
| **Line/Branch Coverage**        | Code execution paths                   | Low — shifts on any structural change | Low (instrumentation)                                                | Unit level              |
| **Mutation Testing**            | Assertion strength via fault injection | Moderate-High — validates post-facto  | High (recompile per mutant)                                          | Unit level              |
| **Checked Coverage**            | Dynamic backward slice from assertions | Moderate — traces data flow           | High (execution tracing)                                             | Unit level              |
| **API Contract Testing (Pact)** | Network boundary agreements            | High — decoupled from internals       | Low                                                                  | Service boundaries only |
| **Gaze Contract Coverage**      | Design responsibility verification     | High — anchored to role, not code     | None for contract analysis; optional test execution for CRAP scoring | Unit level              |

Key differentiators:

- **Static at its core.** Gaze's side effect detection and contract classification analyze source code without running tests — no runtime overhead, no instrumentation, no recompilation. The `crap` subcommand can optionally run `go test` to collect line coverage data for the traditional CRAP formula, but the contract analysis pipeline itself is purely static. Gaze works alongside your existing test runner.

- **Proactive, not reactive.** Mutation testing validates assertion strength by injecting faults _after_ tests exist. Gaze establishes what the assertions _should_ cover based on the function's design role, identifying gaps before they become escaped defects.

- **Unit-level contracts.** Tools like Pact enforce contracts at API boundaries between services. Gaze pushes the contract paradigm down to individual functions and methods within a codebase.

- **Refactoring-resilient by design.** Because the metric is anchored to the function's _role_ (what it is responsible for) rather than its _code lines_ (how it does it), Contract Coverage remains stable across refactors that preserve behavior. Rewrite the internals of `SaveUser` entirely — as long as it still writes to the database and returns errors correctly, the score does not change and the tests do not break.

## CI Gating on What Matters

Gaze provides threshold flags for pipeline enforcement:

```bash
# Fail the build if any test covers less than 80% of its target's contract
gaze quality ./internal/analysis --min-contract-coverage=80

# Fail if any test asserts on more than 2 incidental side effects
gaze quality ./internal/analysis --max-over-specification=2

# Fail if more than 5 functions are in the CRAP danger zone
gaze crap ./... --max-crapload=5

# Same threshold, but using contract coverage instead of line coverage
gaze crap ./... --max-gaze-crapload=3
```

Without threshold flags, Gaze runs in report-only mode (exit code 0) — useful for initial adoption and exploration before enforcing gates.

Gaze also includes a `self-check` command that runs its entire analysis pipeline against its own codebase, serving as both a dogfooding mechanism and a project health dashboard.

### What These Gates Actually Mean

A `--min-contract-coverage=80` gate answers a fundamentally different question than a `--min-coverage=80` gate. The traditional gate asks: "Did the tests touch 80% of the code?" Gaze's gate asks: "Do the tests verify at least 80% of what each function is _responsible for_?"

The first question can be satisfied by assertion-free tests that game the metric. The second cannot. Contract Coverage requires that test assertions exist and that they map to the function's actual design obligations — the side effects that the rest of the system depends on. There is no way to inflate the number without writing tests that genuinely verify behavior.

Combined with the Over-Specification gate, teams get a two-sided quality signal: tests must verify enough of the contract (coverage floor) while avoiding assertions on implementation details (brittleness ceiling). Functions that satisfy both constraints produce tests that are simultaneously thorough and refactoring-resilient — the precise outcome that line coverage was always supposed to deliver but structurally cannot.
