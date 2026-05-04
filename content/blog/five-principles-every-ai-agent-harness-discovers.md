---
title: "Five Principles Every AI Agent Harness Discovers"
description: "OpenAI, Anthropic, and ThoughtWorks independently arrived at the same five conclusions about AI agent systems. Here is how we implement them."
lead: "If you are building AI agent workflows, you will discover these five principles whether you want to or not."
slug: "five-principles-ai-agent-harness"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 50
toc: true
categories: ["Engineering"]
tags: ["agents", "swarm", "architecture", "pipeline", "quality"]
contributors: ["Unbound Force"]
---

## The Pattern Nobody Expected

Three independent teams — OpenAI, Anthropic, and ThoughtWorks — each spent months building AI agent harnesses. They started from different assumptions, used different architectures, and optimized for different goals. They arrived at the same five conclusions.

Yanli Liu documented this convergence in "Harness Engineering: What Every AI Engineer Needs to Know in 2026" (*AI Advances*, Apr 2026), cataloging the principles that every team discovers when they move from toy demos to production-grade agent systems. The article has 1.7K claps for a reason: if you have built an agent harness, you recognize every finding immediately.

Unbound Force implements all five principles. In several cases, it goes further than any of the three teams Liu describes. This post walks through each principle with concrete evidence from the codebase.

## Principle 1: Context Beats Instructions

**The finding**: Showing the agent real file paths, real code patterns, and real progress consistently outperforms abstract instructions. OpenAI learned to "give a map, not a manual." Anthropic built structured feature lists. ThoughtWorks calls it "feedforward" (Liu, "Harness Engineering," *AI Advances*, Apr 2026).

**How Unbound Force implements it**: Through three layers of context, each serving a different purpose.

The first layer is static documentation. Every repository contains an `AGENTS.md` file — a comprehensive project overview that agents read at session start. It describes the project structure, build commands, coding conventions, and behavioral constraints. Think of it as onboarding documentation for a new team member who happens to be an AI.

The second layer is [convention packs](/docs/getting-started/developer/#convention-packs). These are portable rule sets where each rule is numbered and classified as MUST, SHOULD, or MAY (per RFC 2119). A convention pack for Go code specifies naming rules, test patterns, error handling standards, and security policies. Unlike free-form documentation, convention packs give agents grounded, specific standards with explicit severity levels.

The third layer is [Dewey](/docs/getting-started/knowledge/) — a semantic knowledge layer that indexes repository content, GitHub issues, web documentation, and stored learnings from prior sessions. Agents query Dewey before starting work, retrieving relevant decisions, patterns, and architectural context. This is not a static file — it adapts over time as the knowledge base grows.

Most teams implement one of these layers. Unbound Force stacks all three: static docs for stability, versioned rules for enforcement, dynamic memory for adaptation.

## Principle 2: Planning and Execution Must Be Separated

**The finding**: Every team discovered that letting an agent plan and execute in the same pass produces unreliable output. The planning step must be separate, with its output reviewed before implementation begins (Liu, "Harness Engineering," *AI Advances*, Apr 2026).

**How Unbound Force implements it**: Through an 8-phase pipeline with hard gates between phases.

```
constitution → specify → clarify → plan → tasks → analyze → checklist → implement
```

This is not a suggestion — it is enforced. If an agent attempts to write code during the planning phase, it triggers a process violation and must stop. The phase boundaries are structural: you cannot plan before you specify, cannot create tasks before you plan, and cannot implement before spec review passes.

The enforcement mechanisms are concrete:
- Branch naming conventions gate pipeline entry
- All spec artifacts must be committed before implementation begins (the Spec Commit Gate)
- The [/unleash](/docs/getting-started/common-workflows/#autonomous-pipeline-unleash) command has six defined exit points where human judgment is required

Liu's article describes plan/execute separation as a two-phase concern. Unbound Force has eight phases with hard gates between them — significantly more granular than anything described in the three teams.

## Principle 3: Feedback Loops Are Non-Negotiable

**The finding**: All three teams agree that a system without a feedback mechanism is "just a prompt with extra steps." They disagree on whether feedback should come from automated tests, another LLM, or both. ThoughtWorks says: use both, layered — computational feedback first (fast, cheap, deterministic), inferential feedback second (slow, expensive, semantic) (Liu, "Harness Engineering," *AI Advances*, Apr 2026).

**How Unbound Force implements it**: Both, layered — exactly what ThoughtWorks recommends.

The computational feedback layer runs first: test suites, linters, vulnerability scanners, and [Gaze](/docs/team/gaze-tester/) static analysis (CRAP scores, coverage analysis, testability classification). These checks are fast, deterministic, and cheap. If they fail, there is no point running semantic review.

The inferential feedback layer runs second: [the Divisor Council](/docs/team/the-divisor/) — five or more specialized review agents running in parallel. Each agent has exclusive ownership boundaries and explicit "Out of Scope" sections. The Guard agent detects intent drift. The Architect reviews structural integrity. The Adversary stress-tests security. The Tester evaluates test quality. The SRE assesses operational readiness.

Liu's article describes at most a three-agent review system (Planner/Generator/Evaluator). Unbound Force runs 5+ specialized evaluators in parallel, each focused on one dimension of quality.

The Anthropic finding — separate the doer from the judge — is fully realized. The Guard agent operates at temperature 0.1 with tool access restrictions that structurally prevent it from modifying files. It can read and report, but it physically cannot fix what it finds. The separation is not a process rule that can be violated; it is a structural constraint that cannot be bypassed.

## Principle 4: One Thing at a Time

**The finding**: Agents that try to do too much at once lose coherence. Forced incrementalism — where the agent completes one unit of work before starting the next — is universal across every successful implementation (Liu, "Harness Engineering," *AI Advances*, Apr 2026).

**How Unbound Force implements it**: At two levels.

At the implementation level, tasks are completed and checked off sequentially. The task completion bookkeeping rule requires marking tasks complete immediately, not in a batch. Parallel execution is only permitted within a phase for tasks explicitly marked as independent, using isolated worktrees with cherry-pick merge — and even then, merge conflicts trigger an exit point.

At the specification level, the 8-phase pipeline enforces incrementalism on the planning itself. You cannot skip ahead. Each phase produces a specific artifact that is reviewed before the next phase begins. This is more disciplined than any of the three teams Liu describes — most enforce incrementalism at the implementation level only, not at the specification level.

## Principle 5: The Codebase Is the Documentation

**The finding**: Nobody maintains a separate knowledge base for the agent. The repository is the single source of truth. Teams that invest in code organization, clear module boundaries, and embedded documentation get better agent performance for free (Liu, "Harness Engineering," *AI Advances*, Apr 2026).

**How Unbound Force implements it**: All agent context lives in the repository.

- 20+ agent persona files in `.opencode/agents/`
- 15+ command files in `.opencode/command/`
- Convention packs in `.opencode/uf/packs/`
- The [constitution](/docs/getting-started/constitution/) in `.specify/memory/`
- Skills, specs, and workflow configuration

The scaffold engine uses Go's `embed.FS` to bundle all these files and deploy them via `uf init`. Drift detection tests ensure embedded assets match their canonical sources. If the scaffolded files diverge from the originals, tests fail.

Dewey adds a dynamic search layer over the repository without replacing it as the source of truth. The repo remains canonical; Dewey is a semantic index over the codebase, not a separate knowledge store.

## The Principles Reinforce Each Other

These five principles are not independent. They create a reinforcing system:

Context (Principle 1) makes planning (Principle 2) possible — agents cannot plan without understanding the codebase. Planning makes incrementalism (Principle 4) possible — you need a plan to know what "one thing" means. Feedback loops (Principle 3) validate each increment. And embedding all of this in the codebase (Principle 5) ensures context stays current for the next cycle.

Remove any one principle and the others weaken. Skip feedback loops and planning becomes theater. Skip planning and incrementalism degenerates into random wandering. Skip codebase-as-documentation and context goes stale.

This is why teams keep rediscovering the same five principles. They are not a checklist of best practices — they are a mutually dependent system.

## The Honest Part: Harness Weight

Liu's article includes a warning that applies directly to Unbound Force: "the harness you build today will need to be partially dismantled tomorrow."

The system is comprehensive. AGENTS.md, convention packs, agent personas, commands, skills, constitution, specs — collectively thousands of lines of configuration. Every line was added for a reason. But model capabilities are improving, and some of this configuration may be compensating for limitations that future models will outgrow.

The constitution and governance model are likely decay-resistant — organizational values and process rules do not expire with model improvements. CI pipeline checks are deterministic and model-independent. Convention packs encode codebase properties, not model properties.

But the 5-agent Divisor Council may be heavier than needed as models improve at self-review. Detailed step-by-step instructions in agent personas may become unnecessary. The 3-iteration review cap may need adjustment.

The honest answer is that we do not yet know which components will prove to be permanent architecture and which will turn out to be scaffolding. The "build to delete" principle applies to the harness itself: build each component so it can be removed when the time comes, and periodically test whether quality holds without it.

## Where to Start

If you are evaluating Unbound Force, the [quick start guide](/docs/getting-started/quick-start/) gets you from zero to running in minutes. If you want to understand the individual tools, the [Getting Started](/docs/getting-started/) section walks through each one. The [common workflows](/docs/getting-started/common-workflows/) page shows how the pieces work together in practice.

The five principles are not features you turn on. They are architectural decisions that shape how the system behaves. Understanding them helps you predict what the swarm will do, diagnose problems when something goes wrong, and decide which components to adopt for your own projects.
