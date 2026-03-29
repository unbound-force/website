---
title: "Quick Start"
description: "Install the Unbound Force toolchain in one command and start using the AI agent swarm for software engineering."
lead: "One command to install everything. Then start building."
date: 2026-02-23T00:00:00+00:00
draft: false
weight: 20
toc: true
---

## Install

Install the `uf` CLI and run setup to install the full toolchain:

```bash
brew install unbound-force/tap/unbound-force
uf setup
```

`uf setup` installs everything in one command:

- **Core tools** -- OpenCode (AI coding environment), Gaze (quality analysis), Mx F (manager hero), GitHub CLI
- **Development tools** -- Node.js, Bun, OpenSpec CLI, Swarm plugin (multi-agent coordination)
- **Knowledge layer** -- Ollama (local model runtime), Dewey (semantic search), IBM Granite embedding model
- **Project scaffolding** -- agents, commands, convention packs, templates, and workflow configuration via `uf init`

Setup detects your version managers (goenv, nvm, fnm, Homebrew) and installs through them. Use `--dry-run` to preview what would be installed without making changes.

## Verify

```bash
uf doctor
```

Doctor checks 7 areas and shows pass/warn/fail for each with install hints. Fix any failures by copying the suggested command from the output.

## Start Working

Open OpenCode and try your first task:

```text
/swarm "your first task description"
```

Or start with the Speckit pipeline for a new feature:

```text
/speckit.specify
```

## The Stack

Unbound Force runs on three tools that form a layered stack. `uf setup` installs all of them, but each is independently useful:

| Layer            | Tool                                                     | What It Does                                                                                                    |
| ---------------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Agent**        | [OpenCode](https://opencode.ai)                          | The AI coding environment where you interact, write code, and run commands. The personas run inside OpenCode.   |
| **Planning**     | [Speckit](https://github.com/github/spec-kit) (spec-kit) | A specification pipeline that turns ideas into structured specs, plans, and tasks before implementation begins. |
| **Coordination** | [Swarm](https://www.swarmtools.ai/)                      | An OpenCode plugin that enables multi-agent parallelism and learning amongst team members.                      |

Each tool is independently useful, but they compose into the full Unbound Force workflow: plan with Speckit, execute with OpenCode, coordinate with Swarm.

## Next Steps

- **[Developer Guide](/docs/getting-started/developer/)** -- Daily workflow, Speckit pipeline, Swarm coordination, convention packs
- **[Tester Guide](/docs/getting-started/tester/)** -- Gaze quality analysis, CRAP scores, coverage ratchets, CI integration
- **[Product Owner Guide](/docs/getting-started/product-owner/)** -- Muti-Mind backlog management, priority scoring, acceptance decisions
- **[Product Manager Guide](/docs/getting-started/product-manager/)** -- Mx F metrics, dashboards, coaching, retrospectives
- **[Common Workflows](/docs/getting-started/common-workflows/)** -- End-to-end flows for features, bug fixes, code reviews, and setup
