---
title: "Constitution"
description: "The 4 core principles that govern all Unbound Force heroes — Autonomous Collaboration, Composability First, Observable Quality, and Testability."
lead: "The foundational principles that govern every hero in the Unbound Force swarm."
date: 2026-03-29T00:00:00+00:00
draft: false
weight: 85
toc: true
---

## What the Constitution Is

The Unbound Force constitution is the highest-authority document in the organization. It defines four core principles that every hero must follow and every hero constitution must align with. All development practices, pull request reviews, specification pipelines, and quality gates operate within the boundaries set by these principles.

The constitution exists because an AI agent swarm that generates code rapidly needs structural constraints to prevent collapse. Without shared principles, heroes would make incompatible assumptions about communication, quality, and testability. The constitution makes those assumptions explicit, enforceable, and versioned.

Contributors working with the swarm need to understand these principles because:

- Every hero constitution must align with the org constitution (hero-specific rules may add to, but never contradict, these principles)
- Every specification and plan passes a [Constitution Check](#constitution-check) before implementation begins
- Every [OpenSpec](/docs/getting-started/common-workflows/#bug-fix-tactical) proposal includes a constitution alignment assessment

## Principle I: Autonomous Collaboration

Heroes collaborate through well-defined [artifacts](/docs/getting-started/artifacts/) -- files, reports, and schemas -- rather than runtime coupling or synchronous interaction.

### MUST Rules

- Every hero MUST be able to complete its primary function without requiring synchronous interaction with another hero. A hero MAY consume another hero's artifacts, but MUST NOT block waiting for a response.
- Hero outputs MUST be self-describing: each artifact MUST contain enough metadata (producer identity, version, timestamp, artifact type) for any consumer to interpret it without consulting the producing hero.
- Inter-hero communication MUST use the [artifact envelope format](/docs/getting-started/artifacts/#envelope-format) defined by the Hero Interface Contract. Heroes MUST NOT invent ad-hoc exchange formats.

### SHOULD Rules

- Heroes SHOULD publish artifacts to a well-known location within the project repository so other heroes can discover them without explicit coordination.

**Rationale**: Artifact-based communication makes collaboration asynchronous, auditable, and resilient to individual hero unavailability. When heroes communicate through files rather than runtime calls, every interaction is recorded, versioned, and inspectable.

## Principle II: Composability First

Every hero MUST be independently installable and usable without any other hero being present. Combining heroes MUST produce additive value without introducing mandatory dependencies.

### MUST Rules

- A hero MUST deliver its core value when deployed alone. No hero MAY require another hero as a hard prerequisite for installation or primary operation.
- Heroes MUST expose well-defined extension points (configuration, artifact consumption, convention packs) for integration rather than requiring modification of their internals. No hero MAY require patching or forking another hero to integrate.
- When two or more heroes are deployed together, their combination MUST produce value greater than the sum of their individual capabilities. This additive value MUST NOT come at the cost of standalone functionality.

### SHOULD Rules

- Heroes SHOULD auto-detect the presence of other heroes and activate enhanced functionality when peers are available, without requiring manual configuration.

**Rationale**: Adoption friction kills tools. Composability ensures each hero earns its place independently. A team can start with just Gaze for quality analysis, add Cobalt-Crush for implementation, and later bring in the full swarm -- each addition is useful on its own and more useful together.

## Principle III: Observable Quality

Every hero MUST produce machine-parseable output alongside any human-readable output. All quality claims MUST be backed by automated, reproducible evidence.

### MUST Rules

- Every hero that produces output MUST support at minimum a JSON format. Human-readable output is recommended but MUST NOT be the only format available.
- All artifacts MUST include provenance metadata: which hero produced the output, which version, when, and against what input (branch, commit, backlog item).
- Quality claims MUST be backed by automated regression tests or benchmarks that can be re-run by any contributor.
- Metrics MUST be comparable across runs. Output formats MUST be stable enough that tooling built on a hero's output does not break between minor versions.

### SHOULD Rules

- Heroes SHOULD produce artifacts that conform to registered schemas in the shared data model, enabling cross-hero analysis without bespoke parsing.

**Rationale**: Machine-parseable output enables [Mx F](/docs/team/mx-f/) to track trends, [Muti-Mind](/docs/team/muti-mind/) to make data-driven prioritization decisions, and [The Divisor](/docs/team/the-divisor/) to ground reviews in evidence rather than opinion.

## Principle IV: Testability

Every component built within the Unbound Force ecosystem MUST be testable in isolation without requiring external services, network access, or shared mutable state.

### MUST Rules

- Test contracts MUST verify observable side effects (return values, state mutations, I/O operations) rather than implementation details.
- Coverage strategy (unit vs. integration vs. e2e, with specific targets) MUST be defined in the implementation plan for all new code.
- Coverage ratchets MUST be enforced by automated tests; any coverage regression MUST be treated as a test failure and block the build.
- Missing coverage strategy in a spec or plan is a CRITICAL-severity finding and MUST be resolved before implementation begins.

**Rationale**: AI agents generate code rapidly. If that code is not structurally testable, the resulting system will quickly collapse under its own unverified complexity. The Testability principle ensures that velocity never comes at the cost of verifiability.

## Hero Constitution Alignment

Each hero repository maintains its own constitution that extends the organizational constitution:

- Hero constitutions MUST include a `parent_constitution` reference indicating which version of the org constitution they align with.
- Hero constitutions MAY add principles beyond the org principles, provided they do not contradict any org-level MUST rule.
- When the org constitution is amended, all hero constitutions MUST be reviewed for continued alignment. Contradictions MUST be resolved by amending the hero constitution.

This creates a hierarchy: the org constitution sets the floor, and hero constitutions can raise the bar but never lower it.

## Governance

### Supremacy

When a hero constitution and the org constitution conflict, the org constitution prevails. The hero constitution MUST be amended to resolve the conflict.

### Amendments

Any change to the constitution MUST be proposed via pull request, reviewed, and approved before merge. The amendment MUST include a migration plan if it alters or removes existing principles. All hero constitutions MUST be reviewed for continued alignment after any amendment.

### Versioning

The constitution follows semantic versioning:

| Change Type                                                   | Version Bump | Example                               |
| ------------------------------------------------------------- | ------------ | ------------------------------------- |
| Principle removal or incompatible redefinition of a MUST rule | MAJOR        | Removing Testability principle        |
| New principle added or materially expanded guidance           | MINOR        | Adding Testability (v1.0.0 -> v1.1.0) |
| Clarifications, wording, or non-semantic refinements          | PATCH        | Rewording a rationale paragraph       |

### Compliance Review

At each planning phase, the Constitution Check gate MUST verify alignment with all active org principles. Constitution violations are automatically CRITICAL severity and non-negotiable.

### Conflict Resolution

When two org principles appear to conflict, the tradeoff MUST be explicitly documented. No principle has implicit priority over another; resolution is context-dependent and requires written justification.

## Constitution Check

The Constitution Check is a mandatory gate at the planning phase. Before implementation begins, every plan must pass a check against all active principles. The check verifies:

- Each principle's MUST rules are satisfied or explicitly justified
- No principle is violated without documented rationale
- The proposed work aligns with the constitutional intent, not just the letter

Violations are CRITICAL severity and non-negotiable -- they must be resolved before implementation proceeds.

The `/constitution-check` command automates this assessment. OpenSpec proposals also include a Constitution Alignment section where each principle is evaluated against the proposed change.

The constitution is versioned (currently v1.1.0) and the check validates against the version referenced in the project's `parent_constitution` field. See the [contributing guide](/docs/contributing/) for how this fits into the specification pipeline.

## Next Steps

- Read [Hero Artifacts](/docs/getting-started/artifacts/) to understand the envelope format and artifact types referenced in Principle I
- See [Common Workflows](/docs/getting-started/common-workflows/) for how the constitution gates the specification pipeline
- Explore the [Team](/docs/team/) pages to see how each hero implements these principles
