---
title: "Common Workflows"
description: "Step-by-step reference for common Unbound Force workflows: new features, bug fixes, code reviews, and environment setup."
lead: "End-to-end workflows that show how all five heroes collaborate across the development lifecycle."
date: 2026-03-22T00:00:00+00:00
draft: false
weight: 70
toc: true
---

## New Feature (End-to-End)

The full hero lifecycle for a new feature follows six stages. Each stage is owned by a specific hero, and each produces artifacts consumed by the next. Every stage has an **execution mode** -- either `[human]` (driven by the operator) or `[swarm]` (run autonomously by the agent swarm).

### 1. Define (Product Owner) `[human]`

The [Product Owner (Muti-Mind)](/docs/getting-started/product-owner/) creates a backlog item and drives specification creation.

- Create or prioritize a backlog item using Muti-Mind's priority scoring (Business Value, Risk, Dependency Weight, Urgency, Effort)
- Initiate the specification: `/speckit.specify`
- Provide acceptance criteria in Given/When/Then format
- Clarify ambiguities: `/speckit.clarify`

**Output**: Backlog item + feature specification

After the human completes this stage, the swarm takes over automatically.

### 2. Implement (Developer) `[swarm]`

The [Developer (Cobalt-Crush)](/docs/getting-started/developer/) creates the technical plan and implements the feature.

- Generate the implementation plan: `/speckit.plan`
- Generate tasks: `/speckit.tasks`
- Run cross-artifact analysis: `/speckit.analyze`
- Validate checklists: `/speckit.checklist`
- Execute implementation: `/speckit.implement` or `/cobalt-crush`
- For parallel work: `/swarm "implement spec NNN"`
- Mark each task `[x]` in tasks.md as it completes
- Run tests after each phase checkpoint

**Output**: plan.md + tasks.md + code + tests

### 3. Validate (Tester) `[swarm]`

[Gaze](/docs/getting-started/tester/) runs quality analysis on the implemented code.

- Analyze side effects: `gaze analyze --classify ./...`
- Assess test quality: `gaze quality ./...`
- Compute risk scores: `gaze crap ./...`
- Generate comprehensive report: `gaze report ./... --ai=claude`

**Output**: Quality report (contract coverage, CRAP scores, over-specification)

### 4. Review (Reviewer Council) `[swarm]`

[The Divisor](/docs/team/the-divisor/) reviews the code through five specialized personas.

- Invoke the review council: `/review-council`
- Five personas evaluate in parallel:
  - **Guard**: Intent drift, constitution alignment, zero-waste
  - **Architect**: Coding conventions, pattern adherence, DRY
  - **Adversary**: Security, resilience, error handling
  - **Testing**: Test architecture, coverage strategy, assertion depth
  - **SRE**: Release pipeline, dependency health, observability

If the council returns REQUEST CHANGES, the developer addresses findings and re-submits (up to 3 iterations before escalation to human review).

**Output**: Review verdict (APPROVE or REQUEST CHANGES)

After the swarm completes review, the workflow pauses and returns control to the human.

### 5. Accept (Product Owner) `[human]`

The Product Owner evaluates the completed increment against acceptance criteria.

- Reviews the Gaze quality report
- Reviews the Divisor review verdict
- Compares results against the backlog item's acceptance criteria
- Makes a decision: accept, reject, or conditional
- If rejected: a new backlog item is created with the rejection rationale

**Output**: Acceptance decision

### 6. Reflect (Manager) `[swarm]`

[Mx F](/docs/getting-started/product-manager/) runs a retrospective analysis with empirical data from all heroes.

- Collects a metrics snapshot: velocity, quality trends, review efficiency, and CI health
- Consumes Gaze's quality report and the Divisor's review verdict
- Runs cross-hero learning analysis to detect recurring patterns across workflows
- Produces learning feedback with actionable recommendations (e.g., convention pack updates for repeated review findings)
- Updates the team dashboard and identifies improvements for the next retrospective

**Output**: Metrics snapshot + learning feedback + retrospective summary

### Swarm Delegation

The workflow is designed for **autonomous swarm delegation**. After the human completes the define stage (specify + clarify), the swarm runs implementation through review without human intervention. The workflow pauses automatically before the accept stage, returning control to the human for an acceptance decision. After acceptance, the swarm runs the final reflect stage autonomously.

This means a complete feature workflow requires only **two human decision points**:

1. **After define**: Hand off to the swarm
2. **At accept**: Review the increment and accept or reject

### Knowledge Context

Throughout the hero lifecycle, [Dewey](/docs/getting-started/knowledge/) provides semantic context to every hero during their stage. When Muti-Mind writes a specification, Dewey surfaces related issues and past acceptance criteria from across the organization. When Cobalt-Crush implements a feature, Dewey provides toolstack API documentation and implementation patterns from other repositories. When Gaze validates code quality, Dewey offers cross-repo quality baselines and known failure modes.

This context is automatic — heroes query Dewey's MCP tools as part of their normal workflow. No additional steps are required from the operator.

Dewey operates on a [3-tier graceful degradation](/docs/getting-started/knowledge/#graceful-degradation) model: full semantic search when Dewey and Ollama are available, structured graph queries when only the knowledge graph is indexed, and direct file reads when Dewey is not configured. Every hero functions at all three tiers — Dewey enriches the workflow but never blocks it.

## Bug Fix (Tactical)

For bug fixes and small changes (fewer than 3 user stories), use the OpenSpec tactical workflow instead of the full Speckit pipeline.

### 1. Propose

Create a change with proposal, design, and task artifacts in one step:

```
/opsx-propose fix-auth-timeout
```

This creates `openspec/changes/fix-auth-timeout/` with:

- `proposal.md` -- What and why
- `design.md` -- Technical approach
- `tasks.md` -- Implementation steps

### 2. Apply

Implement the tasks:

```
/opsx-apply
```

Works through each task in the tasks.md file, making code changes and marking tasks complete.

### 3. Review

Run the review council to validate the fix:

```
/review-council
```

The five Divisor personas review the changes. Address any REQUEST CHANGES findings.

### 4. Archive

After the fix is merged, archive the change:

```
/opsx-archive
```

Moves the change to `openspec/changes/archive/` with a date prefix for historical reference.

## Code Review

The review council brings five specialized perspectives to every code review.

### Invoking the Council

```
/review-council
```

The council discovers available Divisor persona agents in `.opencode/agents/divisor-*.md` and launches all of them in parallel.

### What Each Persona Evaluates

| Persona       | Focus Area                                                                               |
| ------------- | ---------------------------------------------------------------------------------------- |
| **Guard**     | Intent drift from the spec, constitution alignment, zero-waste mandate, scope discipline |
| **Architect** | Coding conventions per convention packs, architectural patterns, DRY, documentation      |
| **Adversary** | Security vulnerabilities, error handling, resilience, dependency risks                   |
| **Testing**   | Test architecture, coverage strategy, assertion depth, test isolation                    |
| **SRE**       | Release pipeline, dependency health, configuration, runtime observability                |

### The Review Loop

1. All personas review in parallel and return APPROVE or REQUEST CHANGES
2. If any persona returns REQUEST CHANGES, the developer addresses the findings
3. All personas re-review after fixes
4. This loop repeats up to 3 iterations
5. After 3 iterations without full approval, the issue escalates to human review

### Verdict

The council returns **APPROVE** only when all active personas approve. A single REQUEST CHANGES means the council verdict is REQUEST CHANGES. Missing personas (agent files not found) don't block the verdict but are noted in the report.

## Environment Setup

The one-time setup for a new developer or a new project.

### 1. Install the CLI

Install `uf` (short for `unbound-force`):

```bash
brew install unbound-force/tap/unbound-force
```

### 2. Run Setup

```bash
uf setup
```

This installs everything in one command:

- Detects your version managers (goenv, nvm, pyenv, Homebrew)
- Installs OpenCode (via Homebrew or curl)
- Installs Gaze (via Homebrew)
- Installs the Swarm plugin (via npm)
- Configures `opencode.json` with the Swarm plugin
- Initializes `.hive/` for work tracking
- Runs `uf init` to scaffold project files

Use `--dry-run` to preview what would be installed without making changes.

### 3. Verify

```bash
uf doctor
```

Doctor checks 7 areas and shows pass/warn/fail for each with install hints. Fix any failures by copying the suggested command from the output.

### 4. Start Working

Open OpenCode and try your first task:

```
/swarm "your first task description"
```

Or start with the Speckit pipeline for a new feature:

```
/speckit.specify
```

## Next Steps

- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow, Speckit, Swarm, and Cobalt-Crush
- [Tester Guide](/docs/getting-started/tester/) -- Gaze quality analysis and CI integration
- [Product Owner Guide](/docs/getting-started/product-owner/) -- Muti-Mind and backlog management
- [Product Manager Guide](/docs/getting-started/product-manager/) -- Mx F metrics and coaching
