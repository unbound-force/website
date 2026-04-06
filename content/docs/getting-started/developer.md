---
title: "Getting Started: Developer"
description: "Set up your environment, learn the daily workflow, and start building with the Unbound Force AI agent swarm as a developer."
lead: "Install the tools, learn the workflows, and start shipping code with Cobalt-Crush, Speckit, and Replicator."
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

`uf setup` detects your existing version managers (goenv, nvm, fnm, Homebrew) and installs through them:

- **Core tools** -- OpenCode (AI coding environment), Gaze (quality analysis), Mx F (manager hero), GitHub CLI
- **Development tools** -- Node.js, OpenSpec CLI, Replicator (multi-agent coordination)
- **Knowledge layer** -- Ollama (local model runtime), Dewey (semantic search), IBM Granite embedding model
- **Project scaffolding** -- agents, commands, convention packs, templates, and workflow configuration via `uf init`

After setup, verify everything is working:

```bash
uf doctor
```

Doctor checks 7 areas: your detected environment (version managers), core tools, Replicator health, scaffolded files, hero availability, MCP server config, and agent/skill integrity. Every failed check includes a copy-pasteable install command to fix it.

## OpenCode Modes

OpenCode has two primary modes you switch between with the **Tab** key:

- **Plan mode** -- Read-only. File edits and shell commands require approval. Use this to explore ideas, analyze code, and think through an approach before committing to it.
- **Build mode** -- Full tool access. File edits, shell commands, and all MCP tools are enabled. Use this when you're ready to make changes.

Start every task in plan mode to think, then switch to build mode to execute.

## Daily Workflow

Two main workflows for large and small tasks:

### Large Tasks (Strategic)

For features that need architectural planning:

1. **Explore** (plan mode): Think through the idea, investigate the codebase, clarify requirements

2. **Specify** (build mode): Create a structured specification

   ```text
   /speckit.specify
   ```

3. **Unleash** (build mode): The swarm takes it from here -- clarification, planning, implementation, testing, and review

   ```text
   /unleash
   ```

   If `/unleash` pauses (unanswerable question, spec finding, build failure), fix the issue and re-run. If the spec needs refinement, run `/speckit.clarify` then `/unleash` again.

4. **Finale** (build mode): Ship it -- commit, push, PR, merge, return to main

   ```text
   /finale
   ```

### Small Tasks (Tactical)

For bug fixes and changes that don't need the full Speckit pipeline:

1. **Explore** (plan mode): Think through the fix, understand the problem

2. **Propose** (build mode): Create a change with proposal, design, and tasks in one step

   ```text
   /opsx-propose fix-auth-timeout
   ```

3. **Implement** (build mode): Invoke Cobalt-Crush to implement with convention pack adherence

   ```text
   /cobalt-crush
   ```

   `/cobalt-crush` delegates to the `cobalt-crush-dev` agent, which loads [convention packs](/docs/getting-started/developer/#convention-packs) and applies the project's coding standards. This gives you the quality enforcement that a bare `/opsx-apply` would skip.

4. **Finale** (build mode): Ship it

   ```text
   /finale
   ```

## Working with Speckit

Speckit is the strategic specification pipeline for features that need architectural planning. For autonomous execution of the entire pipeline, run [`/unleash`](/docs/getting-started/common-workflows/#autonomous-pipeline-unleash) -- it handles clarification, planning, implementation, testing, and review in a single command, pausing only when human judgment is needed.

For step-by-step control, use the individual commands:

```text
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

Both workflows enforce branch conventions: Speckit uses `NNN-<short-name>` branches (created by `/speckit.specify`), and OpenSpec uses `opsx/<change-name>` branches (created by `/opsx-propose`). Branch validation is a hard gate at each pipeline step.

## Working with Replicator

[Replicator](/docs/projects/replicator/) coordinates parallel AI agents on your codebase. It provides 53 MCP tools for work tracking, file reservations, parallel orchestration with git worktrees, and semantic memory -- all in a single Go binary.

### Replicator + Speckit Integration

When a `tasks.md` file exists from the Speckit pipeline, Replicator uses it as the authoritative task decomposition instead of generating its own. It maps each phase to an epic and respects the `[P]` parallel markers and phase dependencies. The `/unleash` command orchestrates this automatically.

### Parallel Workers

When `/unleash` encounters `[P]`-marked tasks, Replicator spawns parallel workers in dedicated git worktrees. Each worker:

1. Calls `swarmmail_reserve()` to lock their files
2. Implements their subtask
3. Calls `swarm_complete()` when done
4. Releases file locks automatically

### Autonomous Delegation

The hero lifecycle supports **autonomous swarm delegation**. After the Product Owner completes the define stage (specify + clarify), the swarm takes over and runs the implement, validate, and review stages without human intervention. The workflow pauses automatically before the accept stage, returning control to the Product Owner. After acceptance, the swarm runs the final reflect stage (Mx F) autonomously. This means the developer's work -- planning, implementation, and quality validation -- runs as part of the swarm's autonomous stages.

### Knowledge Retrieval with Dewey

When [Dewey](/docs/getting-started/knowledge/) is configured, Cobalt-Crush uses it to pull in context that would otherwise require manual research. Dewey's semantic search finds conceptually related content even when different terminology is used — so a query about "adding a CLI subcommand" finds Cobra framework documentation, past implementation patterns from other repos, and related spec artifacts.

Example queries Cobalt-Crush makes during implementation:

- `dewey_semantic_search("how to validate JSON schema in Go")` — finds toolstack documentation and implementation patterns from other repositories
- `dewey_similar("specs/005-getting-started-guides/plan.md")` — finds related planning artifacts to inform the current implementation approach
- `dewey_semantic_search_filtered("error handling patterns", source: "github")` — finds error handling discussions in GitHub issues and PRs across the organization

When Dewey is not available, Cobalt-Crush falls back to direct file reads and CLI queries. The implementation workflow is the same — with narrower context. See the [graceful degradation tiers](/docs/getting-started/knowledge/#graceful-degradation) for details.

### File Reservations

Before editing any file under Replicator coordination, always reserve it first:

```
swarmmail_reserve({ paths: ["internal/auth/handler.go"], reason: "Implementing auth handler" })
```

This prevents conflicts when multiple workers are active. Reservations auto-release when you call `swarm_complete()`.

### Session Lifecycle

Every session follows this ritual:

| Step      | Command                               | Purpose                                 |
| --------- | ------------------------------------- | --------------------------------------- |
| **Start** | `/speckit.specify` or `/opsx-propose` | Define the work                         |
| **Work**  | `/unleash` or `/cobalt-crush`         | Execute the work                        |
| **End**   | `/finale`                             | Commit, push, PR, merge, return to main |

## Cobalt-Crush Persona

[Cobalt-Crush](/docs/team/cobalt-crush/) is the developer persona -- the Engineering Core of the swarm. When you invoke `/speckit.implement` or `/cobalt-crush`, you're working with an agent that follows six core principles:

- **Clean Code**: Single-purpose functions, intent-revealing names, no dead code
- **SOLID**: Applied at function, type, and package levels
- **DRY / YAGNI**: Extract only at 3+ duplications
- **Separation of Concerns**: Business logic, I/O, config, and presentation are distinct
- **Test-Driven Awareness**: Every function must be testable in isolation
- **Spec-Driven Development**: Implementation follows the spec, maps to task IDs

### Convention Packs

Convention packs are shared coding standards files stored in `.opencode/unbound/packs/`. Cobalt-Crush follows these conventions during implementation, and The Divisor enforces them during review. Every rule in a pack has a severity tag that determines how violations are handled.

#### Pack Files

Packs are organized by language, with each language having a tool-owned canonical pack and a user-owned customization file:

| File                   | Ownership  | Purpose                                                                                                           |
| ---------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------- |
| `default.md`           | Tool-owned | Language-agnostic rules: coding style, architecture, security, testing, documentation                             |
| `default-custom.md`    | User-owned | Project-specific conventions extending the default pack                                                           |
| `go.md`                | Tool-owned | Go-specific rules: gofmt, error handling, GoDoc, Cobra patterns                                                   |
| `go-custom.md`         | User-owned | Project-specific Go conventions                                                                                   |
| `typescript.md`        | Tool-owned | TypeScript-specific rules: ESLint, Prettier, strict typing, architectural patterns                                |
| `typescript-custom.md` | User-owned | Project-specific TypeScript conventions                                                                           |
| `severity.md`          | Tool-owned | Shared severity definitions for all Divisor personas (calibration standard for CRITICAL/HIGH/MEDIUM/LOW findings) |
| `content.md`           | Tool-owned | Language-agnostic writing standards for documentation, blog posts, and website content                            |
| `content-custom.md`    | User-owned | Project-specific writing conventions                                                                              |

#### Ownership Model

- **Tool-owned** files (`default.md`, `go.md`, `typescript.md`) are automatically updated by `uf init` when the embedded version changes. You should not edit these directly -- your changes will be overwritten on the next `uf init` run.
- **User-owned** files (`*-custom.md`) are never overwritten by `uf init`. Your customizations are preserved across updates. These are the files you edit to add project-specific conventions.

#### Rule Severity Tags

Every rule in a pack is tagged with a severity level:

- `[MUST]` -- Mandatory requirements. Violations block the review (The Divisor will issue REQUEST CHANGES).
- `[SHOULD]` -- Strong recommendations. Violations are flagged but do not block.
- `[MAY]` -- Optional improvements. Noted as suggestions.

These tags map directly to review finding severity -- a `[MUST]` violation produces a CRITICAL or HIGH finding, while a `[MAY]` suggestion produces a LOW finding.

#### Language Auto-Detection

When you run `uf init`, it detects your project's language from marker files and deploys only the matching language pack (plus the default pack):

- `go.mod` detected: deploys `go.md` + `go-custom.md`
- `tsconfig.json` or `package.json` detected: deploys `typescript.md` + `typescript-custom.md`
- No language detected: deploys only the default pack

Override auto-detection with the `--lang` flag: `uf init --lang go`.

#### Adding Custom Rules

To add project-specific conventions:

1. Edit the appropriate `*-custom.md` file (e.g., `go-custom.md` for Go projects)
2. Use the `CR-NNN` prefix for custom rule IDs (e.g., `CR-001`)
3. Tag each rule with a severity level (`[MUST]`, `[SHOULD]`, or `[MAY]`)

Custom rules are loaded by Cobalt-Crush during implementation and by all Divisor personas during review, alongside the canonical pack rules.

### Feedback Loops

Cobalt-Crush integrates with two feedback systems:

- **Gaze feedback**: After writing code, checks `.unbound-force/artifacts/quality-report/` for quality findings. High CRAP scores trigger complexity reduction; low contract coverage triggers test improvements.
- **Divisor feedback**: Before submitting for review, validates against a pre-review checklist. After review, addresses findings by persona and severity (CRITICAL and HIGH first).

## Project Scaffolding with `uf init`

`uf init` scaffolds the project files needed to work with the Unbound Force swarm -- agents, commands, templates, scripts, convention packs, OpenSpec schema, and skills. It runs automatically as the final step of `uf setup`, but you can also run it independently to re-scaffold, initialize a new project, or deploy a subset of the swarm.

### Usage

```bash
uf init [--divisor] [--lang go|typescript] [--force]
```

### Flags

| Flag        | Description                                                                                                                                                                          |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `--divisor` | Deploy only PR review agents (Divisor personas) and convention packs. For projects that only want code review, not the full swarm workflow.                                          |
| `--lang`    | Override language auto-detection for convention pack selection. Auto-detects from `go.mod`, `tsconfig.json`, `package.json`, `pyproject.toml`, `Cargo.toml`.                         |
| `--force`   | Overwrite user-owned files that would normally be skipped, re-index Dewey workspace, and refresh `opencode.json` entries. Use with caution -- this will replace your customizations. |

### What Gets Deployed

`uf init` deploys approximately 50 files across these categories:

| Category         | Files | Examples                                                                                   |
| ---------------- | ----- | ------------------------------------------------------------------------------------------ |
| Agents           | ~12   | 5 Divisor personas, Cobalt-Crush, Constitution Check, Mx F Coach                           |
| Commands         | ~15   | 9 Speckit commands, review-council, constitution-check, cobalt-crush                       |
| Convention Packs | 9     | default, go, typescript, content (each with tool-owned and user-owned variants) + severity |
| Templates        | ~6    | spec, plan, tasks, checklist, constitution, agent-file templates                           |
| Scripts          | ~5    | check-prerequisites, setup-plan, create-new-feature, update-agent-context                  |
| OpenSpec         | ~6    | config, schema, 4 templates (design, proposal, spec, tasks)                                |

File counts are approximate and may change between versions.

### File Ownership Model

Files deployed by `uf init` fall into two ownership categories:

- **Tool-owned** (commands, skills, canonical convention packs, OpenSpec schemas): Automatically updated when the embedded version changes. On re-run, if the file content differs from the embedded version, it is silently updated. Tool-owned files carry a version marker: `<!-- scaffolded by uf v{version} -->`.
- **User-owned** (agent files, custom convention packs `*-custom.md`, specify configs): Never overwritten on re-run. If the file already exists, it is skipped. This preserves your customizations across `uf init` updates. Use `--force` to overwrite if needed.

### Sub-Tool Initialization

After deploying files, `uf init` performs sub-tool initialization:

- Creates `.unbound-force/config.yaml` for [workflow configuration](/docs/getting-started/common-workflows/#workflow-configuration) (skipped if it already exists)
- If [Dewey](/docs/getting-started/knowledge/) is available: creates the `.dewey/` workspace, auto-detects sibling repos and your GitHub org to generate a [multi-repo source config](/docs/getting-started/knowledge/#what-uf-init-creates), and builds the initial index. After setup, you can [extend sources with web crawls](/docs/getting-started/knowledge/#extending-your-sources) for your project's toolstack documentation. With `--force`, re-indexes an existing Dewey workspace.
- Configures `opencode.json` with MCP and plugin entries:
  - **Dewey MCP entry**: When `dewey` is in PATH, adds the `mcp.dewey` entry for the Dewey MCP server
  - **Replicator MCP entry**: When `replicator` is in PATH, adds `mcp.replicator` entry for the Replicator MCP server
  - Idempotent — checks for existing entries before adding. Use `--force` to overwrite stale entries.
  - Preserves user-added keys (custom MCP servers, custom config) — only manages the entries it owns

### Summary Output

After completion, `uf init` shows a summary with file dispositions (`+` created, `~` updated, `!` overwritten, `-` skipped) and context-aware next-step guidance based on what tools are available in your environment.

## Session Ritual

The most important habit: **always end your session properly**. The daily workflow follows a specify → unleash → finale loop. When you are done working, run `/finale` to commit, push, create a PR, and merge:

```text
/finale        # commit → push → PR → merge → main
```

`/finale` handles the full end-of-branch workflow: staging changes, generating a conventional commit message, pushing to remote, creating a PR, watching CI checks, rebase-merging, and returning to `main`. The session is not complete until `git push` succeeds — this ensures your work items, semantic memory learnings, and file reservation state are available for your next session and for other team members who may pick up where you left off.

## Next Steps

- Read the [Common Workflows](/docs/getting-started/common-workflows/) page to understand how your work flows through the full hero lifecycle
- Explore the [Cobalt-Crush](/docs/team/cobalt-crush/) team page for the full persona details
- Run `uf doctor` to verify your environment, then try `/unleash` on your first feature
