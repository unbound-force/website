---
title: "Council Review Action"
description: "Multi-perspective AI code review as a GitHub Action — three-workflow chain, persona discovery, and configuration."
lead: "The council-review-action brings multi-perspective AI code review to any GitHub repository through a composite GitHub Action. Specialized reviewers evaluate security, architecture, testing, operations, and documentation in parallel."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 40
toc: true
---

## What the Council Review Action Does

The council-review-action is a composite GitHub Action that adds multi-perspective AI code review to pull requests. It runs the same Divisor review council used locally by `/review-council`, but in a CI environment where reviews happen automatically on every PR.

Nine Divisor personas are discovered and invoked in parallel -- six focused on code review and three on content. Each evaluates through a different lens:

**Review Personas**

| Persona | Focus |
|---------|-------|
| **Guard** | Intent drift detection, constitution alignment, zero-waste compliance |
| **Architect** | Structural integrity, coding conventions, pattern adherence, DRY |
| **Adversary** | Security, resilience, error handling, edge cases |
| **Testing** | Test architecture, coverage strategy, assertion depth, isolation |
| **SRE** | Deployment readiness, dependency health, observability, operational concerns |
| **Curator** | Documentation gaps, blog/tutorial opportunities, website issue filing |

**Content Personas**

| Persona | Focus |
|---------|-------|
| **Scribe** | Technical documentation quality -- READMEs, specs, CLI help, API docs |
| **Herald** | Blog and announcement opportunities -- release notes, feature write-ups |
| **Envoy** | Public communications -- messaging clarity, brand voice, community updates |

Each persona has exclusive ownership boundaries -- a finding like "missing error handling" is raised by exactly one persona (the Adversary), not duplicated across multiple reviewers. This prevents overlap and ensures each finding comes from the agent with the deepest domain expertise. The content personas participate in the review council but defer code-level findings to the review personas -- their focus is documentation completeness, content opportunities, and communication quality.

The action assists human reviewers by surfacing issues that are easy to miss in manual review -- security edge cases, architectural drift, test quality gaps, and operational concerns. It does not replace human review. The structural differentiator is the multi-persona architecture: rather than a single AI reviewer producing a monolithic report, independent agents evaluate the same diff through different quality dimensions, producing findings that are scoped, non-overlapping, and actionable.

## Three-Workflow Chain Architecture

The council-review-action uses a three-workflow chain to separate concerns and work within GitHub Actions' security model. Each workflow handles one stage of the review pipeline:

```text
PR opened/updated --> Trigger --> Review (9 personas) --> Comment --> PR comments posted
```

### 1. Trigger Workflow

The trigger workflow fires on `pull_request` events. It determines whether a review should run based on configurable criteria:

- Skip draft PRs (configurable)
- Filter by label (e.g., only review PRs labeled `review-ready`)
- Skip PRs from bots or automated processes

When a review is warranted, the trigger workflow dispatches the review workflow with the PR context (number, head SHA, base branch).

### 2. Review Workflow

The review workflow is the core review engine. It:

1. Clones the repository at the PR's head commit
2. Discovers available Divisor personas from `.opencode/agents/divisor-*.md`
3. Runs each persona against the PR diff using headless `opencode run` invocations
4. Collects structured findings from each persona (severity, file, line, description)
5. Produces a consolidated review report

The review workflow runs in a clean environment with access to the LLM provider (via secrets) but no write access to the repository. This enforces the separation between the doer and the judge -- review agents can read and analyze, but they structurally cannot modify code.

### 3. Comment Workflow

The comment workflow receives the review report and posts findings as GitHub PR comments:

- Groups findings by persona with severity indicators
- Adds inline code annotations for file-specific findings where applicable
- Posts a summary with the overall review verdict (APPROVE or REQUEST CHANGES)
- Formats output for readability in the GitHub PR interface

The three-workflow separation exists for security reasons. The `pull_request` event trigger runs with limited permissions (it cannot access secrets from forks). By chaining through a `workflow_run` or `repository_dispatch`, the review and comment workflows run with the permissions needed to access LLM credentials and post PR comments.

## Persona Discovery

The action discovers review personas dynamically at runtime rather than hardcoding a fixed set. This means adding or removing a persona is a file operation, not a code change.

Discovery works as follows:

1. The review workflow scans the `.opencode/agents/` directory in the repository
2. It matches files with the `divisor-*.md` naming pattern
3. Each matching file is treated as a review persona
4. The persona's frontmatter defines its review focus area, ownership boundaries, and out-of-scope topics

The default personas ship with `uf init --divisor`: nine agents total (Guard, Architect, Adversary, SRE, Testing, Curator, Scribe, Herald, and Envoy). Because discovery is file-based, repositories with custom personas beyond the default nine will have all of them invoked automatically -- no configuration change is needed. Repositories can customize the review scope:

- **Add a persona**: Create a new `divisor-<name>.md` file in `.opencode/agents/` with the appropriate frontmatter. The action picks it up on the next PR.
- **Remove a persona**: Delete the corresponding `divisor-<name>.md` file. The action stops running that persona.
- **Modify a persona**: Edit the persona's frontmatter to change its focus area or ownership boundaries.

This is the same discovery mechanism used by the local `/review-council` command, ensuring consistency between local and CI reviews.

## Configuration

### Required Inputs

The action accepts inputs through the workflow YAML configuration:

| Input | Required | Description |
|-------|----------|-------------|
| `model` | No | LLM model to use for review (defaults to the model configured in `opencode.json`) |
| `pr_number` | Yes | Pull request number to review (typically `${{ github.event.pull_request.number }}`) |
| `head_sha` | Yes | Head commit SHA of the PR (typically `${{ github.event.pull_request.head.sha }}`) |

### Secrets

The action requires API credentials for the LLM provider that powers the review agents. Configure these as repository secrets in your GitHub repository settings.

For **Anthropic Direct**:

```yaml
env:
  ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

For **Google Cloud Vertex AI** (using Workload Identity Federation):

```yaml
env:
  CLAUDE_CODE_USE_VERTEX: "1"
  ANTHROPIC_VERTEX_PROJECT_ID: ${{ secrets.VERTEX_PROJECT_ID }}
  CLOUD_ML_REGION: ${{ secrets.VERTEX_REGION }}
```

For **AWS Bedrock**:

```yaml
env:
  CLAUDE_CODE_USE_BEDROCK: "1"
  AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
  AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
  AWS_REGION: ${{ secrets.AWS_REGION }}
```

> **Security Note**: Secrets should be scoped to the narrowest context needed. Use repository-level secrets, not organization-level secrets, unless multiple repositories share the same review configuration. Workflow permissions should follow the principle of least privilege -- grant only the permissions the action requires (typically `pull-requests: write` and `contents: read`).

### Workflow Permissions

The workflows in the chain require different permission levels:

| Workflow | Permissions Needed |
|----------|-------------------|
| Trigger | `contents: read` |
| Review | `contents: read` (plus LLM provider secrets) |
| Comment | `pull-requests: write`, `contents: read` |

Set permissions explicitly in each workflow file rather than relying on the repository's default token permissions:

```yaml
permissions:
  contents: read
  pull-requests: write
```

### Environment Variables

Beyond the LLM provider credentials, the action respects these environment variables:

| Variable | Description |
|----------|-------------|
| `OPENCODE_MODEL` | Override the default model for all review agents |
| `REVIEW_SKIP_DRAFTS` | Set to `true` to skip draft PRs (default: `true`) |

## Integration with the Review Lifecycle

The council-review-action is the CI counterpart to the local `/review-council` command. Together with `/review-pr`, they form a complete review lifecycle:

| Stage | Tool | When It Runs | What It Does |
|-------|------|-------------|--------------|
| Pre-PR (local) | `/review-council` | Before pushing | Multi-persona review of local changes with CI gate |
| Post-PR (CI) | council-review-action | After PR is created | Automated multi-persona review in CI |
| Post-PR (local) | `/review-pr` | After PR is created | Single-agent review with CI causality analysis |

The typical workflow:

1. **Develop locally** -- write code, run tests
2. **Run `/review-council`** -- catch issues before pushing. The council runs a CI gate first, then launches the Divisor personas. Address any REQUEST CHANGES findings.
3. **Push and create PR** -- the council-review-action fires automatically in CI, providing a second review pass
4. **Optionally run `/review-pr`** -- for CI causality analysis (classifying failures as PR-caused vs. pre-existing) and fix-branch offers for pre-existing issues

The local `/review-council` and the CI council-review-action use the same persona discovery mechanism and the same Divisor agents. The difference is context: the local command has access to your working tree and can run the CI gate against local changes, while the CI action reviews the PR diff in a clean environment.

See the [Code Review Tutorial](/docs/getting-started/code-review-tutorial/) for a step-by-step walkthrough of the complete review lifecycle.

## Adopting the Action

Setting up the council-review-action involves adding three workflow files to your repository's `.github/workflows/` directory -- one for each stage of the chain (trigger, review, comment). The action also requires Divisor persona files in `.opencode/agents/`, which `uf init` deploys automatically.

For step-by-step setup instructions, see the [Council Review Action Tutorial](/docs/getting-started/council-review-action-tutorial/).

## See Also

- [Council Review Action Tutorial](/docs/getting-started/council-review-action-tutorial/) -- step-by-step setup guide for adding the action to your repository
- [Code Review Tutorial](/docs/getting-started/code-review-tutorial/) -- local review workflow with `/review-council` and `/review-pr`
- [Convention Packs](/docs/reference/convention-packs/) -- the coding standards that review personas enforce
