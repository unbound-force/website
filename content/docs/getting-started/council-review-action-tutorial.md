---
title: "Council Review Action Tutorial"
description: "Step-by-step guide to adding AI-assisted code review to your GitHub repository using the council-review-action."
lead: "Add multi-perspective AI code review to your pull requests in under 10 minutes."
date: 2026-08-21T00:00:00+00:00
draft: false
weight: 75
toc: true
---

## Prerequisites

Before starting this tutorial, ensure:

1. **GitHub repository with pull request workflow** -- you have a repository where PRs are the standard review mechanism
2. **Repository admin access** -- you need permission to add workflow files and configure repository secrets
3. **An API key for your LLM provider** -- Anthropic (direct), Google Cloud Vertex AI, or AWS Bedrock
4. **`uf` CLI installed and initialized** -- run `uf init --divisor` to deploy the review persona files

```bash
uf init --divisor
```

Verify the personas were deployed:

```bash
ls .opencode/agents/divisor-*.md
```

You should see nine persona files: `divisor-adversary.md`, `divisor-architect.md`, `divisor-curator.md`, `divisor-envoy.md`, `divisor-guard.md`, `divisor-herald.md`, `divisor-scribe.md`, `divisor-sre.md`, and `divisor-testing.md`.

## Step 1: Deploy Review Personas

The review personas are the agents that evaluate your pull requests. Each persona is a Markdown file in `.opencode/agents/` with frontmatter that defines its review focus area, ownership boundaries, and out-of-scope topics.

Running `uf init --divisor` deploys nine persona files -- six review-focused and three content-focused:

**Review Personas**

| Persona | File | Focus |
|---------|------|-------|
| **Guard** | `divisor-guard.md` | Intent drift, constitution alignment, zero-waste compliance |
| **Architect** | `divisor-architect.md` | Structural integrity, coding conventions, pattern adherence |
| **Adversary** | `divisor-adversary.md` | Security, resilience, error handling, edge cases |
| **Testing** | `divisor-testing.md` | Test architecture, coverage strategy, assertion depth |
| **SRE** | `divisor-sre.md` | Deployment readiness, dependency health, observability |
| **Curator** | `divisor-curator.md` | Documentation gaps, blog/tutorial opportunities, website issue filing |

**Content Personas**

| Persona | File | Focus |
|---------|------|-------|
| **Scribe** | `divisor-scribe.md` | Technical documentation quality -- READMEs, specs, CLI help, API docs |
| **Herald** | `divisor-herald.md` | Blog and announcement opportunities -- release notes, feature write-ups |
| **Envoy** | `divisor-envoy.md` | Public communications -- messaging clarity, brand voice, community updates |

Each persona has exclusive ownership boundaries -- a finding like "missing error handling" is raised by exactly one persona (the Adversary), not duplicated across reviewers. The content personas participate in the review council but defer code-level findings to the review personas.

### What Happens

The `uf init --divisor` command creates the `.opencode/agents/` directory (if it does not exist) and writes the persona files. These are the same agents used by the local `/review-council` command. The council-review-action discovers them at runtime by scanning for files matching the `divisor-*.md` pattern.

You can customize the review scope by adding, removing, or editing persona files. The action picks up changes on the next PR.

> **Note**: The nine personas listed above are the default set shipped by `uf init --divisor`. However, the action discovers personas dynamically by scanning for `divisor-*.md` files at runtime. If your repository adds custom personas beyond the default nine, the action runs all of them automatically -- no configuration change is needed.

## Step 2: Add the Trigger Workflow

Create `.github/workflows/council-review-trigger.yml` in your repository. This workflow fires on pull request events and dispatches the review workflow:

```yaml
name: Council Review (Trigger)

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read

jobs:
  trigger-review:
    runs-on: ubuntu-latest
    if: github.event.pull_request.draft == false
    steps:
      - name: Dispatch review
        uses: actions/github-script@60a0d83039c74a4aee543508d2ffcb1c3799cdea # v7.0.1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            await github.rest.actions.createWorkflowDispatch({
              owner: context.repo.owner,
              repo: context.repo.repo,
              workflow_id: 'council-review.yml',
              ref: context.payload.pull_request.head.ref,
              inputs: {
                pr_number: String(context.payload.pull_request.number),
                head_sha: context.payload.pull_request.head.sha
              }
            });
```

### What Happens

When a pull request is opened, updated, or reopened, this workflow:

1. Checks whether the PR is a draft (skips drafts by default)
2. Dispatches the review workflow with the PR number and head SHA
3. Exits -- the actual review runs in a separate workflow for security isolation

> **Security**: Never hardcode API keys or tokens in workflow files. Always use GitHub repository secrets.

## Step 3: Add the Review Workflow

Create `.github/workflows/council-review.yml`. This is the core review engine that runs the Divisor personas against the PR diff:

```yaml
name: Council Review

on:
  workflow_dispatch:
    inputs:
      pr_number:
        description: "Pull request number"
        required: true
      head_sha:
        description: "Head commit SHA"
        required: true

permissions:
  contents: read

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout PR head
        uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
        with:
          ref: ${{ inputs.head_sha }}

      - name: Discover personas
        id: personas
        run: |
          personas=$(ls .opencode/agents/divisor-*.md 2>/dev/null | \
            sed 's|.*/divisor-||;s|\.md||' | tr '\n' ',' | sed 's/,$//')
          echo "found=$personas" >> "$GITHUB_OUTPUT"
          echo "Discovered personas: $personas"

      - name: Run review council
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          opencode run --agent divisor \
            --input "Review PR #${{ inputs.pr_number }}" \
            --headless

      - name: Upload review report
        uses: actions/upload-artifact@ea165f8d65b6db9a8b9f2a4d0fe1225601bd1e24 # v4.6.2
        with:
          name: review-report
          path: .uf/artifacts/review-verdict/
```

### What Happens

The review workflow:

1. Checks out the repository at the PR's head commit
2. Discovers available Divisor personas by scanning `.opencode/agents/divisor-*.md`
3. Runs each persona against the PR diff using headless `opencode run` invocations
4. Collects structured findings (severity, file, line, description) from each persona
5. Uploads the consolidated review report as a workflow artifact

The review workflow runs in a clean environment with access to the LLM provider (via secrets) but no write access to the repository. This enforces the separation between the doer and the judge -- review agents can read and analyze, but they cannot modify code.

## Step 4: Add the Comment Workflow

Create `.github/workflows/council-review-comment.yml`. This workflow posts review findings back to the PR as comments:

```yaml
name: Council Review (Comment)

on:
  workflow_run:
    workflows: ["Council Review"]
    types: [completed]

permissions:
  contents: read
  pull-requests: write

jobs:
  comment:
    runs-on: ubuntu-latest
    if: github.event.workflow_run.conclusion == 'success'
    steps:
      - name: Download review report
        uses: actions/download-artifact@d3f86a106a0bac45b974a628896c90dbdf5c8093 # v4.3.0
        with:
          name: review-report
          run-id: ${{ github.event.workflow_run.id }}
          github-token: ${{ secrets.GITHUB_TOKEN }}

      - name: Post review comments
        uses: actions/github-script@60a0d83039c74a4aee543508d2ffcb1c3799cdea # v7.0.1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const fs = require('fs');
            const report = JSON.parse(
              fs.readFileSync('review-verdict/report.json', 'utf8')
            );

            let body = `## Council Review Results\n\n`;
            body += `**Verdict**: ${report.verdict}\n\n`;

            for (const persona of report.personas) {
              body += `### ${persona.name}\n`;
              body += `**Status**: ${persona.status}\n\n`;
              for (const finding of persona.findings) {
                body += `- [${finding.severity}] ${finding.description}`;
                if (finding.file) {
                  body += ` (${finding.file}:${finding.line})`;
                }
                body += `\n`;
              }
              body += `\n`;
            }

            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: report.pr_number,
              body: body
            });
```

### What Happens

The comment workflow:

1. Triggers when the review workflow completes successfully
2. Downloads the review report artifact from the review workflow run
3. Parses the structured findings and formats them for the GitHub PR interface
4. Posts a summary comment with the overall verdict and per-persona findings

The three-workflow separation exists for security reasons. The `pull_request` event trigger runs with limited permissions (it cannot access secrets from forks). By chaining through `workflow_run`, the review and comment workflows run with the permissions needed to access LLM credentials and post PR comments.

## Step 5: Configure Repository Secrets

The review workflow needs API credentials for the LLM provider. Configure these in your repository settings under **Settings > Secrets and variables > Actions**.

### Anthropic (Direct)

Set one secret:

| Secret | Value |
|--------|-------|
| `ANTHROPIC_API_KEY` | Your Anthropic API key |

The workflow references it as `${{ secrets.ANTHROPIC_API_KEY }}`.

### Google Cloud Vertex AI

Set two secrets:

| Secret | Value |
|--------|-------|
| `VERTEX_PROJECT_ID` | Your Google Cloud project ID |
| `VERTEX_REGION` | The Cloud region (e.g., `us-central1`) |

The workflow also needs the `CLAUDE_CODE_USE_VERTEX` environment variable set to `"1"`. Add this to the review workflow's `env` block:

```yaml
env:
  CLAUDE_CODE_USE_VERTEX: "1"
  ANTHROPIC_VERTEX_PROJECT_ID: ${{ secrets.VERTEX_PROJECT_ID }}
  CLOUD_ML_REGION: ${{ secrets.VERTEX_REGION }}
```

### Google Cloud Vertex AI with Workload Identity Federation (Recommended)

For organization members and production deployments, [Workload Identity Federation (WIF)](https://cloud.google.com/iam/docs/workload-identity-federation) is the recommended authentication method. WIF eliminates static credentials entirely -- GitHub Actions authenticates directly with Google Cloud using OIDC tokens.

Set up WIF authentication by adding the `google-github-actions/auth` step before the review step:

```yaml
      - name: Authenticate to Google Cloud
        uses: google-github-actions/auth@ba79af03959ebeac9769e648f473a284504d9193 # v2.1.10
        with:
          workload_identity_provider: ${{ secrets.WIF_PROVIDER }}
          service_account: ${{ secrets.WIF_SERVICE_ACCOUNT }}

      - name: Run review council
        env:
          CLAUDE_CODE_USE_VERTEX: "1"
          ANTHROPIC_VERTEX_PROJECT_ID: ${{ secrets.VERTEX_PROJECT_ID }}
          CLOUD_ML_REGION: ${{ secrets.VERTEX_REGION }}
        run: |
          opencode run --agent divisor \
            --input "Review PR #${{ inputs.pr_number }}" \
            --headless
```

| Secret | Value |
|--------|-------|
| `WIF_PROVIDER` | Your Workload Identity Provider resource name |
| `WIF_SERVICE_ACCOUNT` | The service account email to impersonate |
| `VERTEX_PROJECT_ID` | Your Google Cloud project ID |
| `VERTEX_REGION` | The Cloud region (e.g., `us-central1`) |

WIF requires a one-time setup in Google Cloud (creating a Workload Identity Pool and Provider), but once configured, no static keys need to be stored as GitHub secrets. The static secrets approach shown in the Vertex AI section above remains available as a fallback for external contributors or BYOK (bring your own key) setups.

### AWS Bedrock

Set three secrets:

| Secret | Value |
|--------|-------|
| `AWS_ACCESS_KEY_ID` | Your AWS access key ID |
| `AWS_SECRET_ACCESS_KEY` | Your AWS secret access key |
| `AWS_REGION` | The AWS region (e.g., `us-east-1`) |

The workflow also needs the `CLAUDE_CODE_USE_BEDROCK` environment variable set to `"1"`. Add this to the review workflow's `env` block:

```yaml
env:
  CLAUDE_CODE_USE_BEDROCK: "1"
  AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
  AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
  AWS_REGION: ${{ secrets.AWS_REGION }}
```

### Security Best Practices

- **Scope secrets narrowly**: Use repository-level secrets, not organization-level secrets, unless multiple repositories share the same review configuration
- **Follow least privilege**: Grant only the permissions the action requires -- typically `pull-requests: write` and `contents: read`
- **Rotate keys regularly**: Treat LLM API keys like any other credential and rotate them on a schedule
- **Audit access**: Review which workflows consume each secret in your repository's Actions settings

## Step 6: Verify the Setup

Create a test pull request to verify the action is working:

```bash
git checkout -b test/council-review-setup
echo "# Test" > test-review.md
git add test-review.md
git commit -m "test: verify council review action"
git push -u origin test/council-review-setup
gh pr create --title "test: verify council review action" --body "Testing the council-review-action setup."
```

### What Happens

1. The trigger workflow detects the new PR and dispatches the review workflow
2. The review workflow discovers the Divisor personas and runs them against the diff
3. The comment workflow posts findings as a PR comment

### Expected Output

In the Actions tab, you should see three workflow runs. The review workflow logs show:

```text
Review triggered on PR #1
Discovered 9 review personas:
  divisor-adversary, divisor-architect, divisor-curator,
  divisor-envoy, divisor-guard, divisor-herald,
  divisor-scribe, divisor-sre, divisor-testing

Review findings posted as PR comments.
```

The PR receives a comment with the council's verdict and any findings. After verifying, close the test PR and delete the branch:

```bash
gh pr close --delete-branch
git checkout main
git branch -D test/council-review-setup
```

## Troubleshooting

### Missing Secrets

**Symptom**: The review workflow fails with an authentication error.

**Fix**: Verify that the required secrets are configured in **Settings > Secrets and variables > Actions**. The secret names must match what the workflow references (e.g., `ANTHROPIC_API_KEY`).

### Incorrect Permissions

**Symptom**: The comment workflow fails with a 403 error when posting PR comments.

**Fix**: Ensure the comment workflow has `pull-requests: write` permission. Check both the workflow-level `permissions` block and the repository's default token permissions under **Settings > Actions > General > Workflow permissions**.

### Workflow Trigger Not Firing

**Symptom**: No workflow runs appear in the Actions tab after creating a PR.

**Fix**: Verify the trigger workflow's `on:` block matches your PR events. Common issues:

- The workflow file is not on the default branch (workflows must be merged to `main` before they trigger)
- The PR is a draft and the workflow skips drafts (`github.event.pull_request.draft == false`)
- GitHub Actions is disabled for the repository

### No Personas Discovered

**Symptom**: The review workflow runs but finds zero personas.

**Fix**: Run `uf init --divisor` to deploy the persona files. Verify they exist:

```bash
ls .opencode/agents/divisor-*.md
```

The files must be committed and pushed to the branch the PR is based on.

### Review Takes Too Long

**Symptom**: The review workflow times out or runs for an extended period.

**Fix**: Large PRs with many changed files take longer to review. Consider:

- Breaking large PRs into smaller, focused changes
- Setting a workflow timeout with `timeout-minutes` in the job definition
- Reviewing the LLM provider's rate limits for your API key tier

## How It Fits Together

The council-review-action uses a three-workflow chain to separate concerns:

```text
PR opened/updated
      |
      v
Trigger workflow (detects event, dispatches review)
      |
      v
Review workflow (runs 9 Divisor personas)
      |
      v
Comment workflow (posts findings to PR)
```

Each workflow handles one responsibility. The trigger detects events, the review runs the AI agents, and the comment posts results. This separation provides security isolation -- the trigger workflow runs with minimal permissions, while the review workflow has access to LLM credentials, and the comment workflow has write access to post PR comments.

The council-review-action adds AI-assisted review context to your pull requests. Human reviewers still make the final decisions -- the action gives them additional perspective from specialized review personas that evaluate security, architecture, testing, operations, and documentation in parallel.

## See Also

- [Code Review Tutorial](/docs/getting-started/code-review-tutorial/) -- interactive review workflow with `/review-council` and `/review-pr`
- [Council Review Action Reference](/docs/reference/council-review-action/) -- full configuration options and architecture details
- [Convention Packs](/docs/reference/convention-packs/) -- the coding standards the review personas enforce
