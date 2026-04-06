---
title: "From Spec to Demo in One Command: How /unleash Works"
description: "Most AI coding workflows need a human pressing 'next' at every step. /unleash runs the entire pipeline — from spec to demo-ready code — autonomously."
lead: "One command. Eight stages. The swarm handles clarification, planning, implementation, testing, and review autonomously."
slug: "unleash-in-practice"
date: 2026-03-31T00:00:00+00:00
draft: false
weight: 80
toc: true
categories: ["Engineering"]
tags: ["unleash", "swarm", "autonomous", "pipeline", "speckit"]
contributors: ["Unbound Force"]
---

## The Manual Pipeline Problem

AI coding agents are powerful, but they are not self-directing. A typical agent-assisted development workflow looks like this:

1. Write a specification
2. Clarify ambiguous requirements (manually search for context)
3. Generate an implementation plan
4. Generate a task breakdown
5. Review the spec for consistency
6. Implement each task
7. Run code review
8. Fix review findings, re-review
9. Reflect on what was learned
10. Commit, push, create a PR, merge

Each step requires a human to invoke the next command, evaluate the output, and decide whether to proceed. The agent does the heavy lifting at each stage, but the human drives the sequencing. For a feature with 20 tasks, that means dozens of manual interventions across hours of wall-clock time.

The question is not whether each step is valuable — it is. The question is whether a human needs to be the one pressing "next" at every transition.

## What /unleash Does

`/unleash` is a single command that takes a Speckit specification and runs the entire pipeline autonomously — from clarification through implementation, testing, review, and demo. It handles the transitions, evaluates the outputs, and makes the proceed/stop decisions at each stage.

When it encounters something that genuinely requires human judgment — an unanswerable question, a critical spec finding, a persistent code review issue — it exits cleanly with context about what happened and what to do next. When you re-run `/unleash`, it detects which stages are complete and resumes from where it left off.

The result: you write a spec, run `/unleash`, and come back to demo-ready code with a summary of what was built and how to verify it.

## The Pipeline

`/unleash` orchestrates these stages in sequence:

### 1. Clarify

The pipeline scans your spec for `[NEEDS CLARIFICATION]` markers — questions that were flagged during specification but not answered.

If [Dewey](/docs/getting-started/knowledge/) (the swarm's knowledge retrieval system) is available, `/unleash` attempts to resolve each question automatically by searching across your organization's documentation, GitHub issues, and related specs. Questions that Dewey can answer are resolved silently and recorded in the spec.

Questions that Dewey cannot answer are collected and presented to you as an exit point. You answer them in the spec and re-run `/unleash`.

If Dewey is not configured, all clarification questions exit for human input. The pipeline is the same — you answer more questions yourself.

### 2. Plan

Generates a technical implementation plan (`plan.md`) with:

- Technical context (stack, dependencies, constraints)
- Constitution alignment check (every plan must pass)
- Research phase (resolving unknowns from the spec)
- Design decisions with rationale and alternatives considered

### 3. Tasks

Generates a dependency-ordered task list (`tasks.md`) organized by user story. Each task has:

- A unique ID and priority label
- A `[P]` marker if it can run in parallel with other tasks
- The exact file path to modify
- A description specific enough to implement without additional context

### 4. Spec Review

Runs [The Divisor](/docs/team/the-divisor/) review council in spec review mode. The council's personas evaluate the spec artifacts for completeness, testability, ambiguity, governance gaps, and cross-spec consistency.

LOW and MEDIUM findings are auto-fixed. If HIGH or CRITICAL findings remain, the pipeline exits with the findings and suggests running `/speckit.clarify` to address them.

### 5. Implement

Executes tasks phase by phase. Within each phase:

- **Sequential tasks** run in order via the [Cobalt-Crush](/docs/team/cobalt-crush/) developer agent
- **Parallel tasks** (marked `[P]`) spawn independent [Replicator](/docs/getting-started/developer/#working-with-replicator) workers, each in a dedicated git worktree, up to 4 concurrent workers

After each phase, the pipeline runs the CI build and test commands (derived from `.github/workflows/`, not hardcoded). If anything fails, it exits with the failure details.

If Replicator worktrees are not available, parallel tasks fall back to sequential execution. The pipeline adapts to whatever tools are installed.

### 6. Code Review

Runs the Divisor review council in code review mode. This includes:

- CI hard gate (lint, vulnerability checks)
- Gaze quality analysis (if installed) — contract coverage, CRAP scores
- The Divisor review council evaluating the implementation

If findings exist, the pipeline attempts to fix them and re-review, up to 3 iterations. Persistent findings exit for human resolution.

### 7. Retrospective

Analyzes the session — what was built, what the review council found, what patterns emerged — and stores learnings in semantic memory via [Hivemind](https://github.com/unbound-force/unbound-force). These learnings surface in future sessions when similar problems arise.

If Hivemind is not available, learnings are displayed in the output instead of stored.

### 8. Demo

Presents structured output:

- **What was built** — summary from the spec's user stories
- **How to verify** — steps from the quickstart or acceptance scenarios
- **Key files changed** — grouped by directory
- **Test results** — pass/fail summary
- **Next steps** — run `/finale` to ship, or `/speckit.clarify` to iterate

## Where It Pauses

`/unleash` is autonomous, not unattended. It exits cleanly at five specific points where human judgment is needed:

| Exit Point                  | Why It Pauses                                 | What You Do                                       |
| --------------------------- | --------------------------------------------- | ------------------------------------------------- |
| Unanswerable clarification  | Dewey could not resolve a spec question       | Answer the question in spec.md, re-run `/unleash` |
| HIGH/CRITICAL spec findings | Review council found serious spec issues      | Run `/speckit.clarify`, re-run `/unleash`         |
| Build/test failure          | CI commands failed after a phase              | Fix the failure, re-run `/unleash`                |
| Merge conflict              | Parallel workers produced conflicting changes | Resolve conflicts, re-run `/unleash`              |
| Exhausted code review       | 3 review iterations without full approval     | Fix remaining findings, re-run `/unleash`         |

Every exit message tells you what happened, what to do next, and how to resume. Re-running `/unleash` after fixing the issue skips all completed stages and picks up exactly where it left off.

## The Complete Loop

`/unleash` builds. [`/finale`](/docs/getting-started/common-workflows/#end-of-branch-workflow-finale) ships.

After `/unleash` presents demo instructions, run `/finale` to automate the end-of-branch workflow: stage all changes, generate a conventional commit message, push, create a PR, watch CI checks, rebase-merge, and return to `main`. The full developer loop is two commands:

```text
/unleash       # spec → demo-ready code
/finale        # commit → push → PR → merge → main
```

## Every Tool Is Optional

`/unleash` is designed for graceful degradation. Every external tool it uses has a fallback:

| Tool       | If Available                          | If Not Available               |
| ---------- | ------------------------------------- | ------------------------------ |
| Dewey      | Auto-resolves clarification questions | Questions exit for human input |
| Replicator | Parallel task execution in worktrees  | Sequential execution           |
| Gaze       | Quality analysis in code review       | Code review skips quality data |
| Hivemind   | Stores retrospective learnings        | Displays learnings in output   |

The pipeline works with all four tools, with none of them, or with any combination. You get the most autonomous experience with the full stack, but the pipeline never fails because a tool is missing.

## Where It Works Best

`/unleash` works best for well-scoped features with clear specifications — the kind of work where a senior developer could describe the approach in a few sentences. Complex cross-cutting refactors, ambiguous requirements, and large-scale architectural changes still benefit from the step-by-step [Speckit commands](/docs/getting-started/developer/#working-with-speckit) where you control each transition. The exit-and-resume design means you can start with `/unleash` and drop to manual control if the pipeline pauses at a point where you want finer-grained direction.

## Getting Started

Install the Unbound Force toolchain:

```bash
brew install unbound-force/tap/unbound-force
uf setup
```

Create a feature specification:

```text
/speckit.specify
```

Describe what you want to build. Then unleash the swarm:

```text
/unleash
```

See the [Quick Start guide](/docs/getting-started/quick-start/) for detailed installation and setup instructions, or read the [Common Workflows](/docs/getting-started/common-workflows/) reference for the full pipeline documentation.
