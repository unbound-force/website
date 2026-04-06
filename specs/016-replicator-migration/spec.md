# Feature Specification: Replicator Migration

**Feature Branch**: `016-replicator-migration`
**Created**: 2026-04-06
**Status**: Draft
**Input**: Replace all Swarm plugin references with Replicator across the website. Replicator (Go binary, 53 MCP tools, zero runtime dependencies) has replaced the Node.js Swarm plugin. Remove all swarmtools.ai references except heritage attribution. Create Replicator project page. Add to homepage.

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Replace Swarm Product References with Replicator (Priority: P1)

A new developer reads the website and encounters references to "Swarm plugin," `opencode-swarm-plugin`, `npm install`, `swarmtools.ai`, and `swarm init` — none of which are accurate anymore. Replicator is now the coordination layer: a Go binary installed via Homebrew, integrated as an MCP server entry (not a plugin array entry), with `replicator init`/`replicator serve` subcommands.

**Why this priority**: Factually incorrect installation and configuration instructions are the highest-severity gap. A developer following the current docs will install the wrong tool.

**Independent Test**: Search the entire content directory for "swarmtools.ai", "opencode-swarm-plugin", "Swarm plugin", and `swarm init`. Zero results for all except the heritage attribution on the Replicator project page.

**Acceptance Scenarios**:

1. **Given** a developer reading any page on the site, **When** they search for "swarmtools.ai", **Then** zero results are found except the heritage attribution on the Replicator project page.
2. **Given** a developer reading the Stack table (Quick Start or Getting Started landing), **Then** the coordination layer row shows "Replicator" with a link to `github.com/unbound-force/replicator`, not "Swarm" with swarmtools.ai.
3. **Given** a developer reading setup instructions, **Then** install commands show `brew install unbound-force/tap/replicator`, not `npm install -g opencode-swarm-plugin`.
4. **Given** a developer reading the `uf init` documentation, **Then** it describes `opencode.json` with `mcp.replicator` entry (not plugin array with `opencode-swarm-plugin`).

---

### User Story 2 — Remove `/swarm` Command References (Priority: P1)

The website references `/swarm "task description"` as a user-facing command in the Quick Start, Developer Guide, and Common Workflows. This command does not exist in Replicator. The primary entry point is `/unleash` (full pipeline) or `/speckit.implement` (manual implementation step).

**Why this priority**: Referencing a nonexistent command causes immediate confusion when a developer tries to use it.

**Independent Test**: Search the entire content directory for `/swarm`. Zero results in user-facing docs.

**Acceptance Scenarios**:

1. **Given** a developer reading the Quick Start "Start Working" section, **When** they look for how to run a quick task, **Then** `/swarm` is not offered as an option.
2. **Given** a developer reading the Developer Guide, **When** they look for the "Working with Replicator" section, **Then** it describes Replicator's coordination capabilities without referencing a `/swarm` slash command.
3. **Given** a developer reading the Common Workflows implement stage, **When** they look for how to start implementation, **Then** they find `/speckit.implement` or `/unleash`, not `/swarm`.

---

### User Story 3 — Create Replicator Project Page and Homepage Card (Priority: P1)

Gaze and Dewey each have project pages and homepage cards. Replicator is a first-party project in the org with a shipped Homebrew formula, 53 MCP tools, and 190+ tests — it deserves the same treatment.

**Why this priority**: Without a project page, visitors can't learn about Replicator's capabilities. Without a homepage card, the third shipped project is invisible from the site's highest-traffic page.

**Independent Test**: Navigate to `/docs/projects/replicator/` — the page exists with installation, features, and architecture. The homepage shows 3 project cards (Gaze, Dewey, Replicator).

**Acceptance Scenarios**:

1. **Given** a visitor on the projects index page, **When** they look for Replicator, **Then** they find a project page describing its capabilities, installation, MCP tool categories, and architecture.
2. **Given** a visitor on the homepage, **When** they scroll to the Projects section, **Then** they see 3 project cards (Gaze, Dewey, Replicator) in a 3-column grid.
3. **Given** a developer reading the Replicator project page, **When** they look for heritage, **Then** they find one attribution line: "Go rewrite of cyborg-swarm, originally forked from swarm-tools by Joel Hooks."

---

### User Story 4 — Update Internal Agent and Command Files (Priority: P2)

The `/unleash` command file, Cobalt-Crush agent, and Speckit workflow skill all reference "Swarm plugin" as a product. These affect agent behavior — agents read these files to understand how to use coordination tools.

**Why this priority**: While the MCP tool names are unchanged (agents call `swarm_worktree_create` by name regardless), the fallback messages and install hints reference the wrong product ("Install the Swarm plugin for...").

**Independent Test**: Search `.opencode/` for "Swarm plugin" and "swarmtools" — zero results.

**Acceptance Scenarios**:

1. **Given** an agent reading the `/unleash` command, **When** Replicator is not available, **Then** the fallback message says "Install Replicator for parallel execution" (not "Install the Swarm plugin").
2. **Given** an agent reading the Cobalt-Crush agent file, **When** it describes coordination, **Then** it references "Replicator" (not "Swarm") as the coordination product.

---

### User Story 5 — Update Dewey Sources Configuration (Priority: P3)

The `.dewey/sources.yaml` includes a `web-swarm` source pointing to swarmtools.ai. This should be removed. The Replicator repo should be added as a disk source if not already auto-detected.

**Why this priority**: Lowest priority because it only affects the Dewey knowledge index, not user-facing content. But stale web sources waste crawl time and pollute search results with outdated Swarm plugin docs.

**Independent Test**: Read `.dewey/sources.yaml` and verify no swarmtools.ai references exist.

**Acceptance Scenarios**:

1. **Given** a developer reading `.dewey/sources.yaml`, **When** they look at web sources, **Then** no swarmtools.ai entry exists.

---

### Edge Cases

- What happens when a developer has an old installation with the Swarm plugin still configured? The website should describe the current state (Replicator) — `uf setup` handles the migration automatically.
- What happens when a page uses "swarm" as a generic noun (e.g., "the swarm takes over")? These KEEP as-is — "swarm" as a concept is the project's identity. Only "Swarm" as a product name changes.
- The MCP tool names still contain "swarm" (e.g., `swarm_worktree_create`, `swarmmail_send`). These stay — they're API identifiers, not product references.

## Requirements _(mandatory)_

### Functional Requirements

**Product References (US1):**

- **FR-001**: All "Swarm plugin" product name references MUST be replaced with "Replicator" across all content pages.
- **FR-002**: All swarmtools.ai URLs MUST be removed except one heritage attribution on the Replicator project page.
- **FR-003**: All `opencode-swarm-plugin` npm package references MUST be replaced with `brew install unbound-force/tap/replicator`.
- **FR-004**: All `swarm init`, `swarm setup` CLI references MUST be replaced with `replicator init`, `replicator setup`.
- **FR-005**: The Stack table on Quick Start and Getting Started landing pages MUST show "Replicator" with a link to `github.com/unbound-force/replicator`.
- **FR-006**: The `opencode.json` documentation MUST show `mcp.replicator` entry format (not plugin array).
- **FR-007**: The "Working with Swarm" section heading in developer.md MUST be renamed to "Working with Replicator".

**`/swarm` Command Removal (US2):**

- **FR-008**: All `/swarm "task"` command references MUST be removed from user-facing documentation.
- **FR-009**: Where `/swarm` was offered as a quick-start option, the documentation MUST offer `/unleash` as the primary option and `/speckit.implement` as the manual alternative.

**Replicator Project Page (US3):**

- **FR-010**: A new project page MUST exist at `content/docs/projects/replicator.md` with installation, features (53 MCP tools across 4 categories), CLI commands, and architecture.
- **FR-011**: The project page MUST include one heritage attribution line.
- **FR-012**: The homepage Projects section MUST include a Replicator card in a 3-column grid alongside Gaze and Dewey.
- **FR-013**: The projects index page MUST list Replicator.

**Internal Files (US4):**

- **FR-014**: The `/unleash` command file MUST replace "Swarm plugin" references with "Replicator" in all fallback messages and install hints.
- **FR-015**: The Cobalt-Crush agent file MUST replace "Swarm" product references with "Replicator" in the coordination section.
- **FR-016**: The Speckit workflow skill MUST replace "Swarm coordinator" with "Replicator".

**Dewey Sources (US5):**

- **FR-017**: The `web-swarm` source entry in `.dewey/sources.yaml` MUST be removed.

**Cross-cutting:**

- **FR-018**: All changes MUST pass `npm run build` without errors.
- **FR-019**: Generic "swarm" noun usage (80+ occurrences) MUST NOT be changed.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: Zero occurrences of "swarmtools.ai" in user-facing content (except 1 heritage attribution on Replicator project page).
- **SC-002**: Zero occurrences of "opencode-swarm-plugin" anywhere on the site.
- **SC-003**: Zero occurrences of `/swarm` as a command reference in user-facing content.
- **SC-004**: Replicator project page exists and is accessible from the homepage and projects index.
- **SC-005**: `npm run build` succeeds with zero errors.
- **SC-006**: The word "Swarm" as a product name (capitalized, referring to the tool) appears zero times in user-facing content. The word "swarm" as a generic noun remains unchanged.

## Assumptions

- The Replicator README (53 MCP tools, 190+ tests, 15MB binary, <50ms startup) is the authoritative source for project facts.
- MCP tool names (`swarm_worktree_create`, `swarmmail_send`, etc.) do NOT change — they're API identifiers shared between old Swarm and Replicator.
- `[swarm]` execution mode labels, `define=swarm`, and `--define-mode=swarm` are workflow concepts, not product references, and stay unchanged.
- Historical spec artifacts (specs/002, 005, 010, 012) are planning records and will not be updated.
- The homepage Projects section will change from 2-column to 3-column grid (`col-lg-4`) to accommodate Replicator.
- The `.dewey/sources.yaml` change is a local config file (gitignored) but should still be done to clean up the dev environment.
