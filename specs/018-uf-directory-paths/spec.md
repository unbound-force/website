# Feature Specification: .uf/ Directory Path Update

**Feature Branch**: `018-uf-directory-paths`
**Created**: 2026-04-06
**Status**: Draft
**Input**: Update all directory references across the website for the `.uf/` consolidation (Spec 025). Covers content pages, internal agent/command files, and the local `.dewey/sources.yaml`.

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Content Pages Reference Correct Paths (Priority: P1)

A developer reads the website documentation and encounters directory paths like `.dewey/sources.yaml`, `.hive/`, and `.unbound-force/config.yaml`. These no longer exist — all tool data now lives under `.uf/`. Following these stale paths leads to confusion when the developer's actual filesystem doesn't match the documentation.

**Why this priority**: Stale filesystem paths are the most immediately confusing error a user can encounter. They try to edit a file at the documented path and it doesn't exist.

**Independent Test**: Search all content pages for `.dewey/`, `.hive/`, `.unbound-force/`, `.muti-mind/` — zero results. All replaced with `.uf/` equivalents.

**Acceptance Scenarios**:

1. **Given** a developer reading any documentation page, **When** they see a directory path referencing Dewey workspace files, **Then** the path uses `.uf/dewey/` (not `.dewey/`).
2. **Given** a developer reading workflow configuration docs, **When** they see the config file path, **Then** it reads `.uf/config.yaml` (not `.unbound-force/config.yaml`).
3. **Given** a developer reading the Replicator project page, **When** they see the workspace directory, **Then** it reads `.uf/replicator/` (not `.hive/`).

---

### User Story 2 — Internal Agent/Command Files Reference Correct Paths (Priority: P1)

Agent files and command files reference `.opencode/unbound/packs/` for convention packs. This directory was renamed to `.opencode/uf/packs/` in Spec 025. When agents try to load convention packs from the documented path, the path is wrong.

**Why this priority**: Agent files directly affect agent behavior. If an agent reads its own file and sees `.opencode/unbound/packs/`, it looks for packs at the wrong path.

**Independent Test**: Search all `.opencode/` files for `.opencode/unbound/` — zero results. All replaced with `.opencode/uf/`.

**Acceptance Scenarios**:

1. **Given** an agent loading convention packs, **When** it reads its agent file for the pack path, **Then** the path says `.opencode/uf/packs/` (not `.opencode/unbound/packs/`).

---

### Edge Cases

- The local `.dewey/sources.yaml` is gitignored and won't be committed, but it should be noted that users need to re-run `dewey init` or move their `.dewey/` directory to `.uf/dewey/` to match the new structure.
- The `.opencode/unbound/packs/` directory in this repo needs to be physically moved to `.opencode/uf/packs/` (not just the references updated). However, this may be handled by `uf init` — verify before moving files manually.

## Requirements _(mandatory)_

### Functional Requirements

**Content Pages (US1):**

- **FR-001**: All `.dewey/` path references in content pages MUST be replaced with `.uf/dewey/`.
- **FR-002**: All `.unbound-force/` path references MUST be replaced with `.uf/`.
- **FR-003**: All `.hive/` path references MUST be replaced with `.uf/replicator/`.
- **FR-004**: All `.muti-mind/` path references MUST be replaced with `.uf/muti-mind/`.

**Internal Files (US2):**

- **FR-005**: All `.opencode/unbound/` path references in agent files and command files MUST be replaced with `.opencode/uf/`.

**Cross-cutting:**

- **FR-006**: All changes MUST pass `npm run build` without errors.
- **FR-007**: Zero occurrences of `.dewey/`, `.hive/`, `.unbound-force/`, `.muti-mind/`, or `.opencode/unbound/` in content or `.opencode/` files (excluding historical specs).

## Success Criteria _(mandatory)_

- **SC-001**: Zero old-style directory references in user-facing content (17 `.dewey/` + 4 `.unbound-force/` + 1 `.hive/` + 2 `.muti-mind/` = 24 total replaced).
- **SC-002**: Zero `.opencode/unbound/` references in internal files (16 total replaced).
- **SC-003**: `npm run build` succeeds with zero errors.

## Assumptions

- Spec 025 has landed in the meta repo (confirmed: commit `4140b0c`, 80/80 tasks complete).
- Dewey's code uses `.uf/dewey/` (confirmed: README references `.uf/dewey/graph.db`).
- Replicator's code uses `.uf/replicator/` (confirmed: README says "creates .uf/replicator/ directory").
- The `.opencode/unbound/packs/` directory in this repo will be physically moved to `.opencode/uf/packs/` by running `uf init` (which handles the migration). If `uf init` has not been run on this repo yet, the physical move is out of scope for this spec — only the documentation references are updated.
- Historical spec artifacts (specs/002, 005, 010, etc.) are NOT updated — they document what was true when they were written.
