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

The full hero lifecycle for a new feature follows six stages. Each stage is owned by a specific hero, and each produces artifacts consumed by the next.

### 1. Define (Product Owner)

The [Product Owner (Muti-Mind)](/docs/getting-started/product-owner/) creates a backlog item and drives specification creation.

- Create or prioritize a backlog item using Muti-Mind's priority scoring (Business Value, Risk, Dependency Weight, Urgency, Effort)
- Initiate the specification: `/speckit.specify`
- Provide acceptance criteria in Given/When/Then format
- Clarify ambiguities: `/speckit.clarify`

**Output**: Backlog item + feature specification

### 2. Specify and Plan (Developer)

The [Developer (Cobalt-Crush)](/docs/getting-started/developer/) creates the technical plan and task breakdown.

- Generate the implementation plan: `/speckit.plan`
- Generate tasks: `/speckit.tasks`
- Run cross-artifact analysis: `/speckit.analyze`
- Validate checklists: `/speckit.checklist`
- Commit and push all spec artifacts before implementation

**Output**: plan.md + tasks.md + research.md

### 3. Implement (Developer)

The Developer implements the feature following the task plan.

- Execute implementation: `/speckit.implement` or `/cobalt-crush`
- For parallel work: `/swarm "implement spec NNN"`
- Mark each task `[x]` in tasks.md as it completes
- Run tests after each phase checkpoint

**Output**: Code + tests

### 4. Validate (Tester)

[Gaze](/docs/getting-started/tester/) runs quality analysis on the implemented code.

- Analyze side effects: `gaze analyze --classify ./...`
- Assess test quality: `gaze quality ./...`
- Compute risk scores: `gaze crap ./...`
- Generate comprehensive report: `gaze report ./... --ai=claude`

**Output**: Quality report (contract coverage, CRAP scores, over-specification)

### 5. Review (Reviewer Council)

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

### 6. Accept (Product Owner)

The Product Owner evaluates the completed increment against acceptance criteria.

- Reviews the Gaze quality report
- Compares results against the backlog item's acceptance criteria
- Makes a decision: accept, reject, or conditional
- If rejected: a new backlog item is created with the rejection rationale

**Output**: Acceptance decision

### 7. Measure (Manager)

[Mx F](/docs/getting-started/product-manager/) captures a metrics snapshot and identifies improvement opportunities.

- Collects velocity, quality trends, review efficiency, and CI health
- Updates the team dashboard
- Identifies patterns for the next retrospective

**Output**: Metrics snapshot

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

```bash
brew install unbound-force/tap/unbound
```

### 2. Run Setup

```bash
unbound setup
```

This installs everything in one command:

- Detects your version managers (goenv, nvm, pyenv, Homebrew)
- Installs OpenCode (via Homebrew or curl)
- Installs Gaze (via Homebrew)
- Installs the Swarm plugin (via npm)
- Configures `opencode.json` with the Swarm plugin
- Initializes `.hive/` for work tracking
- Runs `unbound init` to scaffold project files

Use `--dry-run` to preview what would be installed without making changes.

### 3. Verify

```bash
unbound doctor
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
