---
title: "Common Workflows"
description: "The /uf.unleash autonomous pipeline, /uf.finale shipping workflow, manual feature flows, bug fixes, code reviews, and environment setup."
lead: "End-to-end workflows that show how all five heroes collaborate across the development lifecycle."
date: 2026-03-22T00:00:00+00:00
draft: false
weight: 70
toc: true
---

## Autonomous Pipeline (`/uf.unleash`) {#autonomous-pipeline-unleash}

`/uf.unleash` is the autonomous Speckit pipeline execution command. It takes a spec from draft to demo-ready code in a single command, orchestrating the full pipeline with graceful exit points and full resumability.

### The Pipeline

| Step | Name              | Description                                                                                                                                                                        |
| ---- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | **Clarify**       | Scans spec.md for `[NEEDS CLARIFICATION]` markers. Uses Dewey semantic search to auto-resolve. Exits to human only for unanswerable questions.                                     |
| 2    | **Plan**          | Delegates to Cobalt-Crush to generate `plan.md` via `/speckit.plan`                                                                                                                |
| 3    | **Tasks**         | Delegates to Cobalt-Crush to generate `tasks.md` via `/speckit.tasks`                                                                                                              |
| 4    | **Spec Review**   | Runs the review council in Spec Review Mode. Auto-fixes LOW/MEDIUM findings. Exits on HIGH/CRITICAL.                                                                               |
| 5    | **Implement**     | Parses `tasks.md` for phases. `[P]` parallel tasks run via Replicator worktrees (up to 4 concurrent workers). Phase checkpoints run CI commands derived from `.github/workflows/`. |
| 6    | **Code Review**   | Runs the review council in Code Review Mode. Includes Phase 1a CI soft gate with causality analysis, Phase 1b Gaze quality analysis, and Divisor agent reviews. Up to 3 fix iterations. |
| 7    | **Retrospective** | Analyzes the session and stores learnings in Dewey semantic memory.                                                                                                                |
| 8    | **Demo**          | Presents structured demo instructions: what was built, how to verify, key files changed, and next steps.                                                                           |

### Key Capabilities

- **Dewey-powered clarification**: Auto-resolves spec ambiguities using semantic search. Falls back to human input when Dewey is unavailable or results are insufficient.
- **Parallel Replicator workers**: `[P]`-marked tasks execute in parallel via git worktrees (up to 4 concurrent). Falls back to sequential when Replicator is not installed.
- **Resumability**: Probes filesystem state on startup to detect completed steps. Resumes from the first incomplete step. All progress is persisted in spec artifacts (plan.md, tasks.md checkboxes, spec-review marker).
- **Graceful exit points**: Every exit (unanswerable questions, HIGH/CRITICAL findings, worker failures, merge conflicts, test failures, review exhaustion) includes actionable next steps and resume instructions.
- **CI command derivation**: Build and test commands are derived from `.github/workflows/` files, not hardcoded.

### Branch Safety

`/uf.unleash` works with both Speckit (`NNN-*`) and OpenSpec (`opsx/*`) feature branches. It never runs on `main`. For Speckit branches, it validates that `spec.md` exists. For OpenSpec branches, it detects the change name from the branch (`opsx/<name>`) and reads tasks from `openspec/changes/<name>/tasks.md`.

After `/uf.unleash` completes, the demo step suggests running `/uf.finale` to commit, push, and create a PR.

See also: [From Spec to Demo in One Command](/blog/unleash-in-practice/) — a narrative walkthrough of the pipeline.

## End-of-Branch Workflow (`/uf.finale`) {#end-of-branch-workflow-finale}

`/uf.finale` automates the end-of-branch workflow — one command to stage, commit, push, create a PR, watch CI, and return to main. The PR stays open for human review.

### The 8-Step Workflow

| Step | Name                        | Description                                                                         |
| ---- | --------------------------- | ----------------------------------------------------------------------------------- |
| 1    | **Branch Safety Gate**      | Verifies not on `main`. Notes the branch name.                                      |
| 2    | **Check for Changes**       | Inspects working tree. If clean, checks for unpushed commits or existing PR.        |
| 3    | **Generate Commit Message** | Analyzes staged changes, generates conventional commit message, shows for approval. Appends [AI attribution](#structured-pr-descriptions) to the commit. |
| 4    | **Push to Remote**          | Sets upstream if needed (`git push -u origin <branch>`).                            |
| 5    | **Create or Find PR**       | Creates PR via `gh pr create` with a [structured PR body](#structured-pr-descriptions), or finds an existing one. Respects [PR templates](#structured-pr-descriptions) when present. |
| 6    | **Watch CI Checks**         | `gh pr checks --watch`. Stops on failure with options.                              |
| 7    | **Return to Main**          | `git checkout main && git pull`.                                                    |
| 8    | **Summary**                 | Displays completion report: branch, commit, PR, checks, status.                     |

### Structured PR Descriptions

When `/finale` creates a PR, it generates a structured body with these sections:

- **Summary** — what the change does and why, derived from the branch's commit history and spec artifacts
- **How to Test** — concrete steps a reviewer can follow to verify the change
- **How to Demo** — how to demonstrate the feature to stakeholders
- **Key Files Changed** — the most important files with brief descriptions of what changed in each

Each section contains substantive content. For trivial changes, `/finale` writes brief notes rather than fabricating detail to fill the template.

If unresolved findings from `/review-council` exist, `/finale` adds a **Known Issues** section listing them. This section is omitted when no review was run or all findings were resolved.

The PR body ends with an attribution footer: `This PR was generated by /finale (AI-assisted).`

### PR Template Detection

If the repository contains a PR template (`.github/PULL_REQUEST_TEMPLATE.md`), `/finale` detects it and maps its generated sections to the template's headings using case-insensitive matching. When a template heading matches a generated section (e.g., a template heading containing "summary" maps to the Summary section), `/finale` fills in that section. Unmatched template sections are preserved as-is. If no template is found, `/finale` uses its default section format.

### AI Attribution

`/finale` adds AI attribution in two places:

- **Commit message** — an `Assisted-by: <model>` [git trailer](https://git-scm.com/docs/git-interpret-trailers) and a `Generated with AI assistance (<model>)` footer line are appended to the commit message. The model name is a cleaned version of the model identifier (provider prefixes, routing suffixes, and version digits are stripped). The user can edit or remove the attribution during the commit message approval step.
- **PR body** — an attribution footer appears as the last line of the PR description.

### Guardrails

- Never runs on `main`
- Never merges the PR — creates PRs for review, not for immediate merge
- Never stages secret files (`.env`, `credentials.json`, `*.key`, `*.pem`) without warning
- Never commits without user approval of the commit message
- Never creates a PR without user approval
- Uses `--body-file` instead of inline `--body` to safely handle AI-generated content containing shell metacharacters
- If any step fails, stops immediately with context and options

### Conflict Recovery

When the push step (step 4) fails because the remote branch has diverged, `/uf.finale` enters conflict recovery mode and presents five options:

| Option | Name                       | Description                                                                                      |
| ------ | -------------------------- | ------------------------------------------------------------------------------------------------ |
| 1      | **Retry**                  | Attempts the push again (useful if the conflict was transient).                                  |
| 2      | **Manual Resolution**      | Prints step-by-step instructions for resolving conflicts locally with `git pull --rebase`.       |
| 3      | **Abort**                  | Stops the workflow and returns to the shell without changes.                                     |
| 4      | **Force Push**             | Overwrites the remote branch (`git push --force-with-lease`). Use with caution.                  |
| 5      | **AI-Assisted Resolution** | Spawns a sub-agent to merge the target branch, identify conflicts, and resolve them with AI.     |

The AI-assisted option (5) spawns a `cobalt-crush-dev` sub-agent that merges the target branch into your feature branch, analyzes the intent of both sides using the diff context, resolves conflict markers programmatically, and creates a merge commit. After resolution, the normal push flow resumes. If the sub-agent cannot resolve the conflicts, `/uf.finale` falls back to manual resolution instructions.

`/uf.finale` works with both Speckit (`NNN-*`) and OpenSpec (`opsx/*`) branches. It is the natural complement to `/uf.unleash` — `/uf.unleash` builds, `/uf.finale` wraps up the branch and creates a PR for review.

## New Feature (End-to-End) {#new-feature-end-to-end}

> For autonomous execution of this entire workflow in one command, use [`/uf.unleash`](#autonomous-pipeline-unleash). The manual flow below gives you step-by-step control over each stage.

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
- Execute implementation: `/speckit.implement` or `/uf.cobalt-crush`
- For parallel work: use `/uf.unleash` which handles parallel task execution automatically
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

[The Divisor](/docs/team/the-divisor/) reviews the code through its specialized personas.

- Invoke the review council: `/uf.review-council`
- Review personas evaluate in parallel:
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

Set project-level defaults in `.uf/config.yaml` so you don't need to pass flags on every workflow start:

```yaml
workflow:
  execution_modes:
    define: swarm # or "human" (default)
  spec_review: false
```

Config values are the base defaults; CLI flags override them. For spec review, OR logic applies -- either the config or the CLI flag being true enables it.

This file is created by [`uf config init`](/docs/reference/config/#config-init) with all values commented out (defaults apply). Edit it to set your team's preferred workflow behavior. Run [`uf config validate`](/docs/reference/config/#config-validate) to check for invalid keys or type mismatches. See the [full configuration reference](/docs/reference/config/) for all 7 config sections and layered loading precedence.

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

## Issue Triage (`/uf.triage-issue`)

`/uf.triage-issue` evaluates a GitHub issue through 5 Divisor agents, each assessing the issue from a different perspective. The agents produce a consolidated triage recommendation with classification, severity, priority, and suggested labels.

```text
/uf.triage-issue 42
/uf.triage-issue owner/repo#42
```

### Triage Agents

| Agent          | Focus Area                                                        |
| -------------- | ----------------------------------------------------------------- |
| **Architect**  | Architectural impact, design implications, scope assessment       |
| **Adversary**  | Security risks, attack surface, resilience concerns               |
| **Guard**      | Intent alignment, scope discipline, duplicate detection           |
| **SRE**        | Operational impact, deployment risk, monitoring implications      |
| **Testing**    | Testability, regression risk, coverage gaps                       |

### Classification Categories

Each agent classifies the issue into one of 7 categories: **bug**, **feature**, **enhancement**, **question**, **opinion**, **duplicate**, or **needs-info**. The agents' assessments are aggregated into a final triage verdict with a consolidated severity, priority recommendation, and suggested labels.

### Human-Gated Label Mutations

Label changes are gated behind a confirmation prompt — the command never adds or removes labels on the issue automatically. After presenting the triage verdict, `/uf.triage-issue` asks for explicit approval before applying any label mutations to the GitHub issue.

## Swarm Coordination (`/forge`)

`/forge` orchestrates parallel task execution across isolated git worktrees. It decomposes a task into subtasks, spawns worker agents in separate worktrees, and coordinates their execution with file reservation to prevent conflicts. Use `/forge` when you have multiple independent tasks that can be implemented concurrently by separate agents.

```text
/forge "implement auth module and dashboard widget"
```

## Swarm Status (`/forge:status`)

`/forge:status` (a subcommand of `/forge`) checks the status of an active swarm execution. It reports progress for each spawned worker — including completion percentage, files touched, and any blockers — so you can monitor parallel work without switching between worktrees.

```text
/forge:status
```

## Work Item Management (`/org`)

`/org` queries and manages work items in the org database. It supports tasks, bugs, features, epics, and chores with priority scoring and dependency tracking. Use `/org` to list open items, filter by status or type, create new work items, or check what's ready to pick up next.

```text
/org                    # list open items
/org --ready            # show unblocked items
```

## Agent Inbox (`/inbox`)

`/inbox` checks the inter-agent communications inbox for messages from other agents in the swarm. Messages include progress updates, blockers, context broadcasts, and coordination signals. Use `/inbox` at session start to catch up on activity from parallel workers or previous sessions.

```text
/inbox
```

## Session Handoff (`/handoff`)

`/handoff` ends the current work session with structured handoff notes for the next session. It captures what was accomplished, what's in progress, any blockers encountered, and recommended next steps. The handoff notes are persisted so the next session can resume with full context.

```text
/handoff
```

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

### 2. Implement

Invoke Cobalt-Crush to implement with convention pack adherence:

```text
/uf.cobalt-crush
```

`/uf.cobalt-crush` detects the active OpenSpec change and implements the tasks through the `cobalt-crush-dev` agent, which loads [convention packs](/docs/getting-started/developer/#convention-packs) and applies the project's coding standards. This gives you the quality enforcement that a bare `/opsx-apply` would skip. Before proceeding, it validates that you are on the correct `opsx/<name>` branch.

### 3. Review

Run the review council to validate the fix:

```text
/uf.review-council
```

The Divisor personas review the changes. Address any REQUEST CHANGES findings.

### 4. Archive

After the fix is merged, archive the change:

```text
/opsx-archive
```

Moves the change to `openspec/changes/archive/` with a date prefix for historical reference, then returns to the `main` branch.

## Code Review

The review council brings multiple specialized perspectives to every code review. Each persona has [exclusive ownership boundaries](/docs/team/the-divisor/#exclusive-ownership-boundaries) — a finding like "missing error handling" is raised by exactly one persona, not duplicated across multiple reviewers.

### Invoking the Council

```text
/uf.review-council
/uf.review-council 42    # optionally specify a PR number
```

The council discovers available Divisor persona agents in `.opencode/agents/divisor-*.md` and launches all of them in parallel. When a PR number is provided, the council posts its consolidated findings as a GitHub PR review after the local review completes. Without a PR number, the review runs locally only. See the [Code Review Tutorial](/docs/getting-started/code-review-tutorial/#optional-post-to-github) for the full GitHub posting workflow.

### What Each Persona Evaluates

| Persona       | Focus Area                                                                               |
| ------------- | ---------------------------------------------------------------------------------------- |
| **Guard**     | Intent drift from the spec, constitution alignment, zero-waste mandate, scope discipline |
| **Architect** | Coding conventions per convention packs, architectural patterns, DRY, documentation      |
| **Adversary** | Security vulnerabilities, error handling, resilience, dependency risks                   |
| **Testing**   | Test architecture, coverage strategy, assertion depth, test isolation                    |
| **SRE**       | Release pipeline, dependency health, configuration, runtime observability                |
| **Curator**   | Documentation gaps, blog/tutorial opportunities, website issue filing                    |

> **Note**: The table above lists the 6 review personas that participate in code review. Three additional content personas — Scribe (technical documentation), Herald (blog/announcements), and Envoy (public communications) — are invoked separately for content creation tasks and do not participate in the code review council.

### CI Gate (Phase 1a and 1b)

Before delegating to Divisor agents, the review council runs a two-phase CI gate:

**Phase 1a — CI Soft Gate with Causality Analysis**: Derives build, test, vet, lint, and vulnerability check commands from `.github/workflows/` files and executes them locally. When a check fails, the council determines whether the failure is *new* (introduced by your branch) or *pre-existing* (already broken on `main`):

| Your Branch | `main` Baseline | Classification | Effect |
| ----------- | --------------- | -------------- | ------ |
| Fail | Pass | **New failure** | Blocks the review — fix before proceeding |
| Fail | Fail | **Pre-existing** | Informational only — does not block |
| Fail | No data | **Unknown** | Treated as new (conservative) |

To establish the `main` baseline, the council uses a two-tier strategy:

1. **GitHub CI API** — checks recent CI results for the `main` branch via the GitHub API. This is fast and requires no local work.
2. **Git worktree fallback** — if the API returns no data (no recent runs, no `gh` CLI, private repo restrictions), the council creates a temporary worktree of `main`, runs the same commands locally, and compares results. The worktree is cleaned up automatically.

Pre-existing failures appear in an informational section of the council report. They are visible but do not count toward the verdict. New failures (regressions your branch introduced) remain blocking — the review stops before invoking Divisor agents.

**Phase 1b — Conditional Gaze Quality Analysis**: If Gaze is installed, runs `gaze report` to generate CRAP scores, contract coverage, and quality findings. The results are passed as context to Divisor agents — the Testing persona uses Gaze data as evidence for coverage assessment. If Gaze is not installed, Phase 1b is skipped with an informational note.

### The Review Loop

1. Phase 1a CI gate must pass (only new failures block — pre-existing failures are informational)
2. All personas review in parallel and return APPROVE or REQUEST CHANGES
3. If any persona returns REQUEST CHANGES, the developer addresses the findings
4. All personas re-review after fixes
5. This loop repeats up to 3 iterations
6. After 3 iterations without full approval, the issue escalates to human review

### Verdict

The council returns **APPROVE** only when all active personas approve. A single REQUEST CHANGES means the council verdict is REQUEST CHANGES. When all personas approve but one or more include advisory findings (LOW-severity observations), the verdict is **APPROVE WITH ADVISORIES**. Missing personas (agent files not found) don't block the verdict but are noted in the report.

When posting to GitHub via `/uf.review-council N`, the council verdict maps to a GitHub review event: APPROVE maps to `APPROVE`, REQUEST CHANGES maps to `REQUEST_CHANGES`, and APPROVE WITH ADVISORIES maps to `COMMENT` (ensuring advisory findings are visible on the PR without blocking merge).

### GitHub Review Posting

After the council reaches a verdict, it can post the consolidated findings as a GitHub PR review. This bridges local review quality with the PR conversation on GitHub.

```text
/uf.review-council [PR-number]
```

The optional PR number argument targets a specific PR. If omitted, the council auto-detects the open PR for your current branch.

**How it works**:

1. The council completes its normal review (CI gate + Divisor personas)
2. If a PR is detected, the council offers to post findings as a GitHub review
3. All persona findings are aggregated into a single review with per-persona sections
4. The council asks for **human confirmation** before posting — it never posts automatically

**Verdict mapping**:

| Council Verdict | GitHub Review State | When |
| --------------- | ------------------- | ---- |
| APPROVE | `APPROVE` | All personas approve |
| REQUEST CHANGES | `REQUEST_CHANGES` | Any persona returns REQUEST CHANGES with HIGH/CRITICAL findings |
| Mixed (LOW/MEDIUM only) | `COMMENT` | Findings exist but none are HIGH or CRITICAL |

**Pre-posting checks**: Before offering to post, the council verifies the PR exists, the branch matches, and you have write permissions to the repository.

**Graceful degradation**: If the `gh` CLI is not installed, the PR is not found, or permissions are insufficient, the review proceeds normally without posting. The council report is still displayed locally — GitHub posting is an optional enhancement, not a requirement.

### Post-PR Review (`/uf.review-pr`)

`/uf.review-pr` reviews a pull request with full CI context — fetching check results, classifying failures by causality, and running a scoped diff review informed by what CI already validated.

```text
/uf.review-pr
```

This auto-detects the open PR for your current branch. To review a specific PR (including someone else's):

```text
/uf.review-pr 42
```

**What it does**:

1. Resolves the PR and fetches metadata (title, description, changed files)
2. Retrieves CI check results and classifies each failure as PR-caused or pre-existing (causality analysis)
3. Runs local tool checks only for what CI did not already cover
4. Fetches the scoped diff and performs an AI review with severity-classified findings
5. Posts the verdict as a GitHub PR review

**Verdict posting for all outcomes**: The verdict posting step runs for every review outcome — APPROVE, REQUEST_CHANGES, and COMMENT verdicts are all posted to the PR. Previously, verdict posting was skipped when a review had only MEDIUM/LOW findings or zero findings. Now, clean reviews receive an explicit APPROVE post on the PR, giving the author clear signal that the review passed.

**CI causality analysis**: When CI checks fail, `/uf.review-pr` determines whether your PR caused the failure or whether it was pre-existing on the base branch. Pre-existing failures are reported separately and do not block the verdict. See the [Code Review Tutorial](/docs/getting-started/code-review-tutorial/#step-3-post-pr-review-with-review-pr) for a full walkthrough with example output.

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
- **Development tools** -- Node.js, OpenSpec CLI, Replicator (multi-agent coordination)
- **Knowledge layer** -- Ollama (local model runtime), Dewey (semantic search), IBM Granite embedding model, Dewey workspace initialization and index build
- **Project scaffolding** -- `uf init` to deploy agents, commands, convention packs, templates, and workflow configuration

Setup detects your platform and version managers (goenv, nvm, fnm, Homebrew, dnf) and installs through them. On Fedora/RHEL without Homebrew, tools like Podman install via `dnf` and tools without native packages (Ollama, DevPod) fall back to their official curl installers with an interactive confirmation prompt. Use `--dry-run` to preview what would be installed without making changes.

If you previously ran `uf setup` before v0.5.0, re-run it to pick up the new tools (Mx F, GitHub CLI, OpenSpec CLI, Ollama, and Dewey). Existing installations are detected and skipped.

After setup completes, add these environment variables to your shell profile (e.g., `~/.zshrc` or `~/.bashrc`) for embedding model alignment between the swarm and Dewey:

```bash
export OLLAMA_MODEL=granite-embedding:30m
export OLLAMA_EMBED_DIM=256
```

`uf setup` sets these during installation, but they must be in your shell profile for processes spawned outside of setup (e.g., `dewey serve`, manual `replicator init`).

Dewey is optional -- all heroes function without it. See the [knowledge retrieval guide](/docs/getting-started/knowledge/) for source configuration and OpenCode integration.

As the final step of setup, `uf init` scaffolds your project files and performs sub-tool initialization: it creates `.uf/config.yaml` for [workflow configuration](#workflow-configuration), runs `dewey init` + `dewey index --no-embeddings` when Dewey is available (embedding generation is deferred for faster initialization — run `dewey index` separately afterward to generate embeddings for semantic search), initializes `.specify/` with Speckit configuration when `specify` is available, and configures `opencode.json` with Dewey MCP server and Replicator MCP server entries when those tools are detected.

### 3. Verify

```bash
uf doctor
```

Doctor checks 7 areas and shows pass/warn/fail for each with platform-appropriate install hints (e.g., `dnf install` on Fedora/RHEL, `brew install` on macOS). Fix any failures by copying the suggested command from the output.

### 4. Start Working

Open OpenCode and start with the Speckit pipeline for a new feature:

```text
/speckit.specify
```

Then run the full autonomous pipeline:

```text
/uf.unleash
```

## `uf setup` / `uf init` Lifecycle {#uf-setup-uf-init-lifecycle}

The [Environment Setup](#environment-setup) section above covers the quick path: install, setup, verify, start. This section explains what `uf setup` and `uf init` actually do under the hood -- useful when troubleshooting, re-initializing an existing project, or understanding how the toolchain is assembled.

### `uf setup`: Install Cascade

`uf setup` installs the [four tool categories](#2-run-setup) listed in the Environment Setup section above, in order. As a final step, `uf init` runs automatically to scaffold the project.

Setup detects your platform and adjusts the installation method accordingly:

- **macOS with Homebrew**: installs via `brew install` where packages are available
- **Fedora/RHEL with dnf**: installs via `dnf` for tools available in Fedora repos (e.g., Podman). Tools without native packages (Ollama, DevPod) fall back to their official curl installers with an interactive confirmation prompt
- **Other platforms**: falls back to curl-based installers or version manager installs (goenv, nvm, fnm)

Existing installations are detected and skipped. Use `--dry-run` to preview what would be installed without making changes.

#### RPM Version Resolution (Fedora/RHEL)

On dnf-based systems, `uf setup` resolves companion tool versions (Gaze, Replicator) independently. Rather than using the `uf` binary's own build-time version for all companion RPM URLs -- which could cause 404 errors when tool versions diverge -- each companion tool's latest release is resolved independently via `gh release view`. The resolved tag is validated for repository format, semver format, and length bounds.

This means `gh` CLI must be installed before Gaze and Replicator on dnf-based systems. `uf setup` handles this ordering automatically, but if you see RPM 404 errors during manual installation, verify that `gh` is installed and has network access.

### `uf init`: Project Scaffolding

`uf init` is the final step of `uf setup`. It can also be run independently to initialize or re-initialize a project. The init process performs a 12-step scaffolding sequence:

- Creates `.uf/config.yaml` for [workflow configuration](/docs/getting-started/common-workflows/#workflow-configuration)
- Deploys agents, commands, convention packs, and templates into the project
- Injects command-specific guardrails for Speckit and execution commands (4 variants: `speckit.implement`, `speckit.constitution`, `speckit.taskstoissues`, and general execution/utility)
- Injects STOP HERE blocks for 6 spec-phase Speckit commands to enforce phase boundaries
- Manages scaffold comment deduplication on re-runs to prevent duplicate injections
- Cleans up legacy `unbound/packs/` directories from older versions
- Performs sub-tool initialization (see below)

Init is idempotent -- running it multiple times on the same project is safe. Branch enforcement and guardrail injection are split into independent checks, so partial re-runs complete correctly.

### Sub-tool Initialization

After scaffolding, `uf init` initializes companion tools concurrently. Five sub-tools -- Dewey, Gaze, Replicator, Specify, and OpenSpec -- are initialized in parallel via independent goroutines, reducing wall-clock time from ~90-120 seconds (sequential) to ~30-60 seconds.

Sub-tools are divided into two groups:

| Group | Tools | Notes |
| ----- | ----- | ----- |
| **Group A** | Dewey | Handles indexing and embedding generation. Receives `--no-embeddings` during init to avoid blocking. |
| **Group B** | Specify, Replicator, OpenSpec, Gaze | Standard initialization with no special flags. |

Dewey indexing no longer blocks other tool initialization. When Dewey is available, `uf init` runs `dewey init` + `dewey index --no-embeddings`. For full embedding generation, run `dewey index` separately after init completes.

### Re-initialization (`--force`)

`uf init --force` re-initializes all sub-tools across both Group A and Group B. This is useful when a tool's configuration has become corrupted, or after upgrading `uf` to pick up new init steps.

Without `--force`, init only initializes tools that have not been initialized before. With `--force`:

- All sub-tools are re-initialized regardless of current state
- Tools that support `--force` receive the flag via their CLI
- The init summary shows "re-initialized" vs. "initialized" for each tool to distinguish fresh vs. forced initialization

Prior to v0.16.0, `--force` only re-initialized Group A (Dewey), silently skipping Group B tools. This was fixed so that `--force` now covers all sub-tools.

### Guardrail Injection

`uf init` injects command-specific guardrails into your project's agent configuration. There are four guardrail variants, each tailored to a different command category:

- **`speckit.implement`** -- guardrails for the implementation phase
- **`speckit.constitution`** -- guardrails for constitution management
- **`speckit.taskstoissues`** -- guardrails for task-to-issue conversion
- **Execution/utility** -- guardrails for general execution commands

On re-runs, `uf init` self-corrects: correctness markers in the injected content allow init to detect outdated guardrails and replace them with the current version. This means upgrading `uf` and running `uf init` automatically updates your project's guardrails without manual intervention.

### Stale Command Warnings

After the `uf.*` namespace migration, running `uf init` on an existing project scans agent markdown files for references to old-name commands (the pre-`uf.*` namespace). If stale references are found, init emits warnings with actionable output identifying which files contain outdated command names. Update the flagged files to use the current `uf.*` namespace.

### Troubleshooting

**Dewey indexing hangs on `uf init --force`**: The `--force` flag passes `--no-embeddings` to Dewey's index command, so Dewey should not trigger full embedding generation during init. If Dewey appears to hang, it may be performing the initial workspace indexing on a large repository (~2300+ pages). Wait for it to complete, or cancel and run `dewey index` separately with `--no-embeddings` to skip the embedding generation pass. For full embedding generation (required for semantic search), run `dewey index` independently after init completes -- this runs Ollama with the Granite embedding model and can take several minutes depending on workspace size.

**Stale command references after upgrade**: After upgrading `uf`, run `uf init` to trigger the stale command reference scan. If warnings appear, update the referenced files to use the `uf.*` namespace. The warning output identifies exact file paths and the old command names found.

**RPM 404 errors on Fedora/RHEL**: If `uf setup` fails to download companion tool RPMs, verify that `gh` CLI is installed and has network access. Each companion tool's version is resolved independently via `gh release view`, so a missing or unauthenticated `gh` CLI will cause resolution failures. Run `gh auth status` to check authentication, and `gh release view --repo unbound-force/<tool>` to test version resolution manually.

## Next Steps

- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow, Speckit, Replicator, and Cobalt-Crush
- [Tester Guide](/docs/getting-started/tester/) -- Gaze quality analysis and CI integration
- [Product Owner Guide](/docs/getting-started/product-owner/) -- Muti-Mind and backlog management
- [Product Manager Guide](/docs/getting-started/product-manager/) -- Mx F metrics and coaching
