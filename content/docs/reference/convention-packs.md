---
title: "Convention Packs"
description: "Portable, severity-classified coding standards for the Unbound Force toolchain — pack types, severity levels, loading mechanism, and CI workflow rules."
lead: "Convention packs are the coding standards layer of the Unbound Force governance hierarchy. They guide agents before code is written and enforce rules during review."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 30
toc: true
---

## What Convention Packs Are

Convention packs are portable, severity-classified coding standards that define how code should be written across the Unbound Force toolchain. Each pack is a Markdown file containing numbered rules, where every rule carries a severity tag (`[MUST]`, `[SHOULD]`, or `[MAY]`) that determines how violations are handled.

Packs function as **feedforward controls** — they shape agent behavior *before* code is written, rather than correcting output afterward. This is the same principle behind type systems and linter configurations: prevent defects at the source instead of catching them downstream.

In practice, two heroes interact with convention packs at different stages:

- **Cobalt-Crush** loads the active packs during implementation and follows them as coding standards. Rules guide naming conventions, error handling patterns, documentation requirements, and architectural decisions.
- **The Divisor** enforces the same packs during code review. Each review persona checks the code against pack rules and classifies findings by the rule's severity level.

Convention packs sit at **Layer 2** of the [governance hierarchy](/docs/getting-started/constitution/#governance-hierarchy) — between the constitution (which defines principles) and agent personas (which define roles). A convention pack cannot override a constitutional principle, but it can add specific, machine-enforceable rules that operationalize those principles into concrete standards.

## Severity Levels

Every rule in a convention pack carries one of three severity tags. These tags determine how violations are handled during review and map directly to finding severity in Divisor reports.

- **`[MUST]`** — Mandatory requirements. Violations block the review: The Divisor issues REQUEST CHANGES and the finding is classified as CRITICAL or HIGH severity. Code cannot merge until all `[MUST]` violations are resolved.

- **`[SHOULD]`** — Strong recommendations. Violations are flagged in the review but do not block the merge. Findings are classified as MEDIUM severity. Teams are expected to follow `[SHOULD]` rules unless there is a documented reason to deviate.

- **`[MAY]`** — Optional improvements. Noted as suggestions in the review. Findings are classified as LOW severity. These represent best practices that improve code quality but are not required.

This three-tier model follows RFC 2119 semantics, giving teams a clear vocabulary for distinguishing between hard requirements and aspirational standards.

## Available Pack Types

Convention packs cover both language-agnostic standards and language-specific rules. The following packs are available in the toolchain:

| Pack | Scope | Description |
|------|-------|-------------|
| `default.md` | Language-agnostic | Coding style, architecture, security, testing, and documentation standards that apply to every project regardless of language. |
| `content.md` | Language-agnostic | Writing standards for documentation, blog posts, and website content — tone, structure, accuracy, and formatting rules. |
| `go.md` | Go | Go-specific rules: `gofmt` formatting, error handling patterns, GoDoc conventions, Cobra CLI patterns, and Go module structure. |
| `typescript.md` | TypeScript | TypeScript-specific rules: ESLint configuration, Prettier formatting, strict typing requirements, and architectural patterns. |
| `python.md` | Python | Python-specific rules for code style, type hints, testing conventions, and project structure. |
| `ci.md` | CI/CD | CI workflow authoring rules: GitHub Actions pinning, SHA verification, workflow file organization, and secrets handling. |
| `severity.md` | Cross-cutting | Shared severity definitions for all Divisor personas. Provides the calibration standard for CRITICAL, HIGH, MEDIUM, and LOW findings across every review. |

Not all packs are deployed to every project. `uf init` deploys `default.md`, `severity.md`, and the language-specific pack matching your project's detected language. Other packs (like `ci.md`, `python.md`, or `typescript.md`) are deployed when the corresponding language or context is detected, or can be added manually.

Each pack (except `severity.md`) has a corresponding `-custom.md` variant — for example, `default-custom.md`, `go-custom.md`, `content-custom.md`. Custom variants are user-owned files where you add project-specific rules that extend the base pack. They are never overwritten by `uf init` updates, so your customizations persist across toolchain upgrades.

## Loading Mechanism

Convention packs are stored in `.opencode/uf/packs/` at the root of your repository. The `uf init` command handles deployment:

1. **Auto-detection**: `uf init` scans the project for language marker files (`go.mod`, `tsconfig.json`, `package.json`, `pyproject.toml`, `Cargo.toml`) and deploys the matching language pack alongside the default pack.

2. **Manual override**: Use the `--lang` flag to override auto-detection when the marker file does not reflect the primary language:

   ```bash
   uf init --lang go    # Deploy Go pack regardless of detected markers
   ```

3. **Selective deployment**: Use `--divisor` to deploy only the review agents and convention packs — useful for projects that want code review without the full swarm workflow.

4. **Updates**: Running `uf init` again updates tool-owned packs to the latest embedded version. User-owned custom packs are never overwritten.

## Ownership Model

Convention pack files follow a dual-ownership model that separates toolchain standards from project-specific customizations:

**Tool-owned files** (`default.md`, `go.md`, `typescript.md`, `content.md`, `ci.md`, `severity.md`) are managed by the toolchain. When you run `uf init`, these files are automatically updated to the latest version embedded in the `uf` binary. Do not edit these files directly — your changes will be overwritten on the next `uf init` run. Tool-owned files carry a version marker (`<!-- scaffolded by uf v{version} -->`) for traceability.

**User-owned files** (`default-custom.md`, `go-custom.md`, `content-custom.md`, etc.) are never overwritten. These are where you add project-specific rules. Use the `CR-NNN` prefix for custom rule IDs to distinguish them from tool-owned rules:

```markdown
### CR-001: API Response Envelope [MUST]

All REST API responses MUST use the standard envelope format:
`{ "data": ..., "meta": { "request_id": "..." } }`.
```

This separation means you can upgrade the toolchain freely without losing your project-specific standards, while still receiving updated base rules as the toolchain evolves.

## The CI Convention Pack

The CI convention pack (`ci.md`) addresses a specific problem: CI workflow files are infrastructure-as-code, but they are often written without the same rigor applied to application code. Misconfigured workflows create security vulnerabilities, flaky builds, and maintenance burden.

The pack contains 12 rules for CI workflow authoring, covering:

- **Action pinning** — GitHub Actions must be pinned by full commit SHA, not mutable tags. A `v3` tag can be force-pushed to point at malicious code; a SHA cannot.
- **SHA verification** — Pinned SHAs must be verified against the action's release history to prevent typosquatting.
- **Workflow file organization** — Naming conventions, job structure, and step ordering for maintainability.
- **Secrets handling** — Rules for how secrets are referenced, scoped, and isolated within workflow steps.

The CI pack was introduced based on real-world findings from reviewing CI workflows across multiple repositories (see [GitHub issue #206](https://github.com/unbound-force/unbound-force/issues/206) for the original analysis). It applies the same `[MUST]`/`[SHOULD]`/`[MAY]` severity model as all other packs.

## Governance Hierarchy

Convention packs exist within a layered governance model where each layer constrains the layers below it:

```text
Constitution (principles)
    |
Convention Packs (rules)
    |
Agent Personas (roles)
    |
Commands (actions)
    |
CI Pipelines (enforcement)
```

The key constraints:

- A **convention pack cannot override a constitutional principle**. If the constitution requires testability, a convention pack cannot waive test requirements for a specific language.
- Convention packs **operationalize** the constitution's principles into concrete, machine-enforceable rules. Where the constitution says "all code must be testable," the Go convention pack specifies "exported functions must accept interfaces for external dependencies `[MUST]`."
- **Agent personas** are bound by convention packs. Cobalt-Crush cannot ignore a `[MUST]` rule, and The Divisor must flag violations at the correct severity level.

This hierarchy ensures that organizational intent flows consistently from high-level principles to specific implementation standards. See the [constitution governance hierarchy](/docs/getting-started/constitution/#governance-hierarchy) for the full model.

## See Also

- [Constitution](/docs/getting-started/constitution/) — the foundational principles that convention packs implement
- [Developer Guide: Convention Packs](/docs/getting-started/developer/#convention-packs) — how packs integrate into daily workflows
- [Code Review Tutorial](/docs/getting-started/code-review-tutorial/) — how convention pack violations appear in review findings
