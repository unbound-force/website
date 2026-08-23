---
title: "AI Code Review in CI — How council-review-action Brings the Divisor Council to GitHub Actions"
description: "The Divisor Council's multi-persona AI code review now runs as a composite GitHub Action. Fork-safe, auto-discovering personas, and producing structured inline comments on every PR."
lead: "Five AI reviewers, one GitHub Action. council-review-action brings the Divisor Council's multi-persona code review to your CI pipeline — fork-safe, structured, and discoverable."
slug: "council-review-action"
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 107
toc: true
categories: ["Engineering"]
tags: ["divisor", "code-review", "github-actions", "CI", "automation"]
contributors: ["Unbound Force"]
---

## The Problem with AI Code Review in CI

Most AI code review integrations follow the same pattern: drop an API key into your workflow, pipe the diff to a model, and post whatever comes back as a PR comment. The result is a wall of unstructured text that developers scroll past. The review lacks perspective — a single model producing a single opinion, with no specialization and no structure.

The Divisor Council solves this locally. Five AI personas — The Guard, The Architect, The Adversary, The SRE, and The Testing Specialist — each review code through a different lens. But running the council required a local setup with OpenCode and the right agent configurations. That meant the review happened on the developer's machine, not in CI where every PR gets consistent coverage.

`council-review-action` bridges that gap. It packages the Divisor Council as a composite GitHub Action that runs on every pull request, producing structured inline comments tied to specific lines of code.

## Fork-Safe by Design: The Three-Workflow Chain

GitHub Actions restricts secret access for pull requests from forks — a security measure that breaks most AI review integrations. Fork PRs cannot access repository secrets, so any workflow that calls an LLM API fails silently or crashes loudly.

`council-review-action` uses a three-workflow chain pattern to work around this constraint without compromising security. The first workflow, `pull_request`, runs in the fork's context with no secret access. It performs the diff extraction, context gathering, and artifact preparation — all operations that require no credentials. The second workflow, `workflow_run`, triggers after the first completes and runs in the base repository's context where secrets are available. This workflow picks up the prepared artifacts and executes the LLM calls. The third step posts the review comments back to the PR using the base repository's `GITHUB_TOKEN`.

This separation means fork contributors get the same review quality as maintainers. No secrets leak to fork contexts. No special configuration is needed for external contributors. The review runs automatically on every PR regardless of its origin.

## Auto-Discovery: Your Personas or Ours

The action discovers Divisor personas by scanning the repository's `.opencode/agents/` directory for agent configuration files. If your project defines custom personas — perhaps a Database Specialist for a data-heavy application or a Compliance Reviewer for regulated industries — the action picks them up automatically. No configuration flags, no manifest files, no explicit persona lists.

When no custom personas exist, the action falls back to its bundled defaults: the five standard Divisor Council members. Each persona carries its own review focus, severity calibration, and output format expectations. The Guard checks for security vulnerabilities and credential exposure. The Architect evaluates structural decisions and dependency flow. The Adversary stress-tests edge cases and failure modes. The SRE examines operational concerns — logging, monitoring, graceful degradation. The Testing Specialist assesses coverage gaps and test quality.

This auto-discovery pattern means teams adopt the action with zero configuration and customize it by adding agent files they already know how to write.

## The Diff Annotation Pipeline

Raw diffs are noisy. They contain file mode changes, lock file updates, and generated code that no reviewer — human or AI — should spend tokens on. Feeding an unprocessed diff to an LLM wastes context window and produces findings against code that does not matter.

The diff annotation pipeline applies three transformations before any persona sees the code. First, noise filtering removes binary files, lock files, generated artifacts, and files matching configurable ignore patterns. Second, line numbering maps every diff hunk to its exact file and line position in the PR, enabling precise inline comment placement. Third, the output is reformatted into an LLM-friendly structure that preserves enough surrounding context for each change without exhausting the model's context window.

The result is a focused, annotated diff where every line carries its file path and position. When a persona identifies an issue, it references the exact line — and the action maps that reference back to the GitHub PR's diff view for inline comment placement.

## Pre-Fetched PR Context

Code does not exist in isolation. A PR that introduces a new dependency matters differently if CI is already failing. A refactoring PR reads differently when linked issues explain the motivation. A change that duplicates an existing review comment wastes everyone's time.

Before invoking any persona, the action pre-fetches three categories of context. CI check status tells personas whether the build is green, which tests are failing, and whether linting has already flagged issues. Existing review comments prevent personas from duplicating feedback that human reviewers or previous action runs have already posted. Linked issues and PR description provide the "why" behind the change, helping personas distinguish intentional trade-offs from oversights.

This context is injected into each persona's prompt alongside the annotated diff. The personas review code with the same situational awareness a human reviewer would have after reading the PR description, checking CI, and scanning existing comments.

## Structured Output: JSON with Line Validation

Unstructured LLM output is the root cause of low-quality AI reviews. When a model returns free-form text, the integration has no reliable way to extract file paths, line numbers, severity levels, or actionable suggestions. The result is a single monolithic comment that developers ignore.

Each persona in `council-review-action` produces structured JSON output. Every finding includes a file path, line number, severity level (CRITICAL, HIGH, MEDIUM, LOW), the persona that raised it, a description of the issue, and a suggested fix when applicable. The action validates each finding's line number against the actual diff — if a persona references a line that does not exist in the changed code, the finding is dropped rather than posted as a confusing orphaned comment.

Valid findings are posted as inline PR comments at the exact line of code they reference. Developers see each finding in context, right next to the code it concerns, with clear attribution to the persona that raised it. The structured format also enables downstream tooling: filtering by severity, tracking finding trends across PRs, and measuring persona accuracy over time.

## Security: Defense in Depth

Running LLM-based review on untrusted code — especially from fork PRs — introduces prompt injection risk. A malicious contributor could craft code comments, variable names, or documentation strings designed to manipulate the reviewing model into approving vulnerable code or leaking secrets.

`council-review-action` maintains a security risk register that catalogs known attack vectors and their mitigations. The three-workflow chain is the first layer: fork code never executes in a context with secret access. The diff annotation pipeline is the second layer: by controlling what the model sees, the action limits the attack surface to the diff content itself. Persona prompts include injection-resistant framing that instructs the model to treat all diff content as untrusted input, not as instructions.

The risk register is a living document. Each identified risk carries a severity rating, a description of the attack vector, the current mitigation, and the residual risk after mitigation. This transparency lets adopters make informed decisions about their threat model rather than trusting a black-box claim of "secure by default."

## ADR-001: Why OpenCode over Claude Code CLI

The action needed a runtime to orchestrate LLM calls with agent configurations, tool access, and structured output parsing. Two candidates emerged: Claude Code CLI (Anthropic's official tool) and OpenCode (the open-source AI coding assistant).

Claude Code CLI offers tight integration with Anthropic's models but locks the action to a single provider. It requires a specific authentication flow that complicates the three-workflow chain pattern. Its agent configuration format differs from the `.opencode/agents/` convention that Unbound Force projects already use, meaning persona auto-discovery would require a translation layer.

OpenCode supports multiple LLM providers, uses the same agent configuration format the Divisor Council already defines, and runs as a straightforward CLI that fits naturally into composite action steps. The decision — documented as ADR-001 in the action's `docs/decisions.md` — chose OpenCode for provider flexibility, configuration compatibility, and alignment with the existing Unbound Force toolchain. Teams that already use OpenCode locally get the same agent behavior in CI without any configuration drift.

## What This Means for Your Workflow

`council-review-action` turns the Divisor Council from a local developer tool into a CI-native review gate. Every PR gets reviewed by five specialized personas. Every finding lands as an inline comment at the exact line of code. Fork contributors get the same review quality as maintainers. Custom personas are discovered automatically from your existing agent configurations.

The action is available in the `unbound-force/unbound-force` repository under `council-review-action/`. Add it to your workflow, configure your LLM provider credentials as repository secrets, and the council convenes on every pull request.

To get started, check out the [council-review-action README](https://github.com/unbound-force/unbound-force/tree/main/council-review-action) for installation instructions, configuration options, and examples of the three-workflow chain pattern in practice. If you are already using the Divisor Council locally, your personas will carry over to CI with no additional setup.
