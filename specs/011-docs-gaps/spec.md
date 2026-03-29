# Feature Specification: Documentation Gaps

**Feature Branch**: `011-docs-gaps`
**Created**: 2026-03-29
**Status**: Draft
**Input**: Address website documentation gaps identified in the gaps audit: constitution page, artifact types page, workflow command completeness, convention pack depth, and uf init documentation.

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Constitution and Governance Page (Priority: P1)

A visitor wants to understand the foundational principles that govern all Unbound Force heroes. Currently, the 4 core organizational principles (Autonomous Collaboration, Composability First, Observable Quality, Testability) exist in the source repository but have no dedicated page on the website. The visitor needs to find these principles, understand what each one requires (MUST/SHOULD rules), and learn how governance works (amendments, versioning, hero constitution alignment).

**Why this priority**: The constitution is the highest-authority document in the organization. Every hero constitution must align with it. Every proposal includes a constitution alignment assessment. Contributors cannot work effectively with the system without understanding these principles. This is the largest content gap -- no existing page addresses it even partially.

**Independent Test**: Navigate to the constitution page from the sidebar. Verify that all 4 principles are described with their MUST/SHOULD rules. Verify the governance section covers amendments, versioning, supremacy clause, and hero constitution alignment. Cross-reference against the source `.specify/memory/constitution.md` to confirm accuracy.

**Acceptance Scenarios**:

1. **Given** a visitor browsing the documentation, **When** they look for the governance model, **Then** they find a constitution page accessible within two clicks from the homepage that describes all 4 core principles.
2. **Given** a contributor reading the constitution page, **When** they look for a specific principle's requirements, **Then** each principle has clearly labeled MUST and SHOULD rules with rationale.
3. **Given** a contributor building a new hero, **When** they check the constitution page for alignment requirements, **Then** they find the hero constitution alignment section explaining `parent_constitution` references, additive-only hero principles, and amendment review obligations.
4. **Given** a contributor proposing a constitution change, **When** they read the governance section, **Then** they find the amendment process, semantic versioning scheme (MAJOR/MINOR/PATCH definitions), supremacy clause, and conflict resolution guidance.

---

### User Story 2 — Artifact Types and Envelope Format Page (Priority: P1)

A developer or contributor wants to understand how heroes communicate with each other. Currently, the 7 inter-hero artifact types and the standard envelope format are central to the swarm's operation but completely undocumented on the website. The visitor needs to understand what artifacts exist, who produces and consumes each one, and what the envelope format looks like.

**Why this priority**: Artifacts are the backbone of hero communication (Constitution Principle I: Autonomous Collaboration). Anyone building a new hero, integrating with existing heroes, or understanding the data flow through the swarm needs this information. The common-workflows page describes the 6-stage lifecycle but not the artifacts that flow between stages.

**Independent Test**: Navigate to the artifacts page from the sidebar. Verify that all 7 artifact types are listed with their producer/consumer mappings. Verify the envelope format fields are described. Cross-reference against the source `AGENTS.md` "Inter-Hero Artifact Types" section.

**Acceptance Scenarios**:

1. **Given** a developer reading the artifacts page, **When** they look for how heroes communicate, **Then** they find the standard envelope format with all fields described (hero, version, timestamp, artifact_type, schema_version, context, payload).
2. **Given** a developer reading the artifacts page, **When** they look for a specific artifact type, **Then** they find all 7 types listed with their producer hero, consumer heroes, and a brief description of what each artifact contains.
3. **Given** a developer reading the artifacts page, **When** they want to understand data flow, **Then** they find a mapping of which artifacts are produced at each stage of the 6-stage hero lifecycle.

---

### User Story 3 — Complete Workflow Command Documentation (Priority: P2)

A user wants to manage and monitor hero lifecycle workflows. The website documents `/workflow seed` and `/workflow start` but is missing `/workflow status`, `/workflow list`, and `/workflow advance`. The user needs to know how to check workflow progress, list all workflows, and manually advance a workflow past a human checkpoint.

**Why this priority**: The existing workflow command documentation covers starting workflows but not monitoring or advancing them. Users who start a workflow cannot check its status or advance it past human checkpoints without reading the source command files. This is a completeness gap in already-documented functionality.

**Independent Test**: Search the common-workflows page for `/workflow status`, `/workflow list`, and `/workflow advance` -- all must appear with usage syntax and behavior descriptions.

**Acceptance Scenarios**:

1. **Given** a user reading the common-workflows page, **When** they look for how to check workflow progress, **Then** they find `/workflow status` with usage syntax and a description of the stage status indicators.
2. **Given** a user reading the common-workflows page, **When** they look for how to list all workflows, **Then** they find `/workflow list` with usage syntax and the `--status` filter flag.
3. **Given** a user reading the common-workflows page, **When** they look for how to advance a workflow past a human checkpoint, **Then** they find `/workflow advance` with usage syntax and descriptions of checkpoint, escalation, and completion behavior.

---

### User Story 4 — Convention Pack Documentation (Priority: P2)

A developer wants to understand how coding conventions are managed and enforced by the swarm. The developer guide currently mentions convention packs in approximately 4 lines. The developer needs to know what pack files exist, how the ownership model works (tool-owned vs. user-owned), how to customize conventions, and how packs are loaded during review.

**Why this priority**: Convention packs directly affect the developer's daily experience -- they determine what The Divisor flags during review and what Cobalt-Crush follows during implementation. The current 4-line mention is insufficient for a developer to effectively use or customize them.

**Independent Test**: Read the convention packs section and verify it describes all 6 pack files, the ownership model, language auto-detection, customization process, and how The Divisor loads packs.

**Acceptance Scenarios**:

1. **Given** a developer reading the convention packs section, **When** they look for what packs exist, **Then** they find the 6 pack files listed (default, go, typescript -- each with a tool-owned and user-owned variant) with their purpose.
2. **Given** a developer reading the convention packs section, **When** they look for how to customize conventions, **Then** they find instructions for editing `*-custom.md` files, the rule severity tags (MUST/SHOULD/MAY), and assurance that user-owned files are never overwritten by `uf init`.
3. **Given** a developer reading the convention packs section, **When** they look for how packs are selected, **Then** they find the language auto-detection behavior and the `--lang` override flag.

---

### User Story 5 — `uf init` Standalone Documentation (Priority: P3)

A developer wants to understand what `uf init` does independently of `uf setup`. The website mentions `uf init` as part of the setup flow but does not document its standalone capabilities: the 50 scaffolded files, the `--divisor` flag, the `--lang` flag, and the file ownership model.

**Why this priority**: While most users encounter `uf init` through `uf setup`, advanced users may need to run it independently (e.g., re-scaffolding, initializing a new project without full setup). The gap is minor since the primary use case is covered, but the standalone flags and behavior are not documented anywhere.

**Independent Test**: Read the `uf init` documentation section and verify it describes the scaffolded files, the `--divisor` and `--lang` flags, and the file ownership model.

**Acceptance Scenarios**:

1. **Given** a developer reading the `uf init` documentation, **When** they look for what it scaffolds, **Then** they find a description of the file categories deployed (agents, commands, templates, scripts, convention packs, OpenSpec schema, skills).
2. **Given** a developer reading the `uf init` documentation, **When** they look for selective initialization, **Then** they find the `--divisor` flag (deploy only PR review agents and convention packs) and the `--lang` flag (override language auto-detection).
3. **Given** a developer reading the `uf init` documentation, **When** they look for how re-initialization works, **Then** they find the file ownership model: tool-owned files (auto-updated with version markers) vs. user-owned files (never overwritten).

---

### Edge Cases

- What happens when the constitution page references concepts (like artifacts) that have their own dedicated page? The page must cross-link to the artifacts page rather than duplicating content.
- What happens when a visitor reads the artifacts page without understanding the 6-stage lifecycle? The page must either provide a brief lifecycle summary or link to the common-workflows page.
- What happens when convention pack file names change in a future version? The documentation must describe the pack structure generically enough to remain accurate, while still naming the current packs specifically.
- How should the sidebar navigation handle the new pages? The new pages must be discoverable within two clicks from the homepage per the website constitution's Visitor Clarity principle.

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The website MUST have a constitution page describing all 4 organizational principles (Autonomous Collaboration, Composability First, Observable Quality, Testability) with their MUST and SHOULD rules.
- **FR-002**: The constitution page MUST include a governance section covering amendments, semantic versioning, supremacy clause, hero constitution alignment, and conflict resolution.
- **FR-003**: The constitution page MUST describe the Constitution Check process and how it gates the planning phase.
- **FR-004**: The website MUST have an artifacts page describing the standard envelope format with all 7 fields (hero, version, timestamp, artifact_type, schema_version, context, payload).
- **FR-005**: The artifacts page MUST list all 7 artifact types with their producer hero, consumer heroes, and a brief description of what each artifact contains.
- **FR-006**: The artifacts page MUST include a lifecycle stage mapping showing which artifacts are produced and consumed at each of the 6 stages.
- **FR-007**: The common-workflows page MUST document `/workflow status` with usage syntax, stage status indicators, and output description.
- **FR-008**: The common-workflows page MUST document `/workflow list` with usage syntax, the `--status` filter flag, and output format.
- **FR-009**: The common-workflows page MUST document `/workflow advance` with usage syntax, checkpoint behavior, escalation rules, and completion behavior.
- **FR-010**: The developer guide MUST include an expanded convention packs section describing: all 6 pack files and their purpose, the ownership model (tool-owned vs. user-owned), rule severity tags (MUST/SHOULD/MAY), language auto-detection and the `--lang` override, and how to add custom rules.
- **FR-011**: The developer guide or common-workflows page MUST document `uf init` standalone capabilities: the file categories scaffolded, the `--divisor` flag, the `--lang` flag, and the file ownership model (tool-owned vs. user-owned, version markers, skip-on-re-run behavior).
- **FR-012**: The sidebar navigation MUST include entries for the constitution and artifacts pages, accessible within two clicks from the homepage.
- **FR-013**: All new pages MUST include required YAML frontmatter (title, description, lead, date, draft, weight, toc).
- **FR-014**: All new content MUST cross-link to related existing pages rather than duplicating content.
- **FR-015**: All documentation changes MUST pass `npm run build` without errors.

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: Every organizational principle (4 total) is documented on a dedicated page with all MUST/SHOULD rules -- zero undocumented principles.
- **SC-002**: Every artifact type (7 total) is documented with producer/consumer mappings -- zero undocumented artifact types.
- **SC-003**: Every `/workflow` command (seed, start, status, list, advance) appears on at least one user-facing documentation page with usage syntax -- zero undocumented workflow commands.
- **SC-004**: The convention packs section describes all 6 pack files, the ownership model, and the customization process -- zero undocumented packs.
- **SC-005**: `npm run build` succeeds with zero errors after all documentation changes.
- **SC-006**: All new pages are accessible within two clicks from the homepage via sidebar navigation.
- **SC-007**: Website completeness relative to implemented features improves from ~80% to ~95% (as measured by the gaps report criteria).

## Assumptions

- The org constitution v1.1.0 (`.specify/memory/constitution.md` in the unbound-force repo) is the authoritative source for principles and governance.
- The AGENTS.md "Inter-Hero Artifact Types" section is the authoritative source for artifact type definitions and envelope format.
- The workflow command files (`.opencode/command/workflow-*.md`) are the authoritative source for command syntax and behavior.
- The `.opencode/unbound/packs/` directory is the authoritative source for convention pack file names and structure.
- `internal/scaffold/scaffold.go` is the authoritative source for `uf init` behavior, flags, and file ownership model.
- New pages will be placed under `content/docs/getting-started/` to maintain the existing section structure, rather than creating a new "Architecture" section. This keeps navigation simple and avoids the need for menu restructuring. The constitution page sits naturally alongside the existing governance-adjacent content (contributing page), and the artifacts page sits alongside the workflow documentation.
- Convention pack documentation will be an expanded section within the existing `developer.md` page, not a separate page, to avoid over-fragmenting the developer guide.
- `uf init` documentation will be added as a new subsection in the developer guide, consistent with where other CLI command documentation lives.
