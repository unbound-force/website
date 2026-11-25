---
title: "Contributing"
description: "How to contribute to Unbound Force projects — reporting issues, submitting pull requests, and following project conventions."
lead: "Join the swarm. Here is how to contribute to Unbound Force projects."
date: 2026-02-23T00:00:00+00:00
draft: false
weight: 10
toc: true
---

## Reporting Issues

Found a bug or have a feature request? Open an issue on the relevant GitHub repository:

- [Unbound Force Website](https://github.com/unbound-force/website/issues) — this website
- [Gaze](https://github.com/unbound-force/gaze/issues) — test quality analysis tool
- [Dewey](https://github.com/unbound-force/dewey/issues) — semantic knowledge retrieval
- [Replicator](https://github.com/unbound-force/replicator/issues) — multi-agent coordination
- [Unbound Force](https://github.com/unbound-force/unbound-force/issues) — agent personas and roles

Include steps to reproduce, expected behavior, and actual behavior. For feature requests, describe the problem you are trying to solve.

## Submitting Pull Requests

We follow the standard GitHub pull request workflow:

1. Fork the repository
2. Create a feature branch named for the change's workflow tier: `NNN-<name>` for Speckit, `opsx/<name>` for OpenSpec, or `quick/<name>` for Quick (see [Workflow Tiers](#workflow-tiers))
3. Make your changes
4. Submit a pull request against the `main` branch
5. Address any review feedback from The Divisor council

All pull requests are reviewed by The Divisor — a council of nine sub-personas. Six review personas (The Guard, The Architect, The Adversary, The Operator (SRE), The Tester, and The Curator) evaluate intent, structure, resilience, operational readiness, and test quality. Three content personas (The Scribe, The Herald, and The Envoy) ensure documentation, blog posts, and public communications meet quality standards. Collective approval from all active personas is required before merge. See [convention packs](/docs/getting-started/developer/#convention-packs) for the specific coding standards your contributions will be reviewed against.

## Conventional Commits

All commit messages must follow the [Conventional Commits](https://www.conventionalcommits.org/) format:

```text
type: description
```

Valid types:

| Type        | Purpose            | Example                                       |
| ----------- | ------------------ | --------------------------------------------- |
| `feat:`     | New feature        | `feat: add Gaze project page`                 |
| `fix:`      | Bug fix            | `fix: correct broken link in team navigation` |
| `docs:`     | Documentation      | `docs: update getting started guide`          |
| `chore:`    | Maintenance        | `chore: update dependencies`                  |
| `style:`    | Formatting         | `style: fix indentation in config file`       |
| `refactor:` | Code restructuring | `refactor: simplify homepage template logic`  |
| `ci:`       | CI/CD changes      | `ci: add build verification step`             |

## Workflow Tiers

Every change follows a workflow, but which one is decided per change. No single tier is mandatory: pick the one that fits the change's scope and risk. [`/uf.unleash`](/docs/getting-started/common-workflows/#tier-routing) makes this choice automatically.

| Tier | Choose when | Artifacts | Branch |
| ---- | ----------- | --------- | ------ |
| **Speckit** | New capability or cross-cutting change: 3+ user stories, multiple subsystems | `specs/NNN-name/` | `NNN-<name>` |
| **OpenSpec** | Focused feature or non-trivial fix: 1–2 user stories, bounded blast radius | `openspec/changes/name/` | `opsx/<name>` |
| **Quick** | Trivial fix, chore, or docs change where ceremony exceeds value | None | `quick/<name>` |

Changes that touch security-sensitive code, authentication or authorization, data-loss paths, or a public API contract use OpenSpec at minimum, never Quick. When a change could fit two tiers, use the heavier one. Every tier, Quick included, needs passing CI and code review before merge.

## The Speckit Workflow

Changes routed to the Speckit tier go through the full [Speckit](https://github.com/github/spec-kit) pipeline, which ensures structured planning before implementation:

```text
constitution -> specify -> clarify -> plan -> tasks -> analyze -> checklist -> implement
```

Each stage builds on the previous one:

- **Constitution** — project-level governance and principles
- **Specify** — turn a feature description into a structured specification
- **Clarify** — reduce ambiguity before planning
- **Plan** — generate the technical implementation plan
- **Tasks** — create actionable, dependency-ordered task list
- **Analyze** — cross-artifact consistency check
- **Checklist** — requirement quality validation
- **Implement** — execute the plan task by task

The constitution (`.specify/memory/constitution.md`) is the highest-authority document in any Unbound Force project. All work must align with it.

## Links

- [Unbound Force GitHub Organization](https://github.com/unbound-force)
- [Website Repository](https://github.com/unbound-force/website)
- [Gaze Repository](https://github.com/unbound-force/gaze)
- [Dewey Repository](https://github.com/unbound-force/dewey)
- [Replicator Repository](https://github.com/unbound-force/replicator)
- [Speckit (spec-kit)](https://github.com/github/spec-kit)
