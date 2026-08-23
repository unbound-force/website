---
title: "Gaze"
description: "Test quality analysis via side effect detection — Contract Coverage, Over-Specification, and GazeCRAP. Go-native with multi-language support."
lead: "Test quality analysis via side effect detection — Go-native, with multi-language support via external analyzers."
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

For Go projects, Gaze requires no annotations, no test framework changes, and no restructuring of your code. It analyzes your existing packages as-is. Other languages are supported through [external analyzers](#external-analyzers).

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

This creates 8 files across 3 directories in `.opencode/` that wire up the `/gaze` command, a quality reporting agent, and AI-assisted test generation:

| Directory               | Files                                                               |
| ----------------------- | ------------------------------------------------------------------- |
| `.opencode/agents/`     | `gaze-reporter.md`, `gaze-test-generator.md`, `reviewer-testing.md` |
| `.opencode/command/`    | `gaze.md`, `gaze-fix.md`, `speckit.testreview.md`                   |
| `.opencode/references/` | `doc-scoring-model.md`, `example-report.md`                         |

Notable scaffolded files:

- **`reviewer-testing.md`** — The Tester Divisor persona for code review. When the review council runs, this agent evaluates test architecture, coverage strategy, assertion depth, and test isolation. It uses Gaze quality data (when available) to ground its findings in concrete CRAP scores and coverage numbers.
- **`speckit.testreview.md`** — A read-only spec testability analysis command (`/speckit.testreview`). Evaluates whether a spec's requirements are testable before implementation begins — checking that acceptance criteria are specific enough to write tests from.

### The `/gaze` Command

Inside OpenCode, use `/gaze` to get AI-assisted quality reports:

```text
/gaze                           # Full report for entire module
/gaze crap ./internal/store     # CRAP scores only
/gaze quality ./pkg/api         # Contract Coverage metrics only
```

### CLI Usage

Gaze can also be run directly from the command line:

```bash
gaze report ./... --ai=opencode   # Full report using OpenCode adapter
gaze report ./... --ai=claude     # Full report using Claude adapter
```

Both `--ai=opencode` and `--ai=claude` are fully supported AI backends for `gaze report`.

### CLI Flags

| Flag                   | Commands                      | Description                                                                                                                                                    |
| ---------------------- | ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--include-unexported` | `analyze`, `quality`          | Include unexported functions in the analysis. Auto-detected per-package for `package main` (see note below). |
| `--ai-mapper`          | `quality`, `crap`             | Enable AI-assisted assertion mapping as a 5th pass (confidence 50). Uses the configured AI adapter to evaluate structurally disconnected assertions. |
| `--max-gaze-crapload`  | `report`                      | Fail if more than N functions exceed the GazeCRAP threshold. Similar to `--max-crapload` but uses contract coverage instead of line coverage. |
| `--coverprofile`       | `report`                      | Pass a pre-generated `go test -coverprofile` to skip Gaze's internal test run. See the [Tester guide](/docs/getting-started/tester/) for the CI pattern. |
| `--analyzer <binary>`  | `crap`, `quality`, `report`   | Specify an external analyzer binary implementing the JSON-RPC 2.0 protocol. See [External Analyzers](#external-analyzers). |
| `--language <lang>`    | `crap`, `quality`, `report`   | Specify the target language when using an external analyzer (e.g., `python`, `javascript`). |
| `--test-short`         | `crap`, `report`, `self-check`| Pass `-short` to `go test` during Gaze's internal coverage run. By default, Gaze now runs the full test suite. |

**`--include-unexported` auto-detection**: When analyzing multiple packages, each package is checked individually for `package main`. CLI entry points are included automatically without needing this flag.

### External Analyzers

Gaze supports languages beyond Go through the external analyzer protocol. Any tool that implements the [JSON-RPC 2.0 analyzer protocol](https://github.com/unbound-force/gaze/blob/main/docs/protocol.md) can provide side effect data to Gaze, enabling the same Contract Coverage and CRAP metrics for non-Go codebases.

Gaze discovers external analyzers through a three-tier mechanism:

1. **CLI flag** — `--analyzer <binary>` on `gaze crap`, `gaze quality`, or `gaze report`
2. **Configuration** — the `analyzers` section in `.gaze.yaml` maps language names to analyzer binaries
3. **PATH convention** — binaries named `gaze-analyzer-<lang>` (e.g., `gaze-analyzer-python`) are discovered automatically

Use the `--language <lang>` flag to specify the target language when running with an external analyzer. Gaze also supports a streaming mode for large codebases; see [the protocol documentation](https://github.com/unbound-force/gaze/blob/main/docs/protocol.md) for details.

### The `/gaze fix` Command

The `/gaze fix` command provides AI-assisted test generation to close coverage gaps. It reads quality data — contract coverage gaps and fix strategy labels — and generates Go test functions using the `gaze-test-generator` agent.

```text
/gaze fix                       # Fix coverage gaps for the module
```

The command is scaffolded by `gaze init` (as `.opencode/command/gaze-fix.md`) and works alongside the `/gaze` report command. Use `/gaze` to identify gaps, then `/gaze fix` to generate tests that close them.

### Fix Strategy Labels

Each function in a Gaze report carries a remediation label that tells you _how_ to improve it. These labels appear in both text and JSON output:

| Label                | Meaning                                             |
| -------------------- | --------------------------------------------------- |
| `add_tests`          | Function has no tests — write tests                 |
| `add_assertions`     | Tests exist but don't assert on contractual effects |
| `decompose`          | Function is too complex — split it                  |
| `decompose_and_test` | Both too complex and undertested                    |

The `/gaze fix` command uses these labels to determine the appropriate test generation strategy for each function.

## Sample Output

Here is what a full Gaze report looks like on a real Go project — [gcal-organizer](https://github.com/jflowers/gcal-organizer). This sample was generated with an earlier version of Gaze; current output may include additional metrics and formatting improvements.

### Overall Health Assessment

| Dimension         | Grade | Details                                        |
| ----------------- | ----- | ---------------------------------------------- |
| CRAPload          | 🔴 D  | 40/137 functions (29.2%) above threshold       |
| GazeCRAPload      | 🟢 A- | 5/17 analyzed functions above threshold        |
| Avg Line Coverage | 🔴 D  | 26.2% — 101 of 137 functions have 0% coverage  |
| Contract Coverage | 🟡 C  | 52.9% avg across 17 quality-analyzed functions |
| Complexity        | 🟢 B+ | Average 4.9, but 40 functions exceed threshold |

### Top 5 Worst CRAP Scores

| Function                         |  CRAP | Complexity | Coverage | File                                   |
| -------------------------------- | ----: | ---------: | -------: | -------------------------------------- |
| (\*Service).CreateDecisionsTab   | 650.0 |         25 |     0.0% | internal/docs/service.go:460           |
| runBrowserScript                 | 342.0 |         18 |     0.0% | cmd/gcal-organizer/assign_tasks.go:237 |
| loadDotEnv                       | 240.0 |         15 |     0.0% | cmd/gcal-organizer/main.go:382         |
| (\*Service).ListMeetingDocuments | 210.0 |         14 |     0.0% | internal/drive/service.go:113          |
| (\*Organizer).printSummary       | 156.0 |         12 |     0.0% | internal/organizer/organizer.go:227    |

For a section-by-section walkthrough of the full report — including GazeCRAP quadrants, quality analysis, classification, and prioritized recommendations — see [Gaze in Practice](/blog/gaze-in-practice/).

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
| `aireport` | AI CLI adapter integration for `gaze report` AI pipeline          |
| `scaffold` | OpenCode file scaffolding for `/gaze` command setup               |
| `protocol` | JSON-RPC 2.0 external analyzer protocol implementation           |
| `provider` | Analyzer provider abstraction and discovery logic                 |
| `adapter`  | Language-specific adapter wiring for external analyzers           |
| `cliutil`  | Shared CLI utilities and flag handling                             |

The analysis engine detects side effects across three implemented tiers (P0, P1, P2), covering the most common and impactful effect types — from return values and error returns through mutations, I/O, channel operations, and more.

### Universal Side Effect Types

With the external analyzer protocol, Gaze's taxonomy has expanded to 48+ types including 10 universal types that apply across languages:

- **P0**: `ErrorSignal` — language-agnostic error signaling (exceptions, error returns, panics)
- **P1**: `GeneratorYield`, `ContainerMutation`, `StreamOutput` — iterator/generator effects, collection mutations, stream writes
- **P2**: `AsyncGeneratorYield`, `MetaprogrammingMutation`, `DescriptorEffect`, `ResourceManagement`, `ImportSideEffect`, `MonkeyPatch` — async generators, metaprogramming, descriptors, resource lifecycle, import effects, runtime patching

Each side effect can carry a `Detail` metadata field (`map[string]any`) for language-specific context that external analyzers can populate.

For the complete type reference with all 48+ types, see the [protocol documentation](https://github.com/unbound-force/gaze/blob/main/docs/protocol.md).

### Analysis Engine Details

These details are primarily relevant for understanding Gaze's output and accuracy characteristics:

- **SSA degradation diagnostics.** When SSA (Static Single Assignment) construction fails for a package (e.g., due to upstream toolchain issues), Gaze degrades gracefully instead of failing. The CRAP output includes `ssa_degraded_packages` listing affected packages, and the text report shows an SSA diagnostics section. Metrics for degraded packages are based on AST-only analysis with reduced accuracy.
- **AST mutation fallback.** When SSA analysis fails, Gaze falls back to AST-based mutation detection for side effect identification. This is transparent to users but may affect accuracy for complex data flow patterns. The degradation is surfaced via the SSA diagnostics above.
- **`no_test_coverage` classification.** Contract coverage distinguishes between functions that have side effects but no test coverage (`no_test_coverage`) and functions that have no detectable effects. This separation ensures the fix strategy labels recommend "write tests" rather than "add assertions" for completely untested functions.
- **Constructor naming signal.** Functions with `New` or `NewXxx` naming prefixes receive a classification signal that biases toward constructor patterns, improving contractual classification accuracy for factory functions.
- **Reduced GoDoc signal.** GoDoc keywords contribute a reduced signal (+5) to non-matching effect types, preventing documentation-derived classification from overwhelming mechanical signals.
- **Container unwrap mapping.** Assertion mapping handles field access and transformation chains (e.g., JSON unmarshal followed by field access followed by type assertion) at confidence 55, improving mapping accuracy for assertions that verify deeply nested return values.

## Current Limitations

Gaze is actively developed. The current scope has known boundaries:

- **Direct function body only.** Gaze analyzes the immediate function body. Transitive side effects (effects produced by called functions) are out of scope for v1.
- **P3-P4 side effects not yet detected (Go analysis).** P0 through P2 are fully implemented — covering return values, error returns, mutations, I/O, channel operations, file system operations, database writes, goroutine spawns, panics, and context cancellation. P3-P4 side effects (stdout/stderr, environment mutations, mutex operations, reflection, unsafe) are not yet detected in Go analysis. External analyzers define their own detection scope.
- **Assertion mapping accuracy is ~84.7%** (83/98 mapped assertions, ratchet floor 84.0%). The target is 90%. Accuracy is primarily limited by helper function assertions and testify field-access patterns (tracked as [GitHub Issue #6](https://github.com/unbound-force/gaze/issues/6)).
- **No CGo or unsafe analysis.** Functions using `cgo` or `unsafe.Pointer` are not analyzed for their specific side effects.
- **No transitive multi-module analysis.** All four commands (`analyze`, `quality`, `crap`, `report`) accept multiple package patterns including `./...` wildcards, but analysis is scoped to the current module. Cross-module dependency analysis is out of scope for v1.

## Migration Notes

If you are upgrading from an earlier version of Gaze, the following breaking changes may affect your workflow.

### JSON Output Changes

- The `go_version` field in all JSON output has been renamed to `language_version`. Update any scripts or CI pipelines that parse this field.
- A new `language` field has been added to JSON output (e.g., `"language": "go"`).
- Seven language-neutral `SideEffectType` aliases have been added: `AsyncTaskSpawn`, `FFICall`, `ErrorSignal`, `GeneratorYield`, `ContainerMutation`, `StreamOutput`, and `ResourceManagement`. These are equivalent to existing Go-specific types but use language-agnostic names.

### Coverage Behavior Change

- Gaze no longer passes `-short` to `go test` during internal coverage runs. Coverage now reflects the full test suite by default.
- Use `--test-short` to restore the previous behavior if your test suite has expensive integration tests you want to skip during coverage collection.
- The `GAZE_COVERAGE_RUN=1` environment variable is set during Gaze's internal test runs, allowing tests to detect when they are running under Gaze coverage.
- **CRAP scores may change after upgrade.** Because coverage now includes the full test suite, functions that were previously measured with `-short` coverage will have different coverage numbers — and therefore different CRAP and GazeCRAP scores.

## Learn More

- [GitHub Repository](https://github.com/unbound-force/gaze) — source code, issues, and releases
- [Why Contract Coverage?](/blog/why-contract-coverage/) — a deeper look at why line coverage is not enough and how contract coverage changes the way you think about test quality
