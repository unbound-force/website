---
title: "Architecture & Design Philosophy"
description: "The unified system architecture behind Unbound Force — three-tier context, layered governance, control matrix, and composability."
lead: "How six architectural principles connect the tools, agents, and workflows into a coherent system."
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 82
toc: true
---

## Why Architecture Matters

Each page in this documentation describes a piece of Unbound Force: [Dewey](/docs/getting-started/knowledge/) provides semantic search, [convention packs](/docs/getting-started/developer/#convention-packs) encode coding standards, [the Divisor](/docs/team/the-divisor/) runs multi-agent code review, and the [Speckit pipeline](/docs/getting-started/common-workflows/#autonomous-pipeline-unleash) enforces plan-before-execute discipline. But none of those pages explains why the system is structured the way it is — why these specific components exist, how they interact, and what design principles hold them together.

This page presents the six architectural principles that shape how Unbound Force works. Understanding them helps you predict how the system will behave, diagnose problems when something goes wrong, and make informed decisions about which components to adopt.

## Three-Tier Context System

AI agents perform better when given concrete context — real file paths, real code patterns, real project decisions — rather than abstract instructions. Research across multiple AI engineering teams has converged on this finding: showing the agent a map of the territory consistently outperforms giving it a manual of procedures (Yanli Liu, "Harness Engineering," *AI Advances*, Apr 2026).

Unbound Force implements context delivery through three tiers, each serving a different purpose:

**Tier 1 — Static documentation.** The `AGENTS.md` file in every repository provides project structure, build commands, coding conventions, and behavioral constraints. Agents read this at session start. It is the equivalent of onboarding documentation for a new team member — comprehensive, stable, and always available.

**Tier 2 — Versioned rules.** [Convention packs](/docs/getting-started/developer/#convention-packs) encode specific coding standards as numbered, classified rules (MUST, SHOULD, MAY per RFC 2119). Each pack targets a domain: Go code, content writing, default project standards. Unlike static documentation, convention packs are portable — `uf init` deploys the same packs to every project, ensuring consistent standards across repositories.

**Tier 3 — Dynamic semantic memory.** [Dewey](/docs/getting-started/knowledge/) provides a searchable knowledge layer over repository content, GitHub issues, web documentation, and stored learnings from prior sessions. Agents query Dewey before starting work, retrieving relevant decisions, patterns, and architectural context. This layer adapts over time as the knowledge base grows.

The three tiers are additive: Tier 1 works alone (any project with an AGENTS.md benefits), Tier 2 adds enforcement (convention packs standardize across projects), and Tier 3 adds memory (Dewey provides cross-session, cross-repo context). A team can adopt one tier and add others as their needs grow.

## Layered Governance Model

Large-scale agent systems need governance — rules that constrain what agents can do, enforced at specific checkpoints. Without governance, agents make incompatible assumptions, skip quality checks, or drift from the project's intent. With too much governance, the system becomes rigid and slow.

Unbound Force layers governance at five levels, from most authoritative to most specific:

1. **[Constitution](/docs/getting-started/constitution/)** — The highest-authority document. Defines core principles (Autonomous Collaboration, Composability First, Observable Quality, Testability) that all work must align with. Constitution violations are automatically CRITICAL severity. Amendments require a pull request with a migration plan.

2. **Convention packs** — Portable rule sets that encode coding standards, security policies, and documentation requirements. Each rule is numbered and classified by severity (MUST/SHOULD/MAY). Convention packs sit below the constitution — they may add specificity but never contradict constitutional principles.

3. **Agent personas** — Individual agent configurations that define each hero's role, capabilities, tool access, and behavioral constraints. Personas are bound by convention packs and the constitution but specialize them for a specific function (development, testing, review, documentation).

4. **Commands** — Slash commands (`/unleash`, `/review-council`, `/speckit.implement`) that orchestrate multi-step workflows. Commands define the sequence of operations, exit points, and handoff protocols. They operate within the constraints set by personas, packs, and the constitution.

5. **CI pipelines** — Automated checks (linters, test suites, vulnerability scanners) that enforce computational constraints. CI is the final enforcement layer — if code passes all higher layers but fails CI, it does not ship.

Each layer constrains the layers below it. An agent persona cannot override a convention pack rule. A command cannot bypass a constitutional principle. This prevents the governance model from being eroded by any single component.

## Control Matrix

How do you classify the different types of controls in an agent system? One useful framework organizes controls along two axes (adapted from ThoughtWorks' agent taxonomy, as described in Liu, "Harness Engineering," *AI Advances*, Apr 2026):

- **Feedforward vs. feedback**: Does the control guide agents *before* they act (feedforward), or evaluate what they produced *after* (feedback)?
- **Computational vs. inferential**: Is the control deterministic and fast (computational), or does it require judgment and semantic understanding (inferential)?

This produces four quadrants:

|  | Computational | Inferential |
|---|---|---|
| **Feedforward** (guides before action) | Type systems, linter rules, convention packs, JSON Schema validation | AGENTS.md, constitution, spec artifacts, agent persona instructions, Dewey knowledge retrieval |
| **Feedback** (evaluates after action) | Test suites, [Gaze](/docs/team/gaze-tester/) CRAP scores, coverage analysis, vulnerability scanners | [Divisor Council](/docs/team/the-divisor/) reviews, constitution check, retrospective learnings |

Most teams have strong computational controls (tests, linters) but weak inferential ones (relying on human code review as the only semantic check). Unbound Force invests in all four quadrants — Dewey provides inferential feedforward (semantic context before work begins), and the Divisor Council provides inferential feedback (multi-agent semantic review after work completes).

## Doer/Judge Separation

When the same entity both produces and evaluates work, the evaluation is compromised. An agent that wrote code and then reviews its own code is likely to rationalize its choices rather than find genuine issues. This is a well-documented finding across AI agent research: separating the doer from the judge produces higher-quality output (Anthropic's multi-agent architecture, as described in Liu, "Harness Engineering," *AI Advances*, Apr 2026).

Unbound Force enforces this separation structurally, not just procedurally:

- **[Cobalt-Crush](/docs/team/cobalt-crush/)** writes code. It has full tool access: read, write, edit, bash.
- **[The Divisor](/docs/team/the-divisor/)** reviews code. The Guard agent operates at temperature 0.1 with `write: false`, `edit: false`, `bash: false` — it structurally *cannot* modify files. It can only read and report.

The tool access restriction is the key design choice. A process rule ("don't modify files during review") can be violated. A structural restriction (the agent literally lacks write permissions) cannot. The judge is physically unable to fix what it finds, forcing findings to go through the implementation agent with full visibility.

The Divisor Council extends this further with 5+ specialized review agents running in parallel, each with exclusive ownership boundaries and explicit "Out of Scope" sections to prevent overlap. This is a more granular separation than a single evaluator — each agent focuses on one dimension of quality (drift detection, structural integrity, test coverage, operational readiness, documentation completeness).

## Plan/Execute Separation

Letting an agent plan and execute in the same pass produces unreliable output. Every major AI engineering team has independently discovered this: the planning step must be separate from execution, with its output reviewed before implementation begins (Liu, "Harness Engineering," *AI Advances*, Apr 2026).

Unbound Force implements this as an 8-phase pipeline with hard gates between phases:

```
constitution → specify → clarify → plan → tasks → analyze → checklist → implement
```

The phases enforce a strict progression:
- You cannot plan before you specify
- You cannot create tasks before you plan
- You cannot implement before spec review passes
- All spec artifacts must be committed before implementation begins (Spec Commit Gate)

Phase boundaries are enforced rules, not suggestions. If an agent attempts to make code changes during the planning phase, it triggers a process violation and must stop. The [common workflows page](/docs/getting-started/common-workflows/) describes the full pipeline in detail, including the `/unleash` command that orchestrates all eight phases with six defined exit points where human judgment is required.

This separation applies at two levels: the Speckit pipeline separates planning from execution at the feature level, and [OpenSpec](/docs/getting-started/common-workflows/#bug-fix-tactical) provides a lighter workflow for tactical changes that still separates proposal from implementation.

## Composability

Every component in Unbound Force is designed to be independently useful. You do not need the full system to get value from any single piece:

- **Gaze** analyzes Go code quality without requiring any other Unbound Force tool. Install it, run it, get CRAP scores and coverage analysis.
- **Dewey** provides semantic search over any Markdown-based knowledge base. It works with any AI coding environment that supports MCP, not just OpenCode.
- **Convention packs** are portable Markdown files. You can read them as documentation, use them as prompt context, or enforce them through CI — no tooling dependency.
- **The Speckit pipeline** can be followed manually with any AI assistant. The `/speckit.*` commands automate it, but the underlying workflow is documented and executable without automation.

Graceful degradation is a design principle, not an accident. When Dewey is unavailable, agents fall back to grep and direct file reads — less context, but still functional. When Gaze is not installed, the review council skips the quality analysis phase and proceeds with CI checks and agent reviews. When convention packs are not present, agents still follow AGENTS.md conventions.

This composability means teams can adopt Unbound Force incrementally. Start with one tool that solves an immediate problem. Add others when the need arises. The system gets more powerful as more components are present, but never requires all of them to function.
