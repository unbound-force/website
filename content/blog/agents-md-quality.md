---
title: "Your AI Agent's First Read — Why AGENTS.md Quality Determines Code Quality"
description: "AGENTS.md is the first file every AI coding agent reads. A weak one means the agent starts with wrong assumptions. /agent-brief creates or audits yours with a 5-tier scoring rubric."
lead: "Garbage in, garbage out. Your AGENTS.md is the prompt that shapes every line of AI-generated code. Audit it, do not guess."
slug: "agents-md-quality"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 55
toc: true
categories: ["Engineering"]
tags: ["agents-md", "context", "quality", "agent-brief"]
contributors: ["Unbound Force"]
---

## The Problem

Every AI coding agent — Claude Code, Cursor, OpenCode, Copilot — reads a context file at the start of each session. That file tells the agent how to build the project, what conventions to follow, where the tests live, and what constraints to respect. In the Unbound Force ecosystem, this file is `AGENTS.md`. Other tools use `.cursorrules`, `CLAUDE.md`, or similar formats. The name varies; the problem is universal.

When this file is weak, missing, or stale, the agent starts every session with wrong assumptions. It guesses at build commands. It invents conventions that conflict with the team's standards. It writes code that compiles and passes linting but does not fit — wrong error handling patterns, wrong directory structure, wrong import organization. The code looks correct to a tool but wrong to a human reviewer.

The fix is not "be more specific in your prompts." The fix is treating your project context file as production infrastructure — structured, auditable, and maintained.

## What Makes a Good AGENTS.md

A complete AGENTS.md has three tiers of sections, each serving a different purpose:

### Tier 1: Essential Sections

Every AGENTS.md needs these. Without them, the agent is guessing at fundamentals.

| Section | Purpose | What to Include |
|---------|---------|-----------------|
| **Project Overview** | What is this project? | Type (CLI, library, API), domain, license |
| **Build & Test Commands** | How do I build and test? | Exact commands with flags, in fenced code blocks |
| **Project Structure** | Where is everything? | Directory tree with annotations |
| **Code Conventions** | How should I write code? | Commit format, naming, error handling, import order |
| **Technology Stack** | What tools and versions? | Language versions, key dependencies, frameworks |

These five sections are the minimum. An agent with all five can produce code that builds, follows conventions, and lands in the right directory.

### Tier 1C: Context-Sensitive Sections

These sections are triggered by project signals. They are essential *when applicable* but irrelevant otherwise.

- **Constitution / Governance**: Only needed when the project has a constitution file (`.specify/memory/constitution.md`). Summarizes the governing principles that all work must align with.
- **Spec Framework**: Only needed when the project uses Speckit (`specs/` directory) or OpenSpec (`openspec/config.yaml`). Describes the specification workflow and pipeline stages.

An AGENTS.md that omits a Tier 1C section when the trigger exists is incomplete — the agent will not know about governance constraints or spec requirements that other tools enforce.

### Tier 2: Advanced Sections

These sections differentiate a strong AGENTS.md from an adequate one. They capture the institutional knowledge that takes weeks to discover.

| Section | Purpose |
|---------|---------|
| **Architecture** | Patterns, design decisions, module boundaries |
| **Testing Conventions** | Test organization, fixture patterns, coverage expectations |
| **Git & Workflow** | Branching strategy, PR requirements, branch protection |
| **Behavioral Constraints** | Things agents must NEVER do (often more impactful than positive instructions) |

A Behavioral Constraints section is particularly valuable. Negative instructions like "never modify coverage thresholds to make tests pass" and "never commit directly to main" prevent entire categories of agent mistakes.

## The 5-Tier Scoring Rubric

How do you know if your AGENTS.md is good enough? The `/agent-brief` command uses a deterministic scoring rubric:

| Score | Criteria |
|-------|----------|
| **Excellent** | 5/5 Tier 1 + 4/4 Tier 2 + all applicable Tier 1C |
| **Strong** | 5/5 Tier 1 + 2-3/4 Tier 2 |
| **Adequate** | 4-5/5 Tier 1 |
| **Weak** | 2-3/5 Tier 1 |
| **Missing** | 0-1/5 Tier 1 |

If Tier 1C sections are applicable (the project has a constitution or spec framework) but missing, the score is downgraded by one level. An otherwise "Strong" AGENTS.md becomes "Adequate" if it ignores governance constraints that the project enforces.

The scoring is structural, not subjective. It counts section headers, checks for code blocks in the Build section, verifies that directory trees match the actual filesystem, and confirms that constitution references exist when a constitution is present. Twelve checks in total — no AI judgment involved in the score.

## Audit, Don't Guess

```text
/agent-brief audit
```

The audit command reads your existing AGENTS.md and produces a quality report:

- **Section coverage**: Which Tier 1 / 1C / 2 sections are present vs. missing
- **Quality metrics**: Line count, build code blocks, directory tree accuracy, staleness checks
- **Overall score**: One of the 5 tiers, with justification
- **Recommendations**: For each missing section, generated content you can review and apply

The audit is read-only — it does not modify your file. You decide what to accept.

## Create from Project Signals

```text
/agent-brief create
```

When no AGENTS.md exists, `/agent-brief` analyzes your project and generates one from actual project data:

- **`go.mod`**, **`package.json`**, **`Cargo.toml`** → language, dependencies, versions
- **`Makefile`**, **`.github/workflows/`** → exact build, test, and lint commands (CI files are the source of truth)
- **`.golangci.yml`**, **`ruff.toml`**, **`.eslintrc`** → linter rules and conventions
- **`README.md`**, **`LICENSE`**, **`.git/config`** → project description, license, organization
- **`.specify/memory/constitution.md`**, **`specs/`**, **`openspec/`** → governance and spec framework (Tier 1C triggers)

The generated file has two quality levels: Tier 1 sections are filled from actual project data (concrete commands, real directory trees, detected conventions). Tier 2 sections are stubs with detailed TODO comments explaining what to fill and why it matters. The result is a complete skeleton that you can review and flesh out.

## Bridge Files

AGENTS.md is the canonical project context file, but different AI tools read different files. `/agent-brief` ensures cross-tool compatibility by verifying bridge files:

- **CLAUDE.md**: Should contain `@AGENTS.md` to import the project context into Claude Code sessions
- **.cursorrules**: Should reference AGENTS.md for Cursor IDE integration

Bridge file creation is handled by `uf init`. If bridge files are missing or misconfigured, `/agent-brief` reports the status and suggests running `uf init` to create them.

## This Applies to Every AI Coding Tool

AGENTS.md is not an Unbound Force concept — it is a convention adopted across multiple AI coding tools. Claude Code reads `CLAUDE.md` and `AGENTS.md`. Cursor reads `.cursorrules`. Copilot reads context from repository structure. The names differ, but the principle is the same: the quality of your context file determines the quality of your AI-generated code.

The section taxonomy (Tier 1 / 1C / 2), the scoring rubric, and the structural checks apply regardless of which tool you use. If your `.cursorrules` file has no build commands and no project structure, your Cursor agent is guessing — just like an OpenCode agent with a weak AGENTS.md.

## Get Started

Install the Unbound Force CLI and audit your project:

```bash
brew install unbound-force/tap/unbound-force
/agent-brief audit
```

Find out where your project context is weak. Fix it. Watch the quality of AI-generated code improve.

## See Also

- [Common Workflows](/docs/getting-started/common-workflows/) -- `/agent-brief` create and audit documentation
- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
