# Research: v0.5.0 Documentation Sync

**Date**: 2026-03-26  
**Feature**: [spec.md](spec.md) | [plan.md](plan.md)

## R1: `uf setup` Step Inventory (v0.5.0)

**Decision**: Document `uf setup` as performing the following steps in order.

**Source**: `internal/setup/setup.go` at tag v0.5.0.

Before any steps, `uf setup` sets environment variables `OLLAMA_MODEL=granite-embedding:30m` and `OLLAMA_EMBED_DIM=256` for child processes.

| Step | Tool / Action                | Install Method                                                                          |
| ---- | ---------------------------- | --------------------------------------------------------------------------------------- |
| 1    | OpenCode (AI coding agent)   | Homebrew (`brew install anomalyco/tap/opencode`), fallback curl                         |
| 2    | Gaze (quality analysis)      | Homebrew (`brew install unbound-force/tap/gaze`)                                        |
| 3    | Mx F (manager hero)          | Homebrew (`brew install unbound-force/tap/mxf`)                                         |
| 4    | GitHub CLI (`gh`)            | Homebrew (`brew install gh`)                                                            |
| 5    | Node.js (>= 18)              | nvm, fnm, or Homebrew                                                                   |
| 6    | Bun (JS runtime)             | npm (`npm install -g bun`)                                                              |
| 7    | OpenSpec CLI                 | Bun preferred (`bun add -g @fission-ai/openspec@latest`), fallback npm                  |
| 8    | Swarm plugin                 | Bun preferred (`bun add -g opencode-swarm-plugin@latest`), fallback npm                 |
| 9    | Run `swarm setup`            | Subprocess                                                                              |
| 10   | Configure `opencode.json`    | Direct file write (adds Swarm plugin)                                                   |
| 11   | Initialize `.hive/`          | `swarm init` subprocess                                                                 |
| 12   | Ollama (local model runtime) | Homebrew (`brew install ollama`)                                                        |
| 13   | Dewey + embedding model      | Homebrew (`brew install unbound-force/tap/dewey`) + `ollama pull granite-embedding:30m` |
| 14   | Initialize `.dewey/`         | `dewey init` subprocess                                                                 |
| 15   | Build Dewey index            | `dewey index` subprocess                                                                |
| 16   | Run `uf init` (scaffold)     | In-process function call                                                                |

After completion, setup advises adding to shell profile:

```
export OLLAMA_MODEL=granite-embedding:30m
export OLLAMA_EMBED_DIM=256
```

**Rationale**: The website currently lists 7 actions. The v0.5.0 setup performs 16 steps. The documentation must reflect all 16 steps accurately.

**Documentation approach**: The website should NOT list all 16 internal steps (too granular for a website audience). Instead, group them into user-visible categories:

- Core tools (OpenCode, Gaze, Mx F, GitHub CLI)
- Development tools (Node.js, Bun, OpenSpec CLI, Swarm plugin)
- Knowledge layer (Ollama, Dewey, embedding model, Dewey initialization)
- Project scaffolding (`uf init`)

**Alternatives considered**: Listing all 16 steps individually was rejected as too implementation-focused for a website audience (Constitution III: Visitor Clarity).

## R2: `uf init` Expanded Behavior (v0.5.0)

**Decision**: Document that `uf init` now performs sub-tool initialization after file scaffolding.

**Source**: `internal/scaffold/scaffold.go` at tag v0.5.0.

New `initSubTools()` behavior:

1. Creates `.unbound-force/config.yaml` (workflow configuration file) -- skipped if already exists
2. Runs `dewey init` + `dewey index` if Dewey binary is available and `.dewey/` doesn't exist

The config file contains commented-out defaults for workflow execution modes and spec review settings.

**Rationale**: Users need to know that `uf init` creates the config file so they can customize it. The Dewey initialization is a convenience -- it runs automatically when Dewey is available.

## R3: Dewey MCP Tool Count

**Decision**: Use "40 tools across 10 categories" as the canonical count.

**Source**: Dewey README.md (authoritative) states "40 tools across 10 categories." Actual tool tables enumerate 41 distinct tools (one tool was added after the header was written). `server.go` confirms 41 registrations plus a `reload` utility.

The 10 categories are: Navigate (6), Search (4), Analyze (5), Write (10), Decision (5), Journal (2), Flashcard (3), Whiteboard (2), Semantic Search (3), Health (1).

**Rationale**: The README header is the authoritative human-facing statement. The 41 vs 40 discrepancy is minor and internal to the Dewey project. The website should use the README's stated number. The graphthulhu heritage "37 tools" number is historical and should be removed or contextualized.

**Alternatives considered**: Using "41 tools" was rejected because the Dewey README itself says 40. Using "40+" was rejected as imprecise. Using "37" (graphthulhu original) was rejected as outdated.

**Current website inconsistencies to fix**:

- `projects/dewey.md`: "preserving all 37 existing MCP tools" → update to reflect current count; "40 MCP Tools Across 9 Categories" → "40 MCP Tools Across 10 Categories"
- `team/dewey.md`: "40+ MCP tools across 10 categories" → "40 MCP tools across 10 categories"

## R4: The Divisor Sub-Personas (v0.5.0)

**Decision**: Document five sub-personas with the following details.

**Source**: `unbound-force.md` and `.opencode/agents/divisor-*.md` at tag v0.5.0.

| Persona            | Subtitle                             | Focus                                                         | Agent File             |
| ------------------ | ------------------------------------ | ------------------------------------------------------------- | ---------------------- |
| The Guard          | Intent and Cohesion                  | The "Why" -- intent drift, constitution alignment, zero-waste | `divisor-guard.md`     |
| The Architect      | Structure and Sustainability         | The "How" -- conventions, patterns, plan alignment, DRY       | `divisor-architect.md` |
| The Adversary      | Resilience and Security              | "Where it Breaks" -- error handling, security, efficiency     | `divisor-adversary.md` |
| The Operator (SRE) | Deployment and Operational Readiness | Shipping and running -- pipelines, config, observability      | `divisor-sre.md`       |
| The Tester         | Test Quality and Testability         | Observable quality -- coverage, assertion depth, isolation    | `divisor-testing.md`   |

Key details for each persona (for the team page):

**The Guard** -- Ensures the business value remains intact. Detects intent drift from the specification, verifies constitution alignment, enforces the Zero-Waste Mandate (no unused code or spec artifacts), and applies the Neighborhood Rule (changes to shared standards must not break adjacent modules).

**The Architect** -- Verifies code is clean, sustainable, and aligned with the approved plan. Enforces architectural patterns, coding conventions from convention packs, DRY principles, and plan consistency. Provides an Architectural Alignment Score (1-10).

**The Adversary** -- Skeptical auditor that finds where code breaks under stress. Covers error handling, security vulnerabilities (hardcoded secrets, injection vectors, path traversal), performance bottlenecks, and test safety. Universal security checks always run regardless of convention pack.

**The Operator (SRE)** -- Ensures the code is deployable, maintainable, and operable. Reviews release pipeline integrity, dependency health, configuration hygiene, runtime observability (exit codes, error messages), upgrade paths, and operational documentation.

**The Tester** -- Ensures tests are meaningful, not just present. Reviews test architecture (arrange/act/assert), coverage strategy (contract surface, not just line coverage), assertion depth (specific values, not just "no error"), test isolation, and regression protection.

**Rationale**: The website's `team/the-divisor.md` page describes only 3 personas (Guard, Architect, Adversary). The v0.5.0 source has 5 agent files and explicitly states "five canonical roles." The team page must be updated to match.

**Note on naming**: `unbound-force.md` uses "SRE" for the fourth persona, but the agent file (`divisor-sre.md`) titles it "The Operator." The website should use "SRE" as the primary label with "(The Operator)" as a subtitle, matching the upstream naming convention used in the roster description.

## R5: Embedding Model Alignment

**Decision**: Document the `OLLAMA_MODEL` and `OLLAMA_EMBED_DIM` environment variables.

**Source**: `internal/setup/setup.go` at tag v0.5.0; `AGENTS.md` "Embedding Model Alignment" section.

v0.5.0 aligns the swarm (via Swarm plugin) and Dewey on IBM Granite Embedding:

- Model: `granite-embedding:30m`
- Embedding dimension: `256`
- License: Apache 2.0

Environment variables to add to shell profile:

```bash
export OLLAMA_MODEL=granite-embedding:30m
export OLLAMA_EMBED_DIM=256
```

**Rationale**: Without these variables, child processes spawned outside of `uf setup` (e.g., `dewey serve`, `swarm init`) may use different embedding models, causing search quality inconsistencies.

**Documentation approach**: Add a brief section to the knowledge retrieval page (`knowledge.md`) explaining the alignment requirement and the two environment variables. Also mention in the setup section of `common-workflows.md` as a post-setup step.

## R6: Branch Conventions

**Decision**: Document `opsx/<change-name>` branch naming in user-facing content.

**Source**: `.opencode/command/opsx-propose.md`, `.opencode/command/opsx-apply.md`, `.opencode/command/opsx-archive.md` at tag v0.5.0; `AGENTS.md` "Branch Conventions" section.

Behavior:

- `/opsx-propose <name>` creates and checks out an `opsx/<name>` branch (guard: skips if already on correct branch, errors if on different `opsx/*` branch)
- `/opsx-apply` validates you're on the correct `opsx/<name>` branch before proceeding (hard gate)
- `/opsx-archive` returns to `main` branch after archiving

The `opsx/` prefix ensures visual distinction from Speckit branches (`NNN-<short-name>`) in `git branch` output.

**Rationale**: This is a behavioral change that affects the developer's daily workflow. If a developer tries `/opsx-apply` on the wrong branch, it will error. They need to know this.

**Documentation approach**: Add a brief note in the bug fix / OpenSpec section of `common-workflows.md` mentioning branch creation and validation. Also add to `developer.md` under the Speckit vs OpenSpec section.

## R7: `/workflow seed` and `/workflow start` Commands

**Decision**: Document these commands and the `.unbound-force/config.yaml` configuration file.

**Source**: `.opencode/command/workflow-seed.md`, `unbound-force.md`, and `internal/orchestration/config.go` at tag v0.5.0.

**`/workflow seed`**: Convenience command that creates a backlog item and starts a workflow with `define=swarm` in one operation. Usage: `/workflow seed "description" [--spec-review]`.

**`/workflow start`**: Existing command with new flags:

- `--define-mode=human|swarm` (default: human)
- `--spec-review` (default: false)

**`.unbound-force/config.yaml`**: Project-level defaults:

```yaml
workflow:
  execution_modes:
    define: swarm # or "human" (default)
  spec_review: false
```

Merge semantics: Config values are the base; CLI flags override. For spec review, OR logic applies (either config or CLI being true enables it).

**Rationale**: The website already describes the seed concept narratively (from spec 009) but doesn't mention the commands. Users need the actual invocation syntax.

**Documentation approach**: Add command reference to `common-workflows.md` in the Swarm Delegation section. Add `/workflow seed` reference to `product-owner.md` alongside the seed narrative. Add config file documentation to `common-workflows.md`.

## R8: `mxf` CLI Installation

**Decision**: Document Mx F as a separate Homebrew formula installed by `uf setup`.

**Source**: `internal/setup/setup.go` at tag v0.5.0, line 346: `brew install unbound-force/tap/mxf`.

The `mxf` binary is NOT bundled with the `uf` CLI. It is a separate Homebrew formula (`unbound-force/tap/mxf`) installed as step 3 of `uf setup`.

**Rationale**: The website's `product-manager.md` page currently says "Install via Homebrew (included with the `unbound-force` package, the `uf` CLI)." This is inaccurate -- `mxf` is a separate formula. However, since `uf setup` installs it automatically, the practical user experience doesn't change. The documentation should note that `uf setup` handles the installation.

**Documentation approach**: Update the install note in `product-manager.md` if it conflicts, but since `uf setup` handles everything, the primary fix is in the setup documentation itself.
