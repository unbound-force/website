---
title: "The 8-Phase Pipeline: Why Plan/Execute Separation Is Not Enough"
description: "Industry consensus says plan and execute must be separate. Two phases are a start. Here is why complex features need eight, with hard gates between them."
lead: "Everyone agrees on plan/execute separation. Nobody agrees on how many phases you actually need. We landed on eight."
slug: "8-phase-pipeline"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 42
toc: true
categories: ["Engineering"]
tags: ["pipeline", "speckit", "agents", "architecture", "unleash"]
contributors: ["Unbound Force"]
---

## The Two-Phase Illusion

Every team building AI agent workflows discovers the same thing: letting an agent plan and execute in the same pass produces unreliable output. The planning step must be separate, with its output reviewed before implementation begins. OpenAI, Anthropic, and ThoughtWorks all arrived at this conclusion independently (Yanli Liu, "Harness Engineering," *AI Advances*, Apr 2026).

So you separate plan from execute. Two phases. Problem solved.

Except it is not. A two-phase workflow hides a cascade of decisions that each deserve their own checkpoint. "Plan" actually contains: clarifying ambiguous requirements, choosing an implementation strategy, breaking the strategy into tasks, validating that the tasks cover the specification, and verifying that nothing contradicts the project's governance principles. Collapsing all of that into "phase 1" means the agent makes dozens of uncheckpointed decisions before anyone reviews its work.

When a problem surfaces during implementation — an incorrect assumption, a missed constraint, a spec ambiguity — the two-phase model offers one option: go back to the beginning. Everything planned in phase 1 is suspect because the error could have been introduced at any sub-step.

## Eight Phases, Eight Checkpoints

The Speckit pipeline decomposes plan/execute into eight distinct phases:

```
constitution → specify → clarify → plan → tasks → analyze → checklist → implement
```

Each phase produces a specific artifact. Each artifact can be reviewed before the next phase begins. If an issue is found at any point, you go back to the nearest relevant phase, not to the beginning.

### Phase 1: Constitution

Every project starts with a constitution — the core principles that all work must align with. The [constitution](/docs/getting-started/constitution/) defines what the organization values (Autonomous Collaboration, Composability First, Observable Quality, Testability) and establishes the governance model.

**Why a gate here**: Without explicit principles, every subsequent phase operates on implicit assumptions. Two agents can produce technically correct plans that are architecturally incompatible because they assumed different priorities. The constitution makes priorities explicit.

### Phase 2: Specify

Turn a feature description into a structured specification with requirements, acceptance criteria, and scope boundaries.

**Why a gate here**: The specification defines what "done" means. If the specification is wrong, the plan will be wrong, the tasks will be wrong, and the implementation will be wrong — efficiently and at scale. A human review of the specification catches fundamental misunderstandings before any planning begins.

### Phase 3: Clarify

Scan the specification for ambiguities. Use semantic search (Dewey) to auto-resolve where possible. Exit to human judgment for unanswerable questions.

**Why a gate here**: Specifications contain latent ambiguity — phrases that seem clear but have multiple valid interpretations. "Support authentication" could mean OAuth, API keys, SAML, or all three. Clarification before planning prevents the planner from guessing and the implementer from building the wrong thing.

### Phase 4: Plan

Generate the technical implementation plan: which files change, what architectural patterns to follow, what dependencies to consider.

**Why a gate here**: The plan translates requirements into technical decisions. A plan that misidentifies the right abstraction or chooses an incompatible pattern produces tasks that are individually correct but collectively wrong. Human review of the plan catches architectural mistakes.

### Phase 5: Tasks

Break the plan into actionable, dependency-ordered implementation tasks with checkboxes.

**Why a gate here**: Tasks are the contract between the planner and the implementer. If tasks are too coarse, the implementer makes uncheckpointed decisions. If too fine, context is lost between tasks. If dependencies are wrong, tasks execute in the wrong order. The task list is the last artifact reviewed before code is written.

### Phase 6: Analyze

Cross-artifact consistency analysis. Does the plan match the specification? Do the tasks cover the plan? Are there contradictions between artifacts?

**Why a gate here**: Each artifact was created in sequence, potentially by different agents or in different sessions. Inconsistencies accumulate — a requirement in the spec that the plan forgot, a constraint in the plan that the tasks omit. Analysis catches drift between artifacts before implementation amplifies it.

### Phase 7: Checklist

Requirement quality validation. Are all requirements testable? Are acceptance criteria specific enough? Are scope boundaries clear?

**Why a gate here**: This is the last opportunity to catch specification-level problems. A requirement that says "should be fast" cannot be verified. A checklist that asks "does each requirement have a measurable acceptance criterion" catches this before anyone writes code.

### Phase 8: Implement

Execute the plan task by task, with CI checkpoints between task phases.

**Why a gate here**: Implementation is where code review, test execution, and the [Divisor Council](/docs/team/the-divisor/) review happen. The gate at the end of implementation (code review must pass within three iterations, CI must be green) ensures that the output meets quality standards before merge.

## How the Gates Are Enforced

Phase boundaries are enforced through multiple mechanisms, not just instructions:

**Branch naming conventions** gate pipeline entry. A branch named `003-feature-name` signals that the project has a specification with that number. The branch name is checked at every pipeline step.

**The Spec Commit Gate** requires all spec artifacts (spec.md, plan.md, tasks.md) to be committed and pushed before implementation begins. This preserves the planning record in version control and provides a clean baseline if implementation drifts from the plan.

**Filesystem markers** like `<!-- spec-review: passed -->` record that a gate was passed. These markers are checked programmatically — an agent cannot skip spec review by claiming it passed.

**The CI Parity Gate** requires agents to replicate CI checks locally before marking any task complete. The agent reads `.github/workflows/` to determine exactly which commands to run, then executes them. A task is not complete until all CI-equivalent checks pass.

**Workflow Phase Boundaries** are enforced rules in AGENTS.md. If an agent attempts to make code changes during a planning phase, it triggers a process violation. The agent must stop and report the violation rather than proceeding with out-of-phase changes.

## Why This Matters

The difference between two phases and eight phases is the difference between "review the plan" and "review each decision in the plan separately." With two phases, a spec ambiguity discovered during implementation requires replanning everything. With eight phases, it requires returning to the clarify phase — the plan, tasks, and analysis may or may not need updating, but the decision surface is narrower.

This granularity is expensive. Eight phases mean more artifacts, more reviews, more checkpoints. For a one-function bug fix, it is overkill — which is why Unbound Force offers [OpenSpec](/docs/getting-started/common-workflows/#bug-fix-tactical) as a lighter alternative for tactical changes.

But for complex features — the kind that involve multiple files, architectural decisions, and coordination across components — the eight-phase pipeline prevents a specific class of failures: cascading errors from uncheckpointed decisions. Every decision point becomes a review point. Every review point becomes an opportunity to catch mistakes before they compound.

The [/unleash](/docs/getting-started/common-workflows/#autonomous-pipeline-unleash) command orchestrates all eight phases with six defined exit points where human judgment is required. See the [operational walkthrough](/blog/unleash-in-practice/) for how the pipeline works in practice.
