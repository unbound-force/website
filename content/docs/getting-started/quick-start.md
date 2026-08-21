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

On **Fedora/RHEL** without Homebrew, install `uf` from the RPM package, then run `uf setup`. Setup automatically detects `dnf` and uses it for tools available in Fedora repos (e.g., Podman). Tools without native packages (Ollama, DevPod) fall back to their official curl installers with an interactive confirmation prompt. See the [CLI reference](/docs/reference/cli/#install-cascade) for the full install cascade.

`uf setup` installs everything in one command:

- **Core tools** -- OpenCode (AI coding environment), Gaze (quality analysis), Mx F (manager hero), GitHub CLI
- **Development tools** -- Node.js, OpenSpec CLI, Replicator (multi-agent coordination)
- **Knowledge layer** -- Ollama (local model runtime), Dewey (semantic search), IBM Granite embedding model
- **Project scaffolding** -- agents, commands, convention packs, templates, workflow configuration, and `.gitignore` management via `uf init`

Setup detects your platform and version managers (goenv, nvm, fnm, Homebrew, dnf) and installs through them. Use `--dry-run` to preview what would be installed without making changes. For a detailed breakdown of what setup and init do under the hood, see [uf setup / uf init Lifecycle](/docs/getting-started/common-workflows/#uf-setup-uf-init-lifecycle).

## Verify

```bash
uf doctor
```

Doctor checks 7 areas and shows pass/warn/fail for each with platform-appropriate install hints. On Fedora/RHEL, hints show `dnf install` commands for tools available in Fedora repos. Fix any failures by copying the suggested command from the output.

## Start Working

OpenCode has two modes -- switch between them with **Tab**:

- **Plan mode**: Read-only. Explore ideas, analyze code, think through your approach.
- **Build mode**: Full access. Create files, run commands, execute pipelines.

### Large Tasks

Start in plan mode to explore your idea, then switch to build mode:

```text
/speckit.specify    # describe what you want to build
/uf.unleash         # the swarm takes it from here
/uf.finale          # commit, push, create PR
```

`/uf.unleash` runs the entire pipeline autonomously: clarify, plan, implement, test, and review. It pauses when it needs you and resumes where it left off. See the [blog post](/blog/unleash-in-practice/) for a walkthrough.

### Small Tasks

For bug fixes and tactical changes:

```text
/opsx-propose fix-the-bug    # create proposal + design + tasks
/uf.cobalt-crush             # implement with convention pack adherence
/uf.finale                   # commit, push, create PR
```

## The Stack

Unbound Force runs on four tools that form a layered stack. `uf setup` installs all of them, but each is independently useful:

| Layer            | Tool                                                      | What It Does                                                                                                               |
| ---------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **CLI**          | [`uf`](/docs/reference/cli/)                              | Project scaffolding, environment setup, health checks, configuration, sandboxed execution, LLM gateway.                    |
| **Agent**        | [OpenCode](https://opencode.ai)                           | The AI coding environment where you interact, write code, and run commands. The personas run inside OpenCode.              |
| **Planning**     | [Speckit](https://github.com/github/spec-kit) (spec-kit)  | A specification pipeline that turns ideas into structured specs, plans, and tasks before implementation begins.            |
| **Coordination** | [Replicator](https://github.com/unbound-force/replicator) | Multi-agent coordination: parallel workers, git-backed tracking, file reservations, and semantic memory. Single Go binary. |

Each tool is independently useful, but they compose into the full Unbound Force workflow: scaffold with `uf`, plan with Speckit, execute with OpenCode, coordinate with Replicator.

## Next Steps

- **[Constitution](/docs/getting-started/constitution/)** -- The core principles that govern all heroes. The constitution must exist before running `/speckit.specify`
- **[Developer Guide](/docs/getting-started/developer/)** -- Daily workflow, Speckit pipeline, Replicator coordination, convention packs
- **[Tester Guide](/docs/getting-started/tester/)** -- Gaze quality analysis, CRAP scores, coverage ratchets, CI integration
- **[Product Owner Guide](/docs/getting-started/product-owner/)** -- Muti-Mind backlog management, priority scoring, acceptance decisions
- **[Product Manager Guide](/docs/getting-started/product-manager/)** -- Mx F metrics, dashboards, coaching, retrospectives
- **[Common Workflows](/docs/getting-started/common-workflows/)** -- End-to-end flows for features, bug fixes, code reviews, and setup
