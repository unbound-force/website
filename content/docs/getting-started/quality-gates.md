---
title: "Quality Gates"
description: "The complete quality pipeline from commit to merge — CI, Gaze, Divisor Council, and Constitution Check in layered sequence."
lead: "Four layers of quality feedback, ordered from cheapest to most expensive. Nothing merges without passing all of them."
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 83
toc: true
---

## The Pipeline

Quality enforcement in Unbound Force is not a single check — it is a layered pipeline where each layer catches different types of issues. The layers run in order from cheapest (fastest, deterministic) to most expensive (slower, semantic):

1. **CI Pipeline** — deterministic computational checks
2. **Gaze Static Analysis** — computational quality metrics
3. **Divisor Council** — inferential multi-agent review
4. **Constitution Check** — inferential principle alignment

This ordering is deliberate. There is no point running five AI review agents if the code does not compile. Computational checks run first because they are fast, cheap, and unambiguous. Inferential checks run second because they are powerful but expensive.

## Layer 1: CI Pipeline

The first layer is standard continuous integration: test suites, linters, vulnerability scanners, and type checks. These are deterministic — the same input always produces the same result.

For Go projects, the CI layer typically includes:

- `go test -race -count=1 ./...` — test suite with race detection
- `golangci-lint run` — static analysis and style enforcement
- `govulncheck ./...` — known vulnerability scanning
- OSV-Scanner and Trivy — dependency vulnerability scanning

CI is a hard gate. If any check fails, the pipeline stops. No amount of reasoning or justification overrides a failing test. This is the foundational layer that all other quality checks build on.

The CI commands are not hardcoded — they are derived from `.github/workflows/` files, which are the source of truth. Agents read the workflow files to determine exactly which checks to run locally before declaring a task complete (the CI Parity Gate).

## Layer 2: Gaze Static Analysis

[Gaze](/docs/team/gaze-tester/) provides quality metrics that go beyond pass/fail. Where CI tells you "tests passed," Gaze tells you whether those tests actually verify meaningful behavior.

Key metrics:

- **CRAP scores** (Change Risk Anti-Patterns) — combines complexity with coverage to identify high-risk code. A function with high complexity and low coverage gets a high CRAP score, flagging it as a priority for testing.
- **Side effect classification** — identifies what each function actually does (file I/O, network calls, database writes, state mutation). Functions with unobserved side effects are flagged as testing gaps.
- **Coverage analysis** — not just line coverage, but whether tests observe the side effects that matter.

Gaze is a conditional layer. When Gaze is installed and the project is a Go codebase, the review pipeline runs Gaze analysis and includes the results in the review context. When Gaze is not available, the pipeline skips this layer and proceeds with CI checks and agent reviews.

## Layer 3: Divisor Council

[The Divisor Council](/docs/team/the-divisor/) is the inferential feedback layer — multiple specialized AI review agents evaluating code from different quality dimensions. Each agent has a distinct focus:

- **Guard** — intent drift detection. Does the implementation match the specification?
- **Architect** — structural integrity. Does the code follow established patterns and maintain clean boundaries?
- **Adversary** — stress testing. What breaks under edge cases, malicious input, or unexpected state?
- **Tester** — test quality. Do the tests verify behavior, or do they just exercise code?
- **SRE** — operational readiness. Can this code be deployed, monitored, and maintained?

The agents run in parallel, each reviewing independently. They have exclusive ownership boundaries — the Adversary does not comment on test quality (that is the Tester's domain), and the Tester does not flag architectural issues (that is the Architect's domain). This prevents overlap and ensures each finding comes from the agent with the deepest domain expertise.

The key architectural decision: review agents cannot modify files. The Guard agent operates at temperature 0.1 with `write: false`, `edit: false`, `bash: false`. It can read and analyze, but it structurally cannot change a single character. Every finding must be reported as a review comment, routed back through the implementation agent, and processed through the normal quality pipeline. The doer and the judge are separated by tool access restrictions, not behavioral instructions.

## Layer 4: Constitution Check

The [constitution](/docs/getting-started/constitution/) defines the core principles that all work must align with. The Constitution Check runs at each planning phase (spec, plan, tasks) and verifies that the proposed work does not violate any active principle.

Constitution violations are automatically CRITICAL severity — non-negotiable and non-overridable. If a change conflicts with a constitutional principle, the agent must stop and report the violation rather than proceeding.

This layer is the most expensive (it requires semantic understanding of both the change and the principles) but also the most consequential. A piece of code can pass CI, Gaze, and the Divisor Council while still violating an organizational principle. The Constitution Check catches misalignment between what was built and what the organization intended.

## Gatekeeping Value Protection

One category of quality gate requires special protection: the gates themselves. Agents that can modify quality thresholds, coverage requirements, or severity classifications could game the system by weakening the standards rather than meeting them.

Eight categories of values are protected from agent modification:

1. Coverage thresholds and CRAP scores
2. Severity definitions and auto-fix policies
3. Convention pack rule classifications (MUST/SHOULD/MAY)
4. CI flags and linter configuration
5. Agent temperature and tool-access settings
6. Constitution MUST rules
7. Review iteration limits and worker concurrency
8. Workflow gate markers

When an implementation cannot meet a gate, the agent must stop, report which gate is blocking and why, and let the human decide whether to adjust the gate or rework the implementation. Modifying a gate value to make the implementation pass is treated as a CRITICAL violation.

## The Three-Iteration Cap

When the Divisor Council finds issues, the implementation agent fixes them and resubmits for review. This cycle repeats up to three times. If the code cannot pass review in three iterations, the problem escalates to human judgment.

The cap prevents two failure modes:

- **Infinite loops** — the implementation agent and review agent entering a cycle where each fix introduces a new issue
- **Diminishing returns** — after three rounds of feedback, remaining issues are likely design-level problems that benefit from human perspective rather than additional automated iteration

## How the Layers Compose

The four layers are designed to complement each other:

- **CI** catches what is deterministically wrong (compilation errors, test failures, known vulnerabilities)
- **Gaze** identifies what is quantifiably risky (high CRAP scores, unobserved side effects)
- **The Divisor Council** evaluates what requires judgment (design quality, security edge cases, operational concerns)
- **The Constitution Check** validates what requires organizational alignment (principle adherence, governance compliance)

Each layer answers a different question. CI: "Does it work?" Gaze: "How risky is it?" The Divisor Council: "Is it well-built?" The Constitution Check: "Is it what we intended?"

When Gaze or Dewey are not available, the pipeline degrades gracefully — the remaining layers still provide quality assurance. But the full pipeline, with all four layers operating, provides coverage that no single tool or agent can achieve alone.
