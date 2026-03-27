# Feature Specification: v0.5.0 Documentation Sync

**Feature Branch**: `010-v050-docs-sync`  
**Created**: 2026-03-26  
**Status**: Draft  
**Input**: User description: "Update website documentation to reflect v0.5.0 changes from the unbound-force upstream repository"

## User Scenarios & Testing _(mandatory)_

### User Story 1 - Setup Documentation Reflects Full Toolchain (Priority: P1)

A new user reads the environment setup section of the website to install and configure the Unbound Force swarm. The setup documentation must accurately describe what `uf setup` installs (now 16 steps including Mx F, GitHub CLI, OpenSpec CLI, Ollama, and Dewey initialization) so the user understands what to expect and does not need to install tools manually that are already handled by setup.

**Why this priority**: Incorrect setup documentation is the most damaging gap -- new users following outdated instructions will have a different experience than described, eroding trust immediately. The website currently describes 7 actions for `uf setup` when v0.5.0 performs 16 steps. It also presents Dewey and Ollama as separate manual installations when they are now included in setup.

**Independent Test**: Can be fully tested by reading the setup section and comparing each described step against the actual `uf setup` output in v0.5.0. The documentation must match the actual tool behavior.

**Acceptance Scenarios**:

1. **Given** a user reading the environment setup section, **When** they follow the documented `uf setup` steps, **Then** every tool installed by v0.5.0's setup is mentioned in the documentation (OpenCode, Gaze, Swarm plugin, Mx F, GitHub CLI, OpenSpec CLI, Ollama, Dewey init, Dewey index, project scaffolding via `uf init`).
2. **Given** a user reading the environment setup section, **When** they look for Dewey installation instructions, **Then** the documentation explains that `uf setup` now handles Dewey and Ollama installation automatically, with manual installation only needed for projects where `uf setup` was run before v0.5.0.
3. **Given** a user reading the environment setup section, **When** they look for `uf init` behavior, **Then** the documentation explains that `uf init` now creates `.unbound-force/config.yaml` and runs `dewey init` + `dewey index` when Dewey is available.

---

### User Story 2 - Workflow Commands and Configuration Documented (Priority: P1)

A user reads the common workflows or product owner pages to understand how to start a feature workflow. The documentation must describe the new `/workflow seed` command, the `--define-mode` and `--spec-review` flags on `/workflow start`, and the project-level `.unbound-force/config.yaml` configuration file, so the user knows how to control autonomous vs. manual define behavior.

**Why this priority**: These are the primary new capabilities in v0.5.0. The website already describes the seed concept and autonomous define in narrative form (from spec 009), but does not document the actual commands and configuration that users need to invoke these capabilities. Without this, users understand the concept but cannot execute it.

**Independent Test**: Can be tested by searching the website for `/workflow seed`, `/workflow start`, `--define-mode`, `--spec-review`, and `.unbound-force/config.yaml` -- all must appear with accurate descriptions of their behavior.

**Acceptance Scenarios**:

1. **Given** a user reading the common workflows page, **When** they look for how to seed a feature, **Then** they find the `/workflow seed` command with usage syntax and a description of what it does (creates a backlog item and starts a workflow with `define=swarm` in one operation).
2. **Given** a user reading the common workflows page, **When** they look for workflow configuration, **Then** they find documentation of `/workflow start` with `--define-mode=human|swarm` and `--spec-review` flags, plus the `.unbound-force/config.yaml` file for setting project-level defaults.
3. **Given** a user reading the product owner page, **When** they look for how to seed a feature, **Then** they find the `/workflow seed` command referenced alongside the seed concept already documented there.

---

### User Story 3 - The Divisor Accurately Described as Five Personas (Priority: P2)

A user reads the team page for The Divisor to understand the review council structure. The documentation must accurately describe five sub-personas (Guard, Architect, Adversary, Testing, SRE) rather than three, matching the v0.5.0 upstream definition.

**Why this priority**: The team page and team index currently describe only three sub-personas, contradicting the getting-started guides which already reference five. This inconsistency confuses users about the review council's actual structure. The fix is straightforward but important for factual accuracy.

**Independent Test**: Can be tested by reading `team/the-divisor.md` and `team/_index.md` and verifying they describe five personas matching the v0.5.0 `unbound-force.md` source document.

**Acceptance Scenarios**:

1. **Given** a user reading the Divisor team page, **When** they look at the council structure, **Then** they see five sub-personas listed: Guard (Intent and Cohesion), Architect (Structure and Sustainability), Adversary (Resilience and Security), Testing (Test Quality and Coverage), and SRE (Operational Readiness).
2. **Given** a user reading the team index page, **When** they see The Divisor's description, **Then** it references five sub-personas instead of three.
3. **Given** a user reading any page on the site that mentions The Divisor's personas, **When** they compare across pages, **Then** the persona count and names are consistent everywhere.

---

### User Story 4 - Embedding Model Alignment Documented (Priority: P2)

A user configures their environment for optimal semantic search performance. The documentation must explain that v0.5.0 aligns the swarm and Dewey on IBM Granite Embedding (`granite-embedding:30m`) and that users should set `OLLAMA_MODEL` and `OLLAMA_EMBED_DIM` environment variables in their shell profile.

**Why this priority**: Without these environment variables, users may experience degraded or inconsistent embedding behavior. The website already documents Granite as the model but does not mention the alignment requirement or the environment variables.

**Independent Test**: Can be tested by searching the knowledge retrieval or environment setup pages for `OLLAMA_MODEL` and `OLLAMA_EMBED_DIM` environment variable documentation.

**Acceptance Scenarios**:

1. **Given** a user reading the environment setup or knowledge retrieval pages, **When** they look for embedding model configuration, **Then** they find instructions to add `export OLLAMA_MODEL=granite-embedding:30m` and `export OLLAMA_EMBED_DIM=256` to their shell profile.
2. **Given** a user who has already installed Dewey, **When** they read the embedding alignment section, **Then** they understand why these variables are needed (swarm and Dewey alignment on the same model).

---

### User Story 5 - Branch Conventions in User-Facing Docs (Priority: P3)

A developer reads the workflow documentation to understand how feature branches are managed. The documentation must describe the `opsx/<change-name>` branch naming convention for OpenSpec workflows and the branch enforcement behavior (automatic creation on `/opsx-propose`, validation on `/opsx-apply`).

**Why this priority**: This is a new v0.5.0 workflow enforcement that affects the developer's daily experience. While the information exists in AGENTS.md, it is absent from user-facing content pages where developers would look for it.

**Independent Test**: Can be tested by reading the developer guide or common workflows page for branch naming conventions and verifying `opsx/` branch documentation is present.

**Acceptance Scenarios**:

1. **Given** a developer reading the bug fix / OpenSpec workflow section, **When** they look for how branches work, **Then** they see that `/opsx-propose` creates an `opsx/<change-name>` branch and `/opsx-apply` validates the current branch before proceeding.

---

### User Story 6 - Dewey MCP Tool Count Consistency (Priority: P3)

A user compares the Dewey project page and team page. The MCP tool counts and category counts must be consistent across all pages that reference them.

**Why this priority**: Inconsistent numbers (37, 40, 40+ tools; 9 vs 10 categories) undermine credibility. The fix is small but addresses a pre-existing inconsistency that should be reconciled against the v0.5.0 Dewey repository.

**Independent Test**: Can be tested by searching the site for mentions of MCP tool counts and verifying they all state the same number.

**Acceptance Scenarios**:

1. **Given** a user reading any page that mentions Dewey's MCP tool count, **When** they compare across pages, **Then** all pages report the same number of tools and categories.

---

### Edge Cases

- What happens when the website references v0.5.0 features that require Dewey but the user does not have Dewey? Documentation must clearly indicate fallback behavior for every Dewey-dependent feature.
- What happens when a user has a pre-v0.5.0 `uf setup` installation? The setup documentation should note that existing installations can re-run `uf setup` to pick up new tools.
- What happens when a user reads the Divisor team page after reading the getting-started guides? The persona count must be consistent to avoid confusion.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The environment setup section of `common-workflows.md` MUST list all tools installed by `uf setup` in v0.5.0 (OpenCode, Gaze, Swarm plugin, Mx F, GitHub CLI, OpenSpec CLI, Ollama, Dewey initialization, project scaffolding).
- **FR-002**: The environment setup section MUST explain that Dewey and Ollama are now installed automatically by `uf setup`, replacing the separate manual installation step.
- **FR-003**: The environment setup section MUST document that `uf init` now creates `.unbound-force/config.yaml` and runs `dewey init` + `dewey index` when Dewey is available.
- **FR-004**: The common workflows page MUST document the `/workflow seed` command with usage syntax and behavior description.
- **FR-005**: The common workflows page MUST document the `/workflow start` command's `--define-mode=human|swarm` and `--spec-review` flags.
- **FR-006**: The common workflows or product owner page MUST document the `.unbound-force/config.yaml` project-level workflow configuration file, including its structure and merge semantics with CLI flags.
- **FR-007**: The `team/the-divisor.md` page MUST describe five sub-personas: Guard, Architect, Adversary, Testing, and SRE.
- **FR-008**: The `team/_index.md` page MUST reference five sub-personas for The Divisor instead of three.
- **FR-009**: The knowledge retrieval or environment setup page MUST document the `OLLAMA_MODEL=granite-embedding:30m` and `OLLAMA_EMBED_DIM=256` environment variables for embedding model alignment.
- **FR-010**: The developer guide or common workflows page MUST document the `opsx/<change-name>` branch naming convention and branch enforcement behavior for OpenSpec workflows.
- **FR-011**: All pages referencing Dewey's MCP tool count MUST use a consistent number of tools and categories, reconciled against the v0.5.0 Dewey repository.
- **FR-012**: The `developer.md` page MUST update its description of what `uf setup` installs to match v0.5.0 behavior.
- **FR-013**: The `product-owner.md` page MUST reference the `/workflow seed` command alongside the existing seed narrative.
- **FR-014**: All documentation changes MUST pass `npm run build` without errors.
- **FR-015**: All documentation changes MUST render correctly in both light and dark mode.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: Every tool installed by `uf setup` in v0.5.0 is documented on the website -- zero undocumented tools.
- **SC-002**: Every new command or flag introduced in v0.5.0 (`/workflow seed`, `--define-mode`, `--spec-review`, `.unbound-force/config.yaml`) appears on at least one user-facing documentation page.
- **SC-003**: The Divisor persona count is consistent across all pages -- five personas everywhere, zero contradictions.
- **SC-004**: Dewey MCP tool counts are identical across all pages that reference them.
- **SC-005**: `npm run build` succeeds with zero errors after all documentation changes.
- **SC-006**: Zero cross-page factual contradictions remain in the documentation (verified by cross-referencing key claims across all updated pages).

## Clarifications

### Session 2026-03-26

- Q: Is `mxf` bundled with the `uf` CLI or a separate Homebrew formula? → A: Separate formula (`unbound-force/tap/mxf`), verified in v0.5.0 `setup.go`.

## Assumptions

- The v0.5.0 `unbound-force.md` and agent files are the authoritative source for persona descriptions, tool capabilities, and workflow behavior.
- The Dewey MCP tool count should be reconciled against the actual Dewey v0.5.0 codebase or README, not assumed from any single website page.
- The `mxf` CLI is a separate Homebrew formula (`brew install unbound-force/tap/mxf`), not bundled with the `uf` CLI. Verified against v0.5.0 `setup.go` -- `uf setup` installs it as a distinct step via `brew install unbound-force/tap/mxf`.
- The OpenSpec CLI (`@fission-ai/openspec@latest`) is a separate npm/bun package installed by `uf setup`, not bundled with `uf`.
- The website's primary audience is developers and technical product owners who will use these commands directly.
