---
title: "Gaze in Practice: A Real-World Quality Report Walkthrough"
description: "A section-by-section walkthrough of an actual Gaze report run against a real Go project — what the numbers mean, what stands out, and what to do next."
lead: "What does a Gaze report actually look like? Here is a real one, section by section."
slug: "gaze-in-practice"
date: 2026-03-02T00:00:00+00:00
draft: false
weight: 90
toc: true
categories: ["Engineering"]
tags: ["gaze", "testing", "go", "quality"]
contributors: ["Unbound Force"]
---

## Why Real Output Matters

The [existing article on Contract Coverage](/blog/why-contract-coverage/) explains the theory — why line coverage is not enough, how Contract Coverage works, what GazeCRAP measures. But theory only gets you so far. If you are evaluating Gaze for your Go project, you want to know: what will I actually see when I run it?

This walkthrough answers that question. We ran Gaze against [gcal-organizer](https://github.com/jflowers/gcal-organizer), a Go-based Google Calendar organizer tool — a real side project with real gaps, not a contrived demo built to make the tool look good. The project has 137 functions across multiple packages, a mix of well-tested core logic and completely untested CLI code, and a recently added feature branch (decision extraction) that has not yet had any tests written for it.

The report below was generated using `/gaze` in OpenCode — the full report mode that combines CRAP analysis, contract quality assessment, side effect classification, and overall health scoring. Every number is reproduced exactly as Gaze produced it.

**Report metadata:**
- **Project**: github.com/jflowers/gcal-organizer
- **Branch**: 008-decision-extraction
- **Gaze Version**: v1.2.3
- **Go**: 1.24.12
- **Date**: 2026-03-02

## 📊 CRAP Summary

The CRAP (Change Risk Anti-Patterns) summary provides aggregate metrics across all functions in the module.

| Metric | Value |
|--------|------:|
| Total functions analyzed | 137 |
| Average complexity | 4.9 |
| Average line coverage | 26.2% |
| Average CRAP score | 29.7 |
| CRAPload | 40 (functions ≥ threshold 15) |

### What This Tells Us

The average complexity of 4.9 is reasonable — most functions are not individually complex. But 26.2% average line coverage is low, and a CRAPload of 40 means 29.2% of all functions (40 out of 137) exceed the CRAP threshold of 15. That is a significant chunk of the codebase where complexity and poor coverage combine to create change risk.

The average CRAP score of 29.7 is inflated by the tail — a handful of very complex, completely untested functions drive the average up. This is a common pattern in projects where core business logic has decent coverage but CLI entry points and infrastructure code have none.

## Top 5 Worst CRAP Scores

Gaze surfaces the functions with the highest individual CRAP scores — the specific places where change risk is concentrated.

| Function | CRAP | Complexity | Coverage | File |
|----------|-----:|----------:|---------:|------|
| (\*Service).CreateDecisionsTab | 650.0 | 25 | 0.0% | internal/docs/service.go:460 |
| runBrowserScript | 342.0 | 18 | 0.0% | cmd/gcal-organizer/assign_tasks.go:237 |
| loadDotEnv | 240.0 | 15 | 0.0% | cmd/gcal-organizer/main.go:382 |
| (\*Service).ListMeetingDocuments | 210.0 | 14 | 0.0% | internal/drive/service.go:113 |
| (\*Organizer).printSummary | 156.0 | 12 | 0.0% | internal/organizer/organizer.go:227 |

### What This Tells Us

All five worst functions have 0% line coverage. These are not simple getters — they have cyclomatic complexities of 12 to 25, meaning they contain significant branching logic (conditionals, loops, error paths) that has never been exercised by a test.

`CreateDecisionsTab` stands out with a CRAP score of 650 and complexity 25. This is the entry point for the newly added decision extraction feature — it was written on the current branch and has not yet had any tests written. At complexity 25, this function almost certainly needs to be decomposed before meaningful tests can be written against it.

The next three functions (`runBrowserScript`, `loadDotEnv`, `ListMeetingDocuments`) are infrastructure and CLI code. This is the pattern mentioned earlier: core business logic gets tested, but the glue code that wires things together and handles configuration gets ignored. These functions are risky precisely because they handle error-prone operations (browser automation, environment loading, API calls) with no safety net.

## GazeCRAP Quadrant Distribution

This is where Gaze goes beyond traditional CRAP. The GazeCRAP quadrant places each function on a 2x2 grid comparing its traditional CRAP score (complexity + line coverage) against its GazeCRAP score (complexity + contract coverage). For a deeper explanation of this distinction, see [Why Contract Coverage](/blog/why-contract-coverage/).

| Quadrant | Count | Meaning |
|----------|------:|---------|
| 🟢 Q1 — Safe | 12 | Low complexity, high contract coverage |
| 🟡 Q2 — Complex But Tested | 0 | High complexity, contracts verified |
| 🔴 Q4 — Dangerous | 2 | Complex AND contracts not adequately verified |
| ⚪ Q3 — Needs Tests | 3 | Simple but underspecified |

**GazeCRAPload**: 5 (functions ≥ threshold 15) — The 2 Q4 functions (`SyncCalendarAttachments` at GazeCRAP 1482 and `OrganizeDocuments` at 306) have decent line coverage but 0% contract coverage, meaning their side effects are tested incidentally rather than intentionally. The 3 Q3 functions need contract-level assertions added to existing tests, not new test files.

### What This Tells Us

The quadrant distribution reveals something the raw CRAP scores cannot: the difference between functions that are tested and functions that are *verified*.

12 functions land in Q1 (Safe) — these are the functions where both complexity and contract obligations are under control. Zero functions are in Q2 (Complex But Tested), which means there are no complex functions where contracts are being intentionally verified. This is a gap.

The two Q4 (Dangerous) functions are the most important finding. `SyncCalendarAttachments` has a GazeCRAP score of 1482 — by far the highest in the project. It has line coverage (code runs during tests), but 0% contract coverage (no test actually asserts on its contractual side effects). The tests exercise the code path without checking whether the function does what it is supposed to do. This is exactly the scenario that traditional coverage metrics miss and Contract Coverage was designed to catch.

The 3 Q3 functions are the easiest wins: they are simple functions that just need contract-level assertions added to their existing tests. No new test files, no refactoring — just better assertions.

## 🧪 Quality Summary

The quality analysis measures contract coverage at the per-function level — which side effects are contractual, which tests assert on them, and where the gaps are.

> ⚠️ Module-level quality analysis returned 0 tests — run per-package analysis (`/gaze quality ./internal/...`) for detailed contract coverage results.

### What This Tells Us

This warning means the module-level quality scan did not find test files at the root module scope. This is expected for projects that organize tests at the package level (which is the standard Go convention). The quality data that feeds the GazeCRAP quadrants above came from a different analysis path.

The fix is straightforward: run `/gaze quality ./internal/store` or `/gaze quality ./internal/organizer` to get per-package contract coverage breakdowns. This is not a bug — it is the tool telling you to scope your query more narrowly for detailed results.

## 🏷️ Classification Summary

The classification analysis identifies the side effects of each function and classifies them as contractual, incidental, or ambiguous.

The `analyze --classify` command returned only 1 function (`SetVerbose`) with no side effects classified, which is insufficient for a meaningful distribution table.

> ⚠️ No documentation found — classification uses mechanical signals only.

The docscan returned project-level documentation (AGENTS.md, README.md) but no function-level API docs or spec files referencing specific function contracts. Document-enhanced classification cannot meaningfully adjust mechanical scores without function-level documentation signals.

### What This Tells Us

Two things are happening here. First, the project does not have function-level API documentation — no godoc comments explaining what functions are responsible for, no spec files mapping functions to requirements. Without these signals, Gaze's classification engine falls back to purely mechanical heuristics (exported vs unexported, return types, interface satisfaction), which produces less confident classifications.

Second, the single classified function (`SetVerbose`) is a trivial setter with no meaningful side effects, so it does not generate useful distribution data.

This is honest reporting. Gaze does not inflate classification confidence when the signals are not there. The actionable takeaway: adding godoc comments to key exported functions would give the classification engine richer input and produce more useful contractual/incidental distinctions. Start with the functions that appear in the CRAP and GazeCRAP tables — those are the ones where accurate classification matters most.

## 🏥 Overall Health Assessment

The health assessment combines all dimensions into a single scorecard with letter grades and a prioritized action plan.

### Summary Scorecard

| Dimension | Grade | Details |
|-----------|-------|---------|
| CRAPload | 🔴 D | 40/137 functions (29.2%) above threshold |
| GazeCRAPload | 🟢 A- | 5/17 analyzed functions above threshold |
| Avg Line Coverage | 🔴 D | 26.2% — 101 of 137 functions have 0% coverage |
| Contract Coverage | 🟡 C | 52.9% avg across 17 quality-analyzed functions |
| Complexity | 🟢 B+ | Average 4.9, but 40 functions exceed threshold |

### What This Tells Us

The scorecard paints a nuanced picture. Complexity gets a B+ — the project is not over-engineered, and most individual functions are reasonably simple. But line coverage at 26.2% (with 101 functions at 0%) earns a D, and CRAPload at 29.2% confirms that the combination of untested + complex code is widespread.

The interesting contrast is between the two CRAP dimensions: traditional CRAPload gets a D (40 functions above threshold across all 137), but GazeCRAPload gets an A- (only 5 of the 17 quality-analyzed functions above threshold). This suggests that the functions which *do* have tests are generally well-tested at the contract level. The problem is not test quality — it is test *quantity*. Large swaths of the codebase have no tests at all.

Contract Coverage at 52.9% (C grade) across the 17 analyzed functions means about half of the contractual side effects are being verified. For the functions that have tests, there is room to tighten the assertions, but it is not catastrophic.

### Top 5 Prioritized Recommendations

Gaze produces a prioritized action plan based on the analysis:

1. 🔴 **Decompose SyncCalendarAttachments** — complexity 38 with GazeCRAP 1482 makes this the single most dangerous function; split into smaller orchestration steps before adding contract assertions.
2. 🔴 **Add tests for CreateDecisionsTab** — complexity 25 with 0% coverage produces CRAP 650; this is the newly added decision extraction entry point and needs both line and contract coverage.
3. 🔴 **Increase coverage for the cmd/gcal-organizer package** — 39 functions at 0% coverage account for 95% of the CRAPload; prioritize `runBrowserScript` (complexity 18) and `loadDotEnv` (complexity 15).
4. 🟡 **Add contract assertions for OrganizeDocuments, ExtractDecisionsForDoc, and retry.Do** — these have adequate line coverage (57-100%) but 0% contract coverage, placing them in Q3/Q4 quadrants.
5. 🟢 **Run per-package quality analysis** — module-level returned 0 tests; per-package analysis will surface granular contract coverage gaps across internal packages.

### What This Tells Us

The recommendations follow a clear triage logic:

**Red items first.** `SyncCalendarAttachments` is not just undertested — at complexity 38, it is too complex to test meaningfully in its current form. The recommendation is to decompose it before writing tests, because testing a function that complex would require an unwieldy number of test cases to cover the branching paths. `CreateDecisionsTab` is the next priority because it is new code (on the active branch) with zero tests — the easiest time to add tests is before the code becomes entangled with the rest of the system.

**Yellow items are the contract gaps.** Three functions have tests that run the code but do not assert on the contractual outputs. These are the Q3/Q4 functions from the quadrant analysis. Fixing these does not require writing new test files — it means adding assertions to existing tests.

**Green is a process improvement.** Running per-package analysis will surface more granular data that the module-level report could not provide. This is the next diagnostic step, not a code change.

## Key Takeaways

This report tells a common story for side projects and medium-sized codebases:

1. **The code is not overly complex.** Average complexity of 4.9 is fine. The problem is concentrated in a few high-complexity functions, not spread uniformly.
2. **Coverage is bimodal.** Core business logic has decent coverage. CLI and infrastructure code has none. This creates a long tail of high-CRAP functions that inflate the averages.
3. **The tests that exist are decent but incomplete.** Contract coverage of 52.9% means the existing tests are doing useful work, but about half of the contractual obligations are unverified.
4. **The biggest risk is a single function.** `SyncCalendarAttachments` at GazeCRAP 1482 is an outlier that dominates the risk profile. Decomposing it would materially improve the project's overall health score.
5. **The easiest wins are the Q3 functions.** Adding contract assertions to existing tests requires no new test infrastructure — just better assertions in tests that already exist.

For setup instructions and more about how Gaze works, see the [Gaze project page](/docs/projects/gaze/). For the conceptual foundation behind Contract Coverage and GazeCRAP, see [Why Contract Coverage](/blog/why-contract-coverage/).
