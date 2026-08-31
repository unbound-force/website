---
title: "Vision"
description: "The Unbound Force vision — why the industry is shifting from co-pilot to factory, and how a spec-driven AI agent swarm makes that transition real."
lead: "Unbound Force is built for the world where engineers write specifications and agents produce implementations."
date: 2026-01-01T00:00:00+00:00
draft: false
weight: 5
toc: true
---

## The Shift

The industry is moving from co-pilot to factory.

In the co-pilot model, an engineer writes code while an AI assistant fills in the gaps — autocomplete, boilerplate generation, question answering. The human is in the loop for every decision. The AI accelerates individual tasks but does not change the shape of the work.

In the factory model, the relationship inverts. Engineers write specifications. Agents produce implementations. The embodiment — the generated code itself — is disposable. If it does not pass quality gates, the agent regenerates it. Engineers stop fixing code and start fixing the agents and specs that produce the code. The human moves from in-the-loop to on-the-loop: setting direction, reviewing outcomes, and intervening only when the system cannot resolve a problem on its own.

This is not a speculative future. It is the structural shift that every team building production-grade agent systems is converging toward. As Steven Huels articulated: *"Open source communities will increasingly differentiate not on code, but on quality of specs, strength of evals, strong governance and the interoperability of systems."*

Unbound Force is built for that world.

## What Unbound Force Is

Unbound Force is a spec-driven AI agent swarm for software engineering. It is a composable toolkit — CLI-native, local-first, Apache-2.0 licensed — that gives AI coding agents the structure they lack on their own.

The swarm is organized around hero personas, each with a distinct role:

- **Muti-Mind** — the Product Owner. Generates stories from requirements, maintains the backlog, gates acceptance criteria.
- **Cobalt-Crush** — the Developer. Implements approved specifications, follows convention packs, produces tested code.
- **Gaze** — the Quality Sentinel. Measures test quality through CRAP scores, contract coverage, and side-effect classification. Does not measure whether code "looks good" — measures whether tests verify the contractual behavior of the code they exercise.
- **The Divisor** — the Review Council. A panel of specialized reviewers (Guard, Architect, Adversary, SRE, Testing, Curator, Scribe, Herald, Envoy) that evaluate pull requests from orthogonal quality dimensions in parallel.
- **Mx F** — the Flow Facilitator. Tracks metrics, coaches teams, manages sprints.

These personas scaffold into any repository via `uf init`, deploying agents, commands, skills, and convention packs into the consuming project's `.opencode/` directory. Each tool — `uf`, `gaze`, `dewey`, `replicator`, `vibe-check` — is independently installable and useful on its own. Combining them produces additive value without mandatory dependencies.

## The Problem

AI coding agents are powerful but not self-directing.

Give an agent a feature description and it will produce code. Sometimes that code is excellent. Sometimes it silently drifts from the specification. Sometimes it passes tests that do not actually verify the behavior the tests claim to cover. Sometimes it introduces architectural debt that compounds across the codebase.

At scale, three things erode without structure:

**Quality.** Without measurement, "the agent wrote code" is not evidence that the code is correct, well-tested, or maintainable. Quality claims require automated, reproducible evidence — not spot-checking.

**Governance.** Without process boundaries, the agent plans and executes in the same pass, skipping the review and decomposition steps that catch design errors early. Every team that builds production agent systems independently discovers that planning and execution must be separated with hard gates between them.

**Trust.** Without trust, organizations cannot move from human-in-the-loop to human-on-the-loop. Trust is not a feeling — it is an accumulation of evidence that the system produces reliable outcomes under known constraints. That evidence must be measurable, comparable across runs, and auditable after the fact.

Unbound Force exists to solve all three.

## The Enduring Core

The project is governed by a constitution with five non-negotiable principles. These are not model-capability compensators — they are organizational values that persist regardless of how capable the underlying models become.

**I. Autonomous Collaboration.** Agents communicate through well-defined artifacts — files, reports, schemas — rather than runtime coupling. Every agent completes its primary function without requiring synchronous interaction with another agent. Outputs are self-describing with provenance metadata. This makes collaboration asynchronous, auditable, and resilient to individual agent unavailability.

**II. Composability First.** Every agent is independently installable and usable without any other agent being present. Combining agents produces additive value without introducing mandatory dependencies. Adoption friction kills tools; composability ensures each agent earns its place independently.

**III. Observable Quality.** Every agent produces machine-parseable output with provenance metadata. Quality claims are backed by automated, reproducible evidence. Metrics are comparable across runs. A swarm that cannot measure its own performance cannot improve.

**IV. Testability.** Every component is testable in isolation without requiring external services or shared mutable state. Tests verify observable side effects — return values, state mutations, I/O operations — rather than implementation details. Coverage ratchets are enforced automatically; any regression blocks the build.

**V. Security by Default.** Security is a structural property, not a review-time afterthought. Dependencies are verified by content hash. External inputs are validated before reaching security-sensitive operations. Components operate with minimum permissions. Every dependency is attack surface; the default answer is "do not add."

These principles are decay-resistant because they answer the question "what does this organization care about" rather than "what can this model not do." A model that writes perfect code still needs to know what the organization values.

## Build to Delete

Every component in an AI agent harness exists because someone believed the model could not do something reliably enough on its own. A multi-agent review council exists because one agent cannot reliably self-review. A specification pipeline exists because agents cannot reliably plan and execute in the same pass. Convention packs exist because agents do not consistently follow coding standards without explicit rules.

These are bets on model limitations. Some are good bets that will remain true for years. Some are already becoming unnecessary.

The honest position is this: we do not know which components of Unbound Force will prove to be permanent architecture and which will turn out to be temporary scaffolding. What we do know is the methodology for finding out.

After each model upgrade, run the same task with and without each harness component. Measure quality. If quality holds without the component, delete it — not deprecate, not make optional, delete. Dead harness weight consumes context window, adds latency, and gives a false sense of security.

**What persists:** the constitution and governance model, CI pipeline checks, convention packs, branch protection and commit gates. These encode organizational intent that does not expire with model improvements.

**What may decay:** the size of the Divisor Council, detailed step-by-step instructions in agent personas, iteration caps, portions of Dewey's role as models develop better native codebase understanding.

The system should shrink over time. That is a sign of progress, not regression. The discipline is to keep testing.

## What Makes Us Different

**Contract coverage.** Gaze does not measure line coverage or branch coverage alone. It measures whether tests verify the *contractual behavior* of the code they exercise — return values, state mutations, I/O side effects — versus testing implementation details that change when the code is refactored. No other system in the AI agent space measures this. Gaze classifies 37+ side-effect types and computes GazeCRAP scores that combine complexity, coverage, and contract verification into a single actionable metric. Multi-language support comes through language-specific backends: snake-eyes for Python, reading-stone for TypeScript/JavaScript, with Go built in.

**Design and architectural quality.** Vibe-check measures structural health at the package level — coupling, cohesion, circular dependencies, instability, abstractness, distance from the main sequence — using Robert C. Martin's package metrics. It uses a universal model with a cross-language adapter architecture (JSON-RPC 2.0 protocol), so the same metrics are comparable whether the codebase is Go, Python, or TypeScript. Gaze tells you whether your tests are good. Vibe-check tells you whether your design is sound. Together they provide quality measurement at both the function and architectural levels.

**Composability by design.** Each tool — `uf`, `gaze`, `dewey`, `replicator`, `vibe-check` — delivers standalone value. `gaze` works without `uf`. `dewey` works without `gaze`. `uf init --divisor` deploys only the PR review agents. There is no monolithic platform to adopt; you install the pieces you need and add more when you are ready. When tools are deployed together, they auto-detect each other and activate enhanced capabilities without requiring manual configuration.

**Spec-driven development as a structural constraint.** The separation between planning and execution is not a suggestion — it is enforced. Branch naming gates pipeline entry. All spec artifacts must be committed before implementation begins. The `/uf.unleash` command has defined exit points where human judgment is required. `uf gate` enforces spec-first deterministically in headless/CI environments — the model cannot bypass it because it is not a prompt instruction but a pre-commit hook.

**Factory pattern alignment by design.** All five characteristics of the factory pattern — agents producing specs, automation implementing and validating, disposable embodiment, engineers fixing agents rather than code, human-on-the-loop — were structural properties of Unbound Force before the factory vision was formally published. This is convergent design, not retrofitting.

## The Boundary Thesis

Unbound Force is a fluid agent layer — the personas, workflows, quality measurement, and governance rules that shape *how* agents do work. It is not an infrastructure layer and does not aspire to be one.

Infrastructure — sandbox provisioning, credential management, event dispatch, cost tracking, provenance recording, security isolation — belongs to the steady-state layer. When Unbound Force runs inside FullSend's Bring Your Own Agent (BYOA) containers, FullSend provides that infrastructure. When it runs locally, OpenCode provides the runtime. The agent layer is the same in both environments: the same `.opencode/` directory, the same constitution, the same convention packs, the same slash commands.

This boundary is deliberate. By not owning infrastructure, Unbound Force inherits capabilities it would otherwise have to build and maintain — cost/FinOps telemetry, provenance and attribution, sandbox security, event-driven dispatch — while focusing its energy on the agent layer where its differentiation lives.

The broader ecosystem forms three layers:

1. **Domain Intake** — domain-specific requirements enter the system (PRDs, ADRs, product-specific toolkits). This is where organizational context lives.
2. **Spec-Driven Development Bridge** — OpenSpec and Speckit decompose requirements into stories (GitHub Issues) and capability-level specs that drive implementation. This is the translation layer between human intent and agent-executable work.
3. **Execution Engine** — `/uf.unleash` through `/uf.finale`: branching, clarification, planning, task decomposition, spec review, parallel implementation, code review, and commit/PR/CI workflows. This is where agents produce and validate code.

Each layer is independently valuable. Domain intake tooling works without Unbound Force. Unbound Force works without a formal intake layer. But when all three layers connect, the system achieves end-to-end traceability from business requirement to tested, reviewed, merged code — with every decision auditable in git.

## What Success Looks Like

The path from where most organizations are today to full factory-pattern operation has three phases. Each phase builds trust through accumulated evidence.

**Phase 1 — Human-Fronted Factories.** Agents produce artifacts — specs, implementations, reviews, test results — but humans make every merge decision. The agent swarm accelerates work; the human validates outcomes. Unbound Force is ready for this phase today. The `/uf.unleash` command runs the full pipeline autonomously but never auto-merges. The `/uf.finale` command pushes a PR, watches CI, and waits for human approval.

**Phase 2 — Agentic Review Participation.** Agents post formal reviews on pull requests, and those reviews carry weight in the merge decision. The Divisor Council already operates in this mode via the `council-review-action` GitHub Action — agents review PRs in CI and post structured findings. Humans still merge, but agent reviews are a first-class input to the decision, not an afterthought.

**Phase 3 — Factory-to-Factory.** Multiple factory instances operate across repositories at machine speed. One factory identifies an integration issue in its dependency; another factory receives the signal and produces a fix; a third factory validates the fix against its own test suite. Humans set policy and handle escalations. The loop runs continuously. This phase requires cross-repo forge capabilities, mature eval harnesses, and tiered approval policies that do not yet exist. It is the destination, not the current state.

The honest assessment: Phase 1 is operational. Phase 2 is operational for review participation. Phase 3 is not yet built. The roadmap is the path from here to there.
