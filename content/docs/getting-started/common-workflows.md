---
title: "Common Workflows"
description: "The /unleash autonomous pipeline, /finale shipping workflow, manual feature flows, bug fixes, code reviews, and environment setup."
lead: "End-to-end workflows that show how all five heroes collaborate across the development lifecycle."
date: 2026-03-22T00:00:00+00:00
draft: false
weight: 70
toc: true
---

## Autonomous Pipeline (`/unleash`)

`/unleash` is the autonomous Speckit pipeline execution command. It takes a spec from draft to demo-ready code in a single command, orchestrating the full pipeline with graceful exit points and full resumability.

### The Pipeline

| Step | Name              | Description                                                                                                                                                                        |
| ---- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | **Clarify**       | Scans spec.md for `[NEEDS CLARIFICATION]` markers. Uses Dewey semantic search to auto-resolve. Exits to human only for unanswerable questions.                                     |
| 2    | **Plan**          | Delegates to Cobalt-Crush to generate `plan.md` via `/speckit.plan`                                                                                                                |
| 3    | **Tasks**         | Delegates to Cobalt-Crush to generate `tasks.md` via `/speckit.tasks`                                                                                                              |
| 4    | **Spec Review**   | Runs the review council in Spec Review Mode. Auto-fixes LOW/MEDIUM findings. Exits on HIGH/CRITICAL.                                                                               |
| 5    | **Implement**     | Parses `tasks.md` for phases. `[P]` parallel tasks run via Replicator worktrees (up to 4 concurrent workers). Phase checkpoints run CI commands derived from `.github/workflows/`. |
| 6    | **Code Review**   | Runs the review council in Code Review Mode. Includes Phase 1a CI hard gate, Phase 1b Gaze quality analysis, and Divisor agent reviews. Up to 3 fix iterations.                    |
| 7    | **Retrospective** | Analyzes the session and stores learnings in Hivemind semantic memory.                                                                                                             |
| 8    | **Demo**          | Presents structured demo instructions: what was built, how to verify, key files changed, and next steps.                                                                           |

### Key Capabilities

- **Dewey-powered clarification**: Auto-resolves spec ambiguities using semantic search. Falls back to human input when Dewey is unavailable or results are insufficient.
- **Parallel Replicator workers**: `[P]`-marked tasks execute in parallel via git worktrees (up to 4 concurrent). Falls back to sequential when Replicator is not installed.
- **Resumability**: Probes filesystem state on startup to detect completed steps. Resumes from the first incomplete step. All progress is persisted in spec artifacts (plan.md, tasks.md checkboxes, spec-review marker).
- **Graceful exit points**: Every exit (unanswerable questions, HIGH/CRITICAL findings, worker failures, merge conflicts, test failures, review exhaustion) includes actionable next steps and resume instructions.
- **CI command derivation**: Build and test commands are derived from `.github/workflows/` files, not hardcoded.

### Branch Safety

`/unleash` requires a Speckit feature branch (`NNN-*`). It never runs on `main` and rejects `opsx/*` branches (use `/opsx-apply` instead). It validates that `spec.md` exists before proceeding.

After `/unleash` completes, the demo step suggests running `/finale` to commit, push, create a PR, and merge.

See also: [From Spec to Demo in One Command](/blog/unleash-in-practice/) — a narrative walkthrough of the pipeline.

## End-of-Branch Workflow (`/finale`)

`/finale` automates the end-of-branch workflow — one command to stage, commit, push, create a PR, watch CI, merge, and return to main.

### The 9-Step Workflow

| Step | Name                        | Description                                                                         |
| ---- | --------------------------- | ----------------------------------------------------------------------------------- |
| 1    | **Branch Safety Gate**      | Verifies not on `main`. Notes the branch name.                                      |
| 2    | **Check for Changes**       | Inspects working tree. If clean, checks for unpushed commits or existing PR.        |
| 3    | **Generate Commit Message** | Analyzes staged changes, generates conventional commit message, shows for approval. |
| 4    | **Push to Remote**          | Sets upstream if needed (`git push -u origin <branch>`).                            |
| 5    | **Create or Find PR**       | Creates PR via `gh pr create` or finds existing one.                                |
| 6    | **Watch CI Checks**         | `gh pr checks --watch`. Stops on failure with options.                              |
| 7    | **Merge PR**                | `gh pr merge --rebase --delete-branch`. Always uses rebase merge strategy.          |
| 8    | **Return to Main**          | `git checkout main && git pull`.                                                    |
| 9    | **Summary**                 | Displays completion report: branch, commit, PR, checks, status.                     |

### Guardrails

- Never runs on `main`
- Never merges with failing CI checks
- Never stages secret files (`.env`, `credentials.json`, `*.key`, `*.pem`) without warning
- Never commits without user approval of the commit message
- Always uses rebase merge strategy (no squash or merge commits)
- If any step fails, stops immediately with context and options

`/finale` works with both Speckit (`NNN-*`) and OpenSpec (`opsx/*`) branches. It is the natural complement to `/unleash` — `/unleash` builds, `/finale` ships.

## New Feature (End-to-End) {#new-feature-end-to-end}

> For autonomous execution of this entire workflow in one command, use [`/unleash`](#autonomous-pipeline-unleash). The manual flow below gives you step-by-step control over each stage.

The full hero lifecycle for a new feature follows six stages. Each stage is owned by a specific hero, and each produces artifacts consumed by the next. Every stage has an **execution mode** -- either `[human]` (driven by the operator) or `[swarm]` (run autonomously by the agent swarm).

### 1. Define (Product Owner) `[swarm]`

The [Product Owner (Muti-Mind)](/docs/getting-started/product-owner/) creates a specification from a seed -- a short description of intent provided by the human.

The human seeds the feature with 1-2 sentences:

- Select or describe a backlog item: "Fix the authentication timeout bug" or "Add CSV export for the dashboard"
- Muti-Mind uses [Dewey](/docs/getting-started/knowledge/) to retrieve related context -- past specifications, GitHub issues from across the organization, toolstack documentation, and learning feedback from previous cycles
- Muti-Mind drafts the specification autonomously: retrieves context, writes the spec with acceptance criteria, self-clarifies using Dewey instead of asking the human, and validates against historical patterns
- The specification proceeds directly to implementation

**Output**: Backlog item + feature specification (generated by the swarm)

#### Manual Define Mode

For projects without [Dewey](/docs/getting-started/knowledge/) configured, the define stage runs in `[human]` mode. The human drives specification creation directly:

- Create or prioritize a backlog item using Muti-Mind's priority scoring (Business Value, Risk, Dependency Weight, Urgency, Effort)
- Initiate the specification: `/speckit.specify`
- Provide acceptance criteria in Given/When/Then format
- Clarify ambiguities: `/speckit.clarify`

After the human completes this stage, the swarm takes over automatically.

#### Optional: Specification Review

For high-stakes features or unfamiliar domains, teams can enable an optional specification review checkpoint between define and implement. When enabled, the workflow pauses after Muti-Mind drafts the specification, and the human scans it for intent alignment:

- Does the spec address what you actually wanted?
- Is the scope correct -- nothing critical missing, nothing out of scope included?
- Are the acceptance criteria reasonable?

This is a lightweight review -- not a full editing session. If the spec looks right, approve and the swarm proceeds to implementation. If it drifted from your intent, provide a correction and Muti-Mind revises.

The specification review checkpoint is off by default. Enable it per-workflow when the cost of intent drift outweighs the benefit of full autonomy.

### 2. Implement (Developer) `[swarm]`

The [Developer (Cobalt-Crush)](/docs/getting-started/developer/) creates the technical plan and implements the feature.

- Generate the implementation plan: `/speckit.plan`
- Generate tasks: `/speckit.tasks`
- Run cross-artifact analysis: `/speckit.analyze`
- Validate checklists: `/speckit.checklist`
- Execute implementation: `/speckit.implement` or `/cobalt-crush`
- For parallel work: use `/unleash` which handles parallel task execution automatically
- Mark each task `[x]` in tasks.md as it completes
- Run tests after each phase checkpoint

**Output**: plan.md + tasks.md + code + tests

### 3. Validate (Tester) `[swarm]`

[Gaze](/docs/getting-started/tester/) runs quality analysis on the implemented code.

- Analyze side effects: `gaze analyze --classify ./...`
- Assess test quality: `gaze quality ./...`
- Compute risk scores: `gaze crap ./...`
- Generate comprehensive report: `gaze report ./... --ai=opencode` (or `--ai=claude`)

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

The workflow is designed for **autonomous swarm delegation**. When [Dewey](/docs/getting-started/knowledge/) is configured, the swarm runs the entire lifecycle from define through review without human intervention. The workflow pauses automatically before the accept stage, returning control to the human for an acceptance decision. After acceptance, the swarm runs the final reflect stage autonomously.

With Dewey, a complete feature workflow requires only **one human decision point**:

1. **Seed**: Describe the intent in 1-2 sentences
2. **Accept**: Review the completed increment and accept or reject

Everything between seed and accept -- specification, planning, implementation, testing, and review -- is handled by the swarm, powered by Dewey's cross-repository semantic context.

#### Without Dewey

For projects without Dewey configured, the define stage runs in manual mode (`[human]`). This means the workflow requires **two human decision points**:

1. **Define**: Write the specification and clarify ambiguities
2. **Accept**: Review the increment and accept or reject

The swarm still runs implementation through review autonomously -- the only difference is who drives the specification.

### Workflow Commands

To seed a feature from a single description (autonomous define):

```text
/workflow seed "Add CSV export for the quality dashboard" [--spec-review]
```

This creates a backlog item and starts a workflow with `define=swarm` in one operation. Add `--spec-review` to enable the optional specification review checkpoint.

For explicit control over the define mode, use `/workflow start` with flags:

```text
/workflow start --define-mode=swarm --spec-review
```

| Flag            | Values           | Default | Description                                  |
| --------------- | ---------------- | ------- | -------------------------------------------- |
| `--define-mode` | `human`, `swarm` | `human` | Who drives specification creation            |
| `--spec-review` | _(flag)_         | off     | Pause for human review after spec is drafted |

### Workflow Configuration

Set project-level defaults in `.unbound-force/config.yaml` so you don't need to pass flags on every workflow start:

```yaml
workflow:
  execution_modes:
    define: swarm # or "human" (default)
  spec_review: false
```

Config values are the base defaults; CLI flags override them. For spec review, OR logic applies -- either the config or the CLI flag being true enables it.

This file is created by `uf init` with all values commented out (defaults apply). Edit it to set your team's preferred workflow behavior.

### Workflow Management

Once a workflow is running, use these commands to monitor progress and manage stage transitions.

#### `/workflow status`

Check the current workflow state:

```text
/workflow status [workflow-id]
```

If no workflow ID is provided, the command auto-detects the active workflow from your current git branch. The output shows the workflow ID, branch, backlog item, status, start time, and iteration count, followed by a stage-by-stage breakdown.

Each stage displays its execution mode (`[human]` or `[swarm]`), the assigned hero, elapsed time, and one of these status indicators:

| Indicator | Meaning                                       |
| --------- | --------------------------------------------- |
| `✓`       | Completed                                     |
| `◉`       | Active (currently running)                    |
| `○`       | Pending (not yet started)                     |
| `⏸`       | Awaiting human (paused at a human checkpoint) |
| `⊘`       | Skipped                                       |
| `✗`       | Failed                                        |

When a workflow is awaiting a human checkpoint, the status output shows the `⏸` indicator on the next pending human-mode stage and prompts: "Run `/workflow advance` to resume."

#### `/workflow list`

List all workflows:

```text
/workflow list [--status active|completed|all]
```

Displays a table with columns: ID, Branch, Status, and Started. Sorted by start time (most recent first). The default filter shows all workflows.

#### `/workflow advance`

Advance a workflow to the next stage or resume from a human checkpoint:

```text
/workflow advance [workflow-id]
```

The command handles several scenarios:

- **Normal advance**: Completes the current stage with timestamps and produced artifacts, then activates the next non-skipped stage.
- **Checkpoint**: If the completed stage is swarm-mode and the next stage is human-mode, the workflow pauses automatically (status changes to `awaiting_human`). This is how the workflow returns control to the human at the accept stage.
- **Resume**: If the workflow is in `awaiting_human` status, the command activates the next pending human-mode stage and sets the status back to `active`.
- **Escalation**: If the review stage has reached 3 iterations without full approval, the command escalates to human review rather than looping indefinitely.
- **Completion**: If no more stages remain, the workflow is marked `completed` and a `workflow-record` [artifact](/docs/getting-started/artifacts/) is generated with the full workflow trace.

### Knowledge Context

[Dewey](/docs/getting-started/knowledge/) is what makes autonomous define possible. By providing org-wide semantic context -- past specifications, GitHub issues from across the organization, toolstack documentation, and learning feedback -- Dewey gives Muti-Mind enough information to draft specifications without asking the human for context. This is the key capability that reduces human checkpoints from two to one.

Beyond the define stage, Dewey provides context to every hero throughout the lifecycle. When Cobalt-Crush implements a feature, Dewey provides toolstack API documentation and implementation patterns from other repositories. When Gaze validates code quality, Dewey offers cross-repo quality baselines and known failure modes. When the Divisor reviews code, Dewey enriches convention pack context with actual framework documentation.

This context is automatic -- heroes query Dewey's MCP tools as part of their normal workflow. No additional steps are required from the operator.

Dewey operates on a [3-tier graceful degradation](/docs/getting-started/knowledge/#graceful-degradation) model: full semantic search when Dewey and Ollama are available, structured graph queries when only the knowledge graph is indexed, and direct file reads when Dewey is not configured. Every hero functions at all three tiers -- Dewey enriches the workflow but never blocks it.

## Bug Fix (Tactical)

For bug fixes and small changes (fewer than 3 user stories), use the OpenSpec tactical workflow instead of the full Speckit pipeline.

### 1. Propose

Create a change with proposal, design, and task artifacts in one step:

```text
/opsx-propose fix-auth-timeout
```

This creates `openspec/changes/fix-auth-timeout/` with:

- `proposal.md` -- What and why
- `design.md` -- Technical approach
- `tasks.md` -- Implementation steps

This creates an `opsx/fix-auth-timeout` branch and checks it out automatically. The `opsx/` prefix distinguishes OpenSpec branches from Speckit branches (`NNN-<short-name>`) in `git branch` output.

### 2. Apply

Implement the tasks:

```text
/opsx-apply
```

Works through each task in the tasks.md file, making code changes and marking tasks complete. Before proceeding, `/opsx-apply` validates that you are on the correct `opsx/<name>` branch -- this is a hard gate to prevent implementing changes on the wrong branch.

### 3. Review

Run the review council to validate the fix:

```text
/review-council
```

The five Divisor personas review the changes. Address any REQUEST CHANGES findings.

### 4. Archive

After the fix is merged, archive the change:

```text
/opsx-archive
```

Moves the change to `openspec/changes/archive/` with a date prefix for historical reference, then returns to the `main` branch.

## Code Review

The review council brings five specialized perspectives to every code review.

### Invoking the Council

```text
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

### CI Gate (Phase 1a and 1b)

Before delegating to Divisor agents, the review council runs a two-phase CI gate:

**Phase 1a — CI Hard Gate**: Derives build, test, vet, lint, and vulnerability check commands from `.github/workflows/` files and executes them locally. Any non-zero exit code is a gate failure — the review stops before invoking Divisor agents. This ensures the code compiles, tests pass, and static analysis is clean before spending review tokens.

**Phase 1b — Conditional Gaze Quality Analysis**: If Gaze is installed, runs `gaze report` to generate CRAP scores, contract coverage, and quality findings. The results are passed as context to Divisor agents — the Testing persona uses Gaze data as evidence for coverage assessment. If Gaze is not installed, Phase 1b is skipped with an informational note.

### The Review Loop

1. Phase 1a CI gate must pass before reviews begin
2. All personas review in parallel and return APPROVE or REQUEST CHANGES
3. If any persona returns REQUEST CHANGES, the developer addresses the findings
4. All personas re-review after fixes
5. This loop repeats up to 3 iterations
6. After 3 iterations without full approval, the issue escalates to human review

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

This installs the full toolchain in one command:

- **Core tools** -- OpenCode (AI coding environment), Gaze (quality analysis), Mx F (manager hero), GitHub CLI
- **Development tools** -- Node.js, Bun, OpenSpec CLI, Replicator (multi-agent coordination), `.hive/` initialization
- **Knowledge layer** -- Ollama (local model runtime), Dewey (semantic search), IBM Granite embedding model, Dewey workspace initialization and index build
- **Project scaffolding** -- `uf init` to deploy agents, commands, convention packs, templates, and workflow configuration

Setup detects your version managers (goenv, nvm, fnm, Homebrew) and installs through them. Use `--dry-run` to preview what would be installed without making changes.

If you previously ran `uf setup` before v0.5.0, re-run it to pick up the new tools (Mx F, GitHub CLI, OpenSpec CLI, Ollama, and Dewey). Existing installations are detected and skipped.

After setup completes, add these environment variables to your shell profile (e.g., `~/.zshrc` or `~/.bashrc`) for embedding model alignment between the swarm and Dewey:

```bash
export OLLAMA_MODEL=granite-embedding:30m
export OLLAMA_EMBED_DIM=256
```

`uf setup` sets these during installation, but they must be in your shell profile for processes spawned outside of setup (e.g., `dewey serve`, manual `replicator init`).

Dewey is optional -- all heroes function without it. See the [knowledge retrieval guide](/docs/getting-started/knowledge/) for source configuration and OpenCode integration.

As the final step of setup, `uf init` scaffolds your project files and performs sub-tool initialization: it creates `.unbound-force/config.yaml` for [workflow configuration](#workflow-configuration), runs `dewey init` + `dewey index` when Dewey is available, and configures `opencode.json` with Dewey MCP server and Replicator MCP server entries when those tools are detected.

### 3. Verify

```bash
uf doctor
```

Doctor checks 7 areas and shows pass/warn/fail for each with install hints. Fix any failures by copying the suggested command from the output.

### 4. Start Working

Open OpenCode and start with the Speckit pipeline for a new feature:

```text
/speckit.specify
```

Then run the full autonomous pipeline:

```text
/unleash
```

## Next Steps

- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow, Speckit, Replicator, and Cobalt-Crush
- [Tester Guide](/docs/getting-started/tester/) -- Gaze quality analysis and CI integration
- [Product Owner Guide](/docs/getting-started/product-owner/) -- Muti-Mind and backlog management
- [Product Manager Guide](/docs/getting-started/product-manager/) -- Mx F metrics and coaching
