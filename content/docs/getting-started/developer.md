---
title: "Getting Started: Developer"
description: "Set up your environment, learn the daily workflow, and start building with the Unbound Force AI agent swarm as a developer."
lead: "Install the tools, learn the workflows, and start shipping code with Cobalt-Crush, Speckit, and Swarm."
date: 2026-03-22T00:00:00+00:00
draft: false
weight: 30
toc: true
---

## Prerequisites

Everything starts with one command. Install the `uf` CLI (short for `unbound-force`), then run setup to install the full tool chain:

```bash
brew install unbound-force/tap/unbound-force
uf setup
```

`uf setup` detects your existing version managers (goenv, nvm, pyenv, Homebrew) and installs through them:

- **OpenCode** -- the AI coding environment where you work
- **Gaze** -- quality analysis for your Go code
- **Swarm plugin** -- multi-agent coordination for parallel work
- **Project scaffolding** -- agents, commands, convention packs, and templates

After setup, verify everything is working:

```bash
uf doctor
```

Doctor checks 7 areas: your detected environment (version managers), core tools, Swarm plugin health, scaffolded files, hero availability, MCP server config, and agent/skill integrity. Every failed check includes a copy-pasteable install command to fix it.

## Daily Workflow

A typical development session follows this progression:

1. **Check your environment**: Run `uf doctor` to verify all tools are healthy
2. **Open OpenCode**: Start your AI coding environment
3. **Check for work**: Run `hive_ready()` to see what tasks are available
4. **Work**: Use `/swarm "task"` for parallel task execution, or code directly with Cobalt-Crush
5. **End your session**: Run `hive_sync()` then `git push`

The session lifecycle is critical -- **the plane is not landed until `git push` succeeds**. This ensures your work items, learnings, and file reservations are persisted for the next session.

## Working with Speckit

Speckit is the strategic specification pipeline for features that need architectural planning. It follows a strict sequential flow:

```
/speckit.specify    Create the feature specification
/speckit.clarify    Reduce ambiguity (interactive Q&A)
/speckit.plan       Generate the technical implementation plan
/speckit.tasks      Generate the task breakdown
/speckit.analyze    Cross-artifact consistency analysis
/speckit.checklist  Generate quality validation checklists
/speckit.implement  Execute the implementation plan
```

Each stage produces artifacts that feed the next. Specs must be committed and pushed before implementation begins (the Spec Commit Gate).

### When to Use Speckit vs. OpenSpec

| Use Speckit when...                 | Use OpenSpec when...   |
| ----------------------------------- | ---------------------- |
| 3+ user stories                     | 1-2 tasks              |
| Cross-cutting architectural changes | Bug fixes              |
| New hero or capability              | Small improvements     |
| Needs formal review                 | Quick tactical changes |

**OpenSpec** is the tactical workflow for smaller changes. It uses `/opsx-propose` to create a change with proposal, design, and tasks artifacts, then `/opsx-apply` to implement. See [Common Workflows](/docs/getting-started/common-workflows/) for the full flow.

## Working with Swarm

Swarm coordinates parallel AI agents on your codebase. When you have a task that can be broken into independent pieces, Swarm decomposes it and spawns parallel workers.

### Using `/swarm`

```
/swarm "Implement the authentication module"
```

Swarm analyzes the task, decomposes it into subtasks, reserves files for each worker to prevent conflicts, and spawns parallel agents. Each worker:

1. Calls `swarmmail_reserve()` to lock their files
2. Implements their subtask
3. Calls `swarm_complete()` when done
4. Releases file locks automatically

### Swarm + Speckit Integration

When a `tasks.md` file exists from the Speckit pipeline, Swarm uses it as the authoritative task decomposition instead of generating its own. It maps each phase to a Swarm epic and respects the `[P]` parallel markers and phase dependencies.

### Swarm Delegation

The hero lifecycle supports **autonomous swarm delegation**. After the Product Owner completes the define stage (specify + clarify), the swarm takes over and runs the implement, validate, and review stages without human intervention. The workflow pauses automatically before the accept stage, returning control to the Product Owner. After acceptance, the swarm runs the final reflect stage (Mx F) autonomously. This means the developer's work -- planning, implementation, and quality validation -- runs as part of the swarm's autonomous stages.

### Knowledge Retrieval with Dewey

When [Dewey](/docs/getting-started/knowledge/) is configured, Cobalt-Crush uses it to pull in context that would otherwise require manual research. Dewey's semantic search finds conceptually related content even when different terminology is used — so a query about "adding a CLI subcommand" finds Cobra framework documentation, past implementation patterns from other repos, and related spec artifacts.

Example queries Cobalt-Crush makes during implementation:

- `dewey_semantic_search("how to validate JSON schema in Go")` — finds toolstack documentation and implementation patterns from other repositories
- `dewey_similar("specs/005-getting-started-guides/plan.md")` — finds related planning artifacts to inform the current implementation approach
- `dewey_semantic_search_filtered("error handling patterns", source: "github")` — finds error handling discussions in GitHub issues and PRs across the organization

When Dewey is not available, Cobalt-Crush falls back to direct file reads and CLI queries. The implementation workflow is the same — just with narrower context. See the [graceful degradation tiers](/docs/getting-started/knowledge/#graceful-degradation) for details.

### File Reservations

Before editing any file under Swarm coordination, always reserve it first:

```
swarmmail_reserve({ paths: ["internal/auth/handler.go"], reason: "Implementing auth handler" })
```

This prevents conflicts when multiple workers are active. Reservations auto-release when you call `swarm_complete()`.

### Session Lifecycle

Every session follows this ritual:

| Step      | Command                          | Purpose                          |
| --------- | -------------------------------- | -------------------------------- |
| **Start** | `hive_ready()`                   | Get the next unblocked work item |
| **Work**  | `/swarm "task"` or direct coding | Execute the work                 |
| **End**   | `hive_sync()` + `git push`       | Persist work items and learnings |

## Cobalt-Crush Persona

[Cobalt-Crush](/docs/team/cobalt-crush/) is the developer persona -- the Engineering Core of the swarm. When you invoke `/speckit.implement` or `/cobalt-crush`, you're working with an agent that follows six core principles:

- **Clean Code**: Single-purpose functions, intent-revealing names, no dead code
- **SOLID**: Applied at function, type, and package levels
- **DRY / YAGNI**: Extract only at 3+ duplications
- **Separation of Concerns**: Business logic, I/O, config, and presentation are distinct
- **Test-Driven Awareness**: Every function must be testable in isolation
- **Spec-Driven Development**: Implementation follows the spec, maps to task IDs

### Convention Packs

Cobalt-Crush loads coding conventions from `.opencode/unbound/packs/`. These are shared standards files with rule severity tags:

- `[MUST]` -- Mandatory requirements (violations block review)
- `[SHOULD]` -- Strong recommendations
- `[MAY]` -- Optional improvements

Packs come in pairs: a tool-owned canonical pack (e.g., `go.md`) and a user-owned customization file (e.g., `go-custom.md`). The canonical packs are updated by `uf init`; your customizations are preserved.

### Feedback Loops

Cobalt-Crush integrates with two feedback systems:

- **Gaze feedback**: After writing code, checks `.unbound-force/artifacts/quality-report/` for quality findings. High CRAP scores trigger complexity reduction; low contract coverage triggers test improvements.
- **Divisor feedback**: Before submitting for review, validates against a pre-review checklist. After review, addresses findings by persona and severity (CRITICAL and HIGH first).

## Session Ritual

The most important habit: **always end your session properly**.

```
1. hive_sync()     # Persist work items to git
2. git push        # Push to remote
3. Verify          # "Your branch is up to date with origin"
```

The plane is not landed until `git push` succeeds. This ensures your work items, semantic memory learnings, and file reservation state are available for your next session -- and for other team members who may pick up where you left off.

## Next Steps

- Read the [Common Workflows](/docs/getting-started/common-workflows/) page to understand how your work flows through the full hero lifecycle
- Explore the [Cobalt-Crush](/docs/team/cobalt-crush/) team page for the full persona details
- Run `uf doctor` to verify your environment, then try `/swarm "your first task"`
