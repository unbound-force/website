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
- [Unbound Force](https://github.com/unbound-force/unbound-force/issues) — agent personas and roles

Include steps to reproduce, expected behavior, and actual behavior. For feature requests, describe the problem you are trying to solve.

## Submitting Pull Requests

We follow the standard GitHub pull request workflow:

1. Fork the repository
2. Create a feature branch named with a number prefix (e.g., `003-feature-name`)
3. Make your changes
4. Submit a pull request against the `main` branch
5. Address any review feedback from The Divisor council

All pull requests are reviewed by The Divisor — a council of three sub-personas (The Guard, The Architect, and The Adversary) that evaluate intent, structure, and resilience. Collective approval from all three is required before merge.

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

## The Speckit Workflow

All non-trivial feature work goes through the [Specify](https://github.com/github/spec-kit) pipeline. This ensures structured planning before implementation:

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
- [Specify (spec-kit)](https://github.com/github/spec-kit)
