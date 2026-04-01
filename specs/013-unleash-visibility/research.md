# Research: Unleash Visibility

**Date**: 2026-03-31
**Feature**: [spec.md](spec.md) | [plan.md](plan.md)

## R1: Blog Post Format and Tone (from existing posts)

**Decision**: Follow the exact format of the two existing blog posts.

**Source**: `content/blog/why-contract-coverage.md`, `content/blog/gaze-in-practice.md`

**Pattern observed**:

- Frontmatter: title, description, lead, slug, date, draft, weight, toc, categories, tags, contributors
- Categories: `["Engineering"]`
- Contributors: `["Unbound Force"]`
- Body starts with H2 introducing the problem
- Structure: Problem -> What the tool does instead -> Walkthrough/evidence -> Implications -> Getting started
- Tone: Technical but accessible, confident but not promotional, uses real data/output not hypotheticals
- Length: 150-180 lines of Markdown
- No version numbers in prose (timeless)
- No weasel words ("simply", "just", "easily")

**Blog post structure for `/unleash`**:

1. **The Problem**: Manual AI agent pipelines require 8+ commands with human decisions between each step. Most teams run `/speckit.specify`, then manually clarify, then manually plan, etc.
2. **What /unleash Does**: Single command, 8-step pipeline. Exits when it needs you, resumes when you come back.
3. **The 8 Steps**: Walk through each with a brief description (not full documentation -- link out)
4. **Exit Points**: Where it pauses for human judgment (unanswerable clarification questions, HIGH/CRITICAL spec review findings, build failures, exhausted code review iterations)
5. **What Makes It Autonomous**: Dewey clarifies questions, Swarm parallelizes work, Divisor reviews quality, Hivemind stores learnings
6. **The Unleash + Finale Loop**: The complete developer workflow
7. **Getting Started**: Install, setup, create spec, unleash

## R2: Homepage Feature Card Current State

**Decision**: Rewrite the "Speckit Workflow" card text rather than adding a 5th card.

**Source**: `layouts/home.html` lines 128-152

**Current card text**:

```
<h3 class="h5 fw-semibold">Speckit Workflow</h3>
<p class="text-body-secondary mb-0">
  A structured spec-to-implementation pipeline. From specification
  through planning, task generation, and implementation with quality
  gates at every step.
</p>
```

**New card text** (lead with autonomous capability):

```
<h3 class="h5 fw-semibold">One-Command Pipeline</h3>
<p class="text-body-secondary mb-0">
  Run /unleash to go from spec to demo-ready code autonomously.
  The swarm handles clarification, planning, implementation,
  testing, and review — pausing only when it needs you.
</p>
```

**Rationale**: The current card describes the pipeline as a multi-step process. The new card leads with the single-command capability -- the most compelling differentiator.

## R3: Quick Start Current State

**Decision**: Add `/unleash` as the first option in "Start Working", reframe `/speckit.specify` as the manual alternative.

**Source**: `content/docs/getting-started/quick-start.md` lines 37-43

**Current "Start Working" section** offers `/swarm` and `/speckit.specify`. Neither mentions `/unleash`.

**New structure**:

```
For the fastest path — one command from spec to demo:
/unleash

For step-by-step control over each pipeline stage:
/speckit.specify
```

## R4: Developer Guide Current State

**Decision**: Add a prominent `/unleash` callout before the individual Speckit command listing.

**Source**: `content/docs/getting-started/developer.md` "Working with Speckit" section

The section currently lists individual commands (`/speckit.specify`, `/speckit.plan`, etc.) with no mention of `/unleash`. Add a note at the top: "For autonomous execution of the entire pipeline, run `/unleash`. The commands below give you step-by-step control."

## R5: Common Workflows Section Order

**Decision**: Reorder H2 sections to place `/unleash` first.

**Source**: `content/docs/getting-started/common-workflows.md`

**Current H2 order**:

1. New Feature (End-to-End)
2. Bug Fix (Tactical)
3. Autonomous Pipeline (`/unleash`)
4. End-of-Branch Workflow (`/finale`)
5. Code Review
6. Environment Setup
7. Next Steps

**New H2 order**:

1. Autonomous Pipeline (`/unleash`) -- the headline capability
2. End-of-Branch Workflow (`/finale`) -- unleash builds, finale ships
3. New Feature (End-to-End) -- with added note: "for step-by-step control"
4. Bug Fix (Tactical)
5. Code Review
6. Environment Setup
7. Next Steps

**Anchor preservation**: The "New Feature (End-to-End)" heading text stays the same, so the anchor `#new-feature-end-to-end` is preserved.

## R6: /unleash Pipeline Details for Blog Post

**Decision**: Source pipeline details from the /unleash command file and PR #68.

**Source**: Dewey search results for `/unleash` command (upstream `.opencode/command/unleash.md`, GitHub PR #68)

**8-step pipeline**:

1. **Clarify**: Scans spec for `[NEEDS CLARIFICATION]` markers. If Dewey is available, auto-resolves by searching the knowledge graph. Unanswerable questions exit for human input.
2. **Plan**: Generates `plan.md` with technical context, constitution check, research, and design decisions.
3. **Tasks**: Generates `tasks.md` with phased, dependency-ordered task list. Sequential and parallel (`[P]`) tasks.
4. **Spec Review**: Runs the Divisor review council in spec review mode. Auto-fixes LOW/MEDIUM findings. Exits on HIGH/CRITICAL.
5. **Implement**: Executes tasks phase by phase. Parallel tasks spawn Swarm worktrees (max 4 concurrent). Phase checkpoints run CI commands.
6. **Code Review**: Runs the Divisor review council in code review mode. Phase 1a CI gate + Phase 1b Gaze quality. Up to 3 fix iterations.
7. **Retrospective**: Stores learnings in Hivemind semantic memory for future sessions.
8. **Demo**: Presents what was built, how to verify, key files changed, and next steps (`/finale` or iterate).

**Exit points** (pipeline pauses for human judgment):

- Unanswerable clarification questions (Dewey couldn't resolve)
- HIGH/CRITICAL spec review findings
- Build/test failures after a phase
- Parallel worker failures or merge conflicts
- 3 code review iterations exhausted

**Resumability**: On re-run, `/unleash` detects which steps are complete by checking filesystem state (plan.md exists? tasks.md has spec-review marker? all checkboxes done?) and skips forward.

**Graceful degradation**: Every tool is optional:

- Without Dewey: clarification questions require human input
- Without Swarm: parallel tasks run sequentially
- Without Gaze: code review skips quality analysis
- Without Hivemind: retrospective displays learnings instead of storing them

## R7: Getting Started Landing Page and Product Owner Guide

**Decision**: Minor text updates to surface `/unleash`.

**Getting Started `_index.md`**: Change Common Workflows description from "End-to-end flows for features, bug fixes, code reviews, and setup" to something that mentions the autonomous pipeline.

**Product Owner `product-owner.md`**: Add `/unleash` alongside `/workflow seed` in the autonomous workflow section. The PO guide describes the swarm's autonomous behavior conceptually but never names the command.
