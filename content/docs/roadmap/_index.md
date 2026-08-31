---
title: "Roadmap"
description: "The Unbound Force roadmap — capability horizons from headless autonomy and the factory foundation through trust at scale to factory-to-factory operation."
lead: "Organized into capability horizons — Now, Next, and Later — shaped by model improvements, upstream dependencies, and community demand."
date: 2026-01-01T00:00:00+00:00
draft: false
weight: 6
toc: true
---

This roadmap is organized into capability horizons — Now, Next, and Later — rather than calendar dates. The work is shaped by model improvements, upstream dependencies, and community demand, all of which shift faster than quarterly plans can track. Where the phasing roughly maps to calendar time, it aligns with: Now ≈ Q4 2026, Next ≈ H1 2027, Later ≈ H2 2027 and beyond.

For the vision and principles that drive this roadmap, see [Vision](/docs/vision/).

---

## Where We Are Today

An honest snapshot of strengths and gaps, scored against the factory pattern described in the vision.

**Strong.** SDLC acceleration is the system's best area. The spec-driven pipeline (Speckit/OpenSpec), the 8-phase `/uf.unleash` workflow, the Divisor review council, Gaze test-quality measurement, convention packs, and the constitution governance model are all operational and battle-tested across multiple repositories and contributors.

**Operational.** Phase 1 (human-fronted factories) and Phase 2 (agentic review participation) of the trust ladder are working today. Agents produce specs, implement code, and post formal PR reviews. Humans make every merge decision. The `council-review-action` GitHub Action runs Divisor reviews in CI on real pull requests.

**Partial.** Setup and operations (distribution is solid via Homebrew/RPM/go install/containers, but first-run onboarding has friction). Governance and compliance (convention packs are strong, but design-level and architectural-level metrics are only now emerging via vibe-check). Cost and FinOps (no native cost tracking — this is infrastructure-layer work that FullSend provides). Interrupt-centric metrics (the OTEL substrate exists but is not fully instrumented).

**Gap.** The eval harness is the largest gap. Gaze measures code and test quality. Vibe-check measures design quality. Neither measures whether agent *output* satisfies *success criteria* — the end-to-end evaluation loop that the factory pattern demands. This is the single most important capability to build. Cross-repo factory-to-factory operation does not exist yet; forge is single-repo. Vertical/platform accelerators are absent and demand-driven.

---

## Horizon 1 — Now: Headless Autonomy and the Factory Foundation

The immediate work establishes Unbound Force as a headless, autonomous agent layer that runs inside infrastructure platforms without losing any of its local capabilities. This is the foundation that every later horizon depends on.

### FullSend BYOA Integration

The defining work of this horizon is running the full Unbound Force swarm inside FullSend's Bring Your Own Agent containers with complete local/FullSend parity — the same `.opencode/` directory, the same constitution, the same slash commands, the same convention packs. Not a degraded skill-based shim; full parity.

This follows a five-increment progression, each building trust before the next adds capability:

**Increment 1 — uf-review (read-only).** Prove that the Divisor review council runs headless inside a FullSend sandbox. OpenCode runs via `opencode run --format json`, emitting ndjson events that the FullSend runtime consumes. The agent reads code, posts structured review findings, and exits. No writes. This is the go/no-go gate — if OpenCode cannot run headless in the sandbox, the entire approach is blocked. ([#509](https://github.com/unbound-force/unbound-force/issues/509))

**Increment 2 — uf-address-feedback (async Q&A + resume).** Add the ability for agents to pause, ask questions via GitHub comments, and resume when a human answers. This replaces blocking interactive prompts with an async `status:needs_input` / `/uf.answer` pattern that works in unattended environments. ([#513](https://github.com/unbound-force/unbound-force/issues/513))

**Increment 3 — uf-specify + `uf gate` (first writes, with security).** The agent creates specification artifacts — proposals, designs, task breakdowns — and enforces spec-first via `uf gate`, a deterministic pre-commit hook that cannot be bypassed by the model because it is not a prompt instruction. This increment requires closing the narrow security-hook gap in OpenCode's before-hook (structured block channel, not throw-only). Security is a prerequisite, not a follow-up. ([#514](https://github.com/unbound-force/unbound-force/issues/514), [#515](https://github.com/unbound-force/unbound-force/issues/515))

**Increment 4 — uf-code (source writing).** Cobalt-Crush implements approved specifications inside the sandbox. The full write path: branch, implement against the spec, run tests, commit. This is where the agent goes from reading and reviewing to producing code — the transition from Phase 2 to the operational core of Phase 1 at scale.

**Increment 5 — uf-forge (multi-agent parallelism).** Multiple agents operate in a single sandbox via OpenCode's sub-agent delegation. Parallel worktree execution with cherry-pick merge. This is the foundation for machine-speed throughput within a single repository.

**Upstream contributions.** The approach is upstream-first: contribute `OpenCodeRuntime` to `fullsend-ai/fullsend` (no fork), publish a digest-pinned `ghcr.io/unbound-force/fullsend-opencode` container image, and onboard the unbound-force org's own repos into FullSend. The `pi` runtime — shipped as the second non-Claude runtime — proves this is a days-scale contribution effort and serves as the template. ([#510](https://github.com/unbound-force/unbound-force/issues/510), [#511](https://github.com/unbound-force/unbound-force/issues/511), [#519](https://github.com/unbound-force/unbound-force/issues/519))

### Spec-First Enforcement

`uf gate` is a new headless subcommand that enforces the spec-first constraint deterministically. It runs as a `validation_loop.script` in FullSend and as a pre-commit hook locally. The model cannot bypass it because it operates outside the model's control surface — it checks whether spec artifacts exist and are committed before allowing implementation files to be written. This is the mechanism that makes the factory pattern's "specs before code" property structural rather than aspirational. ([#514](https://github.com/unbound-force/unbound-force/issues/514))

### Vibe-Check GA

Vibe-check reaches general availability as an independent project measuring design and architectural quality. The universal metrics model is complete — Instability, Abstractness, Distance from main sequence, LCOM, zone classification — with a cross-language adapter architecture (JSON-RPC 2.0 over stdin/stdout). The remaining work: the Entropy Sentinel agent for Boy Scout enforcement (structural delta tracking base-vs-PR), the architectural design convention pack (AD-001 through AD-010), and the `/vibe-check` reporter command. Vibe-check deploys into consuming repos via `vibe-check init`, the same scaffolding pattern as `uf init`.

### Gaze Multi-Language Backends

Gaze's test-quality measurement — CRAP scores, contract coverage, side-effect classification — extends beyond Go to Python and TypeScript/JavaScript through dedicated backend projects:

- **Snake-eyes** (Python): JSON-RPC server implementing the Gaze analyzer protocol. Taxonomy of 48 side-effect types, side-effect detection, complexity scoring, coverage analysis, classification signal extraction, and test mapping pipeline.
- **Reading-stone** (TypeScript): The same protocol, adapted for TypeScript and JavaScript codebases.

Both backends communicate with the Gaze frontend via the same JSON-RPC interface, producing metrics that are directly comparable across languages — a requirement of Constitution Principle III (Observable Quality: metrics MUST be comparable across runs).

### Vibe-Check Multi-Language Backends

Vibe-check's design and architectural metrics — instability, abstractness, distance from the main sequence, cohesion, and circular-dependency detection — extend across languages through the same universal-model adapter architecture the completed metrics package defines (JSON-RPC 2.0 over stdin/stdout). The frontend stays language-agnostic; each backend implements the analyzer protocol for one language:

- **Go** (native): `vibe-check analyze` computes package-level coupling metrics directly. ([vibe-check#2](https://github.com/zero-dot-force/vibe-check/issues/2))
- **rattler** (Python): a coupling adapter bringing the same Martin-metrics analysis to Python codebases. ([vibe-check#9](https://github.com/zero-dot-force/vibe-check/issues/9))
- **TypeScript/JavaScript**: a coupling adapter over the same protocol, planned alongside the other cross-language work. ([vibe-check#14](https://github.com/zero-dot-force/vibe-check/issues/14))

As with Gaze, every backend produces metrics directly comparable across languages — the universal model guarantees an identical unit of analysis (the module) and identical metric definitions regardless of source language, satisfying Constitution Principle III. This is the design-quality counterpart to Gaze's test-quality backends: one measures whether tests verify contractual behavior, the other whether the architecture stays on the main sequence, and both speak the same adapter protocol so a polyglot repository gets consistent measurement from a single toolchain.

### Inherited Infrastructure Capabilities

By adopting FullSend as the infrastructure layer, several previously-gapped capabilities close without Unbound Force building them:

- **Cost and FinOps** — FullSend's `RunMetrics` tracks `TotalCostUSD`, token counts, and OTEL telemetry per run. Unbound Force inherits this by running inside the platform.
- **Provenance and attribution** — FullSend's `TranscriptHandler` records model identity, tool invocations, fetch audit trails, and trace IDs. Every agent action is attributable.
- **Sandbox security** — FullSend owns env sanitization, OpenShell/landlock containment, network isolation, and prompt-injection hardening. Unbound Force's agents operate inside these controls rather than reimplementing them.

This is the boundary thesis in practice: the agent layer focuses on what it differentiates on; the infrastructure layer provides everything else.

---

## Horizon 2 — Next: Trust and Governance at Scale

With the factory foundation operational, the next horizon builds the governance, measurement, and interoperability capabilities that allow organizations to extend trust — moving from "agents produce code that humans review" toward "agents produce evidence that justifies trust."

### Eval Harness

The largest gap in the current system. Gaze measures whether tests are good. Vibe-check measures whether design is sound. Neither measures whether agent *output* satisfies the *success criteria* that motivated the work.

The eval harness closes this loop. Given a specification and an agent's implementation, the harness evaluates whether the implementation fulfills the spec — not just whether it compiles and passes tests, but whether it achieves the stated objectives. This is the cross-cutting capability that the factory pattern demands most urgently. A concrete first instance: the review council validating that a PR addresses the originating issue's acceptance criteria — the output-side mirror of intake-kit's input-side validation. ([#563](https://github.com/unbound-force/unbound-force/issues/563))

Evaluation spans two complementary layers that share infrastructure — the same headless driver, rubric-based LLM judge, and comparable-results store:

- **Output eval** (the loop above) — does an implementation satisfy its spec's success criteria? The design draws on FullSend's `validation_loop` and the `agent-eval-harness` pattern (ADR 0051 upstream), with cross-model evaluation — the same task judged by different models — to reduce single-model bias.
- **Command and harness-quality eval** — do the commands, agents, and skills the project ships perform well, and have they regressed? The `eval-infra` proposal defines a `unbound-force/eval-infra` repository: a reusable GitHub Actions workflow any repo can call, a TypeScript SDK driver that runs OpenCode headlessly and can answer the interactive `AskUserQuestion` prompts that pervade multi-phase commands (the gap that off-the-shelf harnesses hit), a fixture format, and a persistent `runs.db` tracking token cost and output quality over time. Configuration profiles measure whether an efficiency change saves money without degrading output. ([Discussion #399](https://github.com/orgs/unbound-force/discussions/399))

### Security Hooks for Write-Capable Agents

OpenCode's permission system is documented as "a UX feature, not a security boundary." Headless mode grants all permissions by default. For read-only operations (Increment 1), this is acceptable — FullSend's sandbox provides compensating controls. For write-capable agents (Increments 3+), the narrow gap must close: OpenCode's `tool.execute.before` hook needs a structured block channel (not throw-only) so that policy decisions can be communicated back to the runtime rather than simply aborting the operation.

This is the prerequisite for safely running spec-writing and code-writing agents in unattended environments. The fix is narrow and well-understood; the upstream coordination is the harder part.

### Design-Quality Gates

Vibe-check's metrics become CI-enforced quality gates:

- **Entropy Sentinel** — a Divisor agent (`divisor-entropy`) that computes structural deltas between base and PR branches. If a PR increases coupling, reduces cohesion, or introduces circular dependencies, the sentinel reports it with quantified evidence. Not a subjective "this feels coupled" — a measured change in Ce, I, or D metrics.
- **Pre-change CRAP gate** — before modifying a function, Gaze checks whether its existing CRAP score exceeds the threshold. If it does, the function must be improved (tests added, complexity reduced) before new changes are applied. The Boy Scout rule, enforced by measurement.
- **Architectural design convention pack** (AD-001 through AD-010) — rules with concrete thresholds: cognitive complexity < 15, efferent coupling < 10, instability < 0.7, no circular dependencies, files < 400 lines. Consumed by both Cobalt-Crush (when writing) and Divisor (when reviewing), ensuring the same standards apply to production and review.
- **Package regression in CI** — track coupling and cohesion metrics across builds. A PR that causes package-level regression fails CI. This makes design quality a ratchet, not a snapshot.
- **Architectural drift tracking** — Dewey stores time-series coupling and cohesion data. A trend agent monitors for gradual drift — the kind that no single PR causes but that accumulates over months. This is mandated by Constitution Principle III: metrics must be comparable across runs, which implies they must be tracked over time.

### Curated Knowledge Stores

Dewey's current knowledge layer is manual (`store_learning`) and ephemeral (SQLite-only). Curated knowledge stores replace this with a persistent, file-backed system:

- **Config-driven extraction** — map sources (meetings, Slack, GitHub issues) to knowledge stores via configuration. LLMs extract decisions, facts, patterns, and references automatically.
- **Source tracing** — every extracted fact links back to its source document, block, and timestamp. Full provenance, auditable in git.
- **Multi-source aggregation** — knowledge compounds across sources. A decision made in a meeting, referenced in a Slack thread, and formalized in a GitHub issue converges into a single, attributed knowledge article.
- **Quality scoring** — confidence levels (high/medium/low/flagged) and quality flags (missing rationale, implied dependency, contradiction, stale reference, scope conflict). `dewey lint` surfaces knowledge quality problems.
- **Git-backed persistence** — knowledge stores are markdown files in the repository. They are versioned, diffable, reviewable, and mergeable. No external database required for the canonical store.

This extends Dewey from a session-scoped tool into an organizational memory that improves with every interaction. ([Discussion #114](https://github.com/orgs/unbound-force/discussions/114))

### Pluggable VCS and Ticketing

Unbound Force is currently GitHub-only — Issues, PRs, Actions, the `gh` CLI. Real-world adoption requires:

- **GitLab support** — Merge Requests instead of Pull Requests, GitLab CI instead of GitHub Actions, the Files API for knowledge store operations.
- **Jira integration** — stories-as-Jira-tickets alongside stories-as-GitHub-Issues, for organizations where project management lives in Jira.

The architecture already supports this in principle: convention packs, agents, and the constitution are VCS-agnostic markdown files. The coupling is in the CLI commands and workflow scripts that assume GitHub. Pluggable backends for VCS operations and ticketing operations make the agent layer portable across platforms.

### The Three-Layer Intake Ecosystem

The long-term architecture for requirements flowing into Unbound Force is a three-layer system:

1. **Domain Intake** — intake-kit provides CUE-validated PRD authoring with a 5-specialist review council (Guard, Adversary, Tester, Operator, Curator) that gates requirements on testability, completeness, and security before any code is written. Domain-specific convention packs (e.g., `spog.md`) extend the intake layer for particular products without modifying the core agent layer. This forms the input side of a symmetric two-council architecture: intake-kit validates requirements in; the Divisor review council validates implementations out.
2. **SDD Bridge** — OpenSpec and Speckit translate domain requirements into stories (GitHub Issues) and capability-level specs. The complytime RFC defines the long-term bridge: a PRD+ADR-to-task adapter folded into `/unleash`, run per story under ADR constraints, with spec/task artifacts as disposable scaffolding. The GitHub Issue — carrying FR/AC IDs from the PRD — is the durable anchor that connects the intake council's input validation to the review council's output validation.
3. **Execution Engine** — `/uf.unleash` through `/uf.finale`: the agent pipeline that produces and validates code from specs.

Each layer is independently valuable. The bridge makes them composable. The adapter pattern ensures domain intake tooling does not need to know about Unbound Force's internals — it produces structured requirements; the bridge translates them; the engine executes them.

### RFE-to-STRAT Front-End

The pipeline has a gap at the very beginning: the path from a raw request-for-enhancement to a strategic decision to pursue it. Today, this is informal — someone writes an issue, it gets discussed, work starts. The front-end formalizes the intake: structured RFE capture, strategic prioritization, and routing into the SDD bridge.

Intake-kit already provides the core authoring and review machinery (CUE-validated PRDs with a 5-specialist review council). The complytime RFC defines the workflow spine: PRD → ADR → AAC Review → Story (GitHub Issue) → OpenSpec. The remaining work is the strategic prioritization layer — the rules and tooling that help organizations decide *which* validated requirements to pursue and in what order — and tighter integration between intake-kit's output and the SDD bridge's input.

This is uf's domain and differentiator — the part of the pipeline that no infrastructure platform provides.

### Asynchronous Agent Coordination and Typed Guardrails

Today the swarm executes a feedforward, spec-driven plan (`tasks.md`) through phase-gated stages. This is effective for well-specified work, but for long-horizon tasks the rigidity becomes a liability: an agent that discovers mid-execution that its plan is invalid — a wrong dependency assumption, a violated ADR, a compliance conflict — is trapped until the next phase boundary. Because standard MCP tool calls are synchronous, an agent cannot work and listen for lateral updates at the same time. The costs are wasted compute, no passive awareness, and flat escalation — no way to distinguish a bug the swarm should fix itself from an architectural flaw it must not touch.

Asynchronous coordination decouples listening from the model's context window. Replicator gains a pub/sub message broker; an OpenCode background-listener custom tool connects to it, returns immediately so the agent keeps working, and injects lateral messages back into the session as system notifications via the OpenCode SDK. Coordination messages are strongly typed rather than free text — which is what turns real-time messaging into governance:

- **`IMPLEMENTATION_DEVIATION`** — the swarm self-heals: force an interrupt, discard the invalidated `tasks.md`, and trigger an `/opsx-repropose` loop, no human needed.
- **`GOVERNANCE_BLOCKER`** — the swarm halts: if the flaw is in the specification itself (an ADR with an impossible constraint), stop the `/uf.unleash` loop and escalate to a human architect. Agents may rewrite implementations but never overrule architecture.

This gives the execution engine mid-flight agility and autonomous recovery while keeping architectural authority with humans — attention governance encoded in the message type system. It is also the foundation the next horizon builds on: the same typed-interruption substrate, extended across repository boundaries, is what makes factory-to-factory coordination safe. ([Discussion #444](https://github.com/orgs/unbound-force/discussions/444))

---

## Horizon 3 — Later: Factory-to-Factory

The final horizon is where the factory pattern reaches its full expression: multiple autonomous factory instances operating across repositories at machine speed, with humans setting policy and handling escalations.

### Cross-Repo Forge

Today, `/uf.forge` orchestrates multi-agent parallelism within a single repository. Cross-repo forge extends this to multi-repository coordination: one factory identifies an integration issue in a dependency, dispatches a signal, and another factory produces and validates a fix.

This requires solving several hard problems: cross-repo artifact routing, dependency-aware dispatch (a change in library A triggers re-validation in services B and C), and merge coordination across repositories with different owners and policies. It builds directly on the asynchronous coordination and typed guardrails established in Horizon 2 — extending the same self-correction and escalation substrate across repository boundaries — but the cross-repo topology itself is new.

### Machine-Speed Loops

When factories operate across repositories, the loop from "issue identified" to "fix validated" can run faster than human review cadence. This does not mean removing humans from the loop — it means changing what humans review. Instead of reviewing individual PRs, humans review *policies* (convention packs, constitution amendments, approval thresholds) and *outcomes* (trend dashboards, eval harness results, architectural drift reports).

The infrastructure for this is partially in place: OTEL telemetry, structured review output, provenance recording. What is missing is the policy layer — the rules that determine which outcomes require human attention and which can be auto-approved based on accumulated trust evidence.

### Tiered Approvals and Embargo Controls

Not all changes carry the same risk. A documentation fix and a security-sensitive API change should not go through the same approval process. Tiered approvals classify changes by risk and route them accordingly:

- **Low risk** (documentation, test additions, low-complexity refactors) — auto-approvable if CI passes and eval harness confirms spec satisfaction.
- **Medium risk** (feature implementation, dependency updates) — agent review + one human approval.
- **High risk** (security changes, public API modifications, dependency additions) — full Divisor council review + multiple human approvals.
- **Embargo** — changes that affect unreleased features or IP-sensitive code require additional controls: restricted visibility, mandatory legal/compliance review, time-locked merge windows.

This is a shared frontier across the industry. No system has mature tiered approval for AI-generated code. The design will draw on whatever emerges as best practice, anchored by the constitution's Security by Default principle.

### Vertical and Platform Accelerators

Domain-specific convention packs and agent configurations for particular technology stacks, compliance frameworks, or industry verticals. These are demand-driven — built when specific communities need them, not speculatively. Examples might include:

- A compliance-focused pack for FedRAMP/FISMA requirements
- A platform-specific pack for Kubernetes operator development
- An industry-specific pack for financial services regulatory constraints

The convention pack architecture already supports this. The work is in writing, testing, and maintaining domain-specific rules — which requires domain expertise from the communities that need them.

---

## The Throughline: The Harness Shrinks

Across all three horizons, a single discipline runs as a cross-cutting concern: the harness should get lighter over time.

Every component is a bet on a model limitation. As models improve, some bets expire. The pruning methodology is simple: after each model upgrade, run the same task with and without each harness component. Measure quality. Delete what does not contribute. Crucially, this measurement is automated rather than manual — the command and harness-quality eval layer (`eval-infra`, above) produces comparable token-cost and quality data across runs, so each pruning experiment yields evidence instead of an impression.

Concrete experiments that should be run periodically:

- Run with fewer Divisor agents (3 instead of 5, or even 1 with computational-only validation)
- Simplify agent persona instructions from multi-page documents to single paragraphs
- Skip the Gaze feedback loop for a sprint and measure whether quality degrades
- Remove Dewey knowledge retrieval from agent initialization and compare output
- Reduce the 3-iteration review cap to 2 or 1
- Test whether explicit ownership boundaries between review agents are still necessary

If quality holds in any of these experiments, that component has outlived its usefulness and should be removed. The goal is not to preserve the current architecture — it is to preserve the outcomes the architecture produces, with the minimum scaffolding necessary.

The system at its best is invisible. The specification is clear, the implementation is correct, the tests verify contractual behavior, the design is structurally sound, and the harness that made it happen is as light as it can possibly be. That is the destination. The roadmap is how we get there.
