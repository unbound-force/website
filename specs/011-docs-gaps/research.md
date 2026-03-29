# Research: Documentation Gaps

**Date**: 2026-03-29
**Feature**: [spec.md](spec.md) | [plan.md](plan.md)

## R1: Constitution Page Content Structure

**Decision**: Adapt the org constitution v1.1.0 for a website audience, using the same principle names and MUST/SHOULD structure but with web-oriented framing.

**Source**: `.specify/memory/constitution.md` (org repo) at v1.1.0.

**Content outline** (adapted from upstream):

1. **Introduction** -- What the constitution is, why it matters, its authority
2. **Principle I: Autonomous Collaboration** -- Artifact-based communication, self-describing outputs, envelope format, well-known publication locations. MUST rules: no synchronous dependencies, self-describing artifacts with metadata, use envelope format. SHOULD rules: publish to well-known locations.
3. **Principle II: Composability First** -- Independent installability, extension points, additive value. MUST rules: standalone core value, extension points over modification, additive combination value. SHOULD rules: auto-detect peers.
4. **Principle III: Observable Quality** -- Machine-parseable output, provenance, reproducible evidence. MUST rules: JSON format minimum, provenance metadata, automated evidence, stable formats. SHOULD rules: conform to shared schemas.
5. **Principle IV: Testability** -- Isolation, observable side effects, coverage strategy. MUST rules: test without external services, verify side effects not internals, define coverage strategy, enforce coverage ratchets. Missing coverage = CRITICAL finding.
6. **Hero Constitution Alignment** -- `parent_constitution` reference, additive-only principles, amendment review obligations.
7. **Governance** -- Supremacy clause, amendment process (PR + review + migration plan), semantic versioning (MAJOR/MINOR/PATCH definitions), compliance review at planning phases, conflict resolution.
8. **Constitution Check** -- How it gates the planning phase, link to the `/constitution-check` command, role in proposal templates.

**Documentation approach**: The constitution content is inherently structured and rule-based. Present it faithfully -- this is reference documentation, not marketing. Use the principle headings directly (I, II, III, IV) and list the MUST/SHOULD rules clearly. Include the rationale paragraphs from the upstream document since they explain the "why" to website visitors. Cross-link to the artifacts page for envelope format details (Constitution Principle I references artifacts).

**Alternatives considered**: Creating a simplified "values" page without MUST/SHOULD specifics was rejected because contributors need the actual rules to align their hero constitutions. The rules are the value.

## R2: Artifacts Page Content Structure

**Decision**: Document the 7 artifact types and envelope format as a reference page with a lifecycle stage mapping.

**Source**: `AGENTS.md` "Inter-Hero Artifact Types" section; `specs/009-shared-data-model/` contracts.

**Content outline**:

1. **Introduction** -- Heroes communicate through artifacts (Constitution Principle I). Artifacts are files with a standard envelope format.
2. **Envelope Format** -- The 7 standard fields: `hero` (producing hero name), `version` (hero version), `timestamp` (ISO 8601), `artifact_type` (one of the 7 types), `schema_version` (semver of the schema), `context` (branch, commit, backlog item reference), `payload` (artifact-specific data).
3. **Artifact Types** -- Table and brief descriptions:
   - `quality-report` (Gaze -> Mx F, Muti-Mind, Cobalt-Crush): Test results, coverage data, CRAP scores, risk analysis
   - `review-verdict` (The Divisor -> Mx F, Cobalt-Crush, Muti-Mind): APPROVE/REQUEST CHANGES with per-persona findings
   - `backlog-item` (Muti-Mind -> Mx F, Cobalt-Crush): Feature/bug descriptions with priority scores and acceptance criteria
   - `acceptance-decision` (Muti-Mind -> Mx F, Cobalt-Crush): ACCEPT/REJECT/CONDITIONAL with rationale
   - `metrics-snapshot` (Mx F -> Muti-Mind): Velocity, quality trends, review efficiency, CI health
   - `coaching-record` (Mx F -> All heroes): Learning feedback, recommendations, convention pack updates
   - `workflow-record` (Swarm Orchestration -> Mx F, Muti-Mind): Complete workflow trace with stage timings and artifacts
4. **Lifecycle Stage Mapping** -- Which artifacts are produced at each of the 6 lifecycle stages:
   - Define: `backlog-item`
   - Implement: (code artifacts, not envelope-formatted)
   - Validate: `quality-report`
   - Review: `review-verdict`
   - Accept: `acceptance-decision`
   - Reflect: `metrics-snapshot`, `coaching-record`, `workflow-record`

**Documentation approach**: The artifacts page is technical reference -- developers need the specific field names, types, and mappings. Use tables for the type/producer/consumer mapping and the lifecycle mapping. Cross-link to common-workflows for the lifecycle stages, and to the constitution page for Principle I (Autonomous Collaboration).

**Alternatives considered**: Including a sample JSON envelope was considered but deferred. The envelope format is specified in the Hero Interface Contract spec (002), and including JSON in the website documentation risks going stale if the schema changes. Instead, describe the fields narratively and link to the source.

## R3: Workflow Commands (status, list, advance)

**Decision**: Add a "Workflow Management" subsection to common-workflows.md after the existing "Workflow Commands" section.

**Source**: `.opencode/command/workflow-status.md`, `workflow-list.md`, `workflow-advance.md`.

**Content for each command**:

### `/workflow status`

- **Usage**: `/workflow status [workflow-id]`
- Auto-detects workflow from current git branch if no ID provided
- Displays: workflow ID, branch, backlog item, status, start time, iteration count
- Stage status indicators: `✓` completed, `◉` active, `○` pending, `⏸` awaiting human, `⊘` skipped, `✗` failed
- Per-stage details: execution mode (`[human]`/`[swarm]`), hero name, elapsed time
- Lists artifacts produced so far
- If awaiting human: shows `⏸` indicator and "Run /workflow advance to resume"

### `/workflow list`

- **Usage**: `/workflow list [--status active|completed|all]`
- Lists all workflows as a table: ID, Branch, Status, Started
- Sorted by start time (most recent first)
- Default: shows all workflows

### `/workflow advance`

- **Usage**: `/workflow advance [workflow-id]`
- **Normal advance**: Completes current stage, activates next non-skipped stage
- **Checkpoint handling**: If completed stage is swarm-mode and next is human-mode, pauses workflow (status: `awaiting_human`)
- **Resume**: If workflow is awaiting_human, activates the next pending human-mode stage
- **Escalation**: If review stage and iteration_count >= 3, escalates to human review
- **Completion**: If no more stages, marks workflow "completed" and generates `workflow-record` artifact

**Documentation approach**: Keep it concise -- usage syntax, brief behavior description, and status indicators. This is reference documentation, not tutorial. Place after the existing "Workflow Commands" and "Workflow Configuration" sections in common-workflows.md.

## R4: Convention Pack Documentation

**Decision**: Expand the existing convention packs section in developer.md from ~4 lines to a comprehensive subsection.

**Source**: `.opencode/unbound/packs/` (6 files); `internal/scaffold/scaffold.go` (ownership model, language detection).

**Content structure**:

1. **What Convention Packs Are** -- Shared coding standards files that Cobalt-Crush follows during implementation and The Divisor enforces during review.
2. **Pack Files** -- Table of 6 files with their purpose:
   - `default.md` (tool-owned): Language-agnostic rules -- coding style, architecture, security, testing, documentation
   - `default-custom.md` (user-owned): Project-specific conventions extending the default pack
   - `go.md` (tool-owned): Go-specific rules -- gofmt, error handling, GoDoc, Cobra patterns
   - `go-custom.md` (user-owned): Project-specific Go conventions
   - `typescript.md` (tool-owned): TypeScript-specific rules -- ESLint, Prettier, strict typing, architectural patterns
   - `typescript-custom.md` (user-owned): Project-specific TypeScript conventions
3. **Ownership Model**: Tool-owned files are updated by `uf init` when the embedded version changes. User-owned files (`*-custom.md`) are never overwritten -- your customizations are preserved across updates.
4. **Rule Severity Tags**: `[MUST]` (mandatory, violations block review), `[SHOULD]` (strong recommendations), `[MAY]` (optional improvements). These tags map directly to review finding severity.
5. **Language Auto-Detection**: `uf init` detects language from marker files: `go.mod` -> Go, `tsconfig.json` or `package.json` -> TypeScript. Only the matching language pack (plus default) is deployed. Override with `--lang`.
6. **Adding Custom Rules**: Edit the `*-custom.md` files. Use `CR-NNN` prefix for custom rule IDs. Rules are loaded by Cobalt-Crush and all Divisor personas during their workflows.

**Documentation approach**: This replaces the current ~4 lines. Keep it practical -- developers need to know what files exist, how to customize, and what happens during review. Don't reproduce the full pack content (that would violate the website constitution's Content Accuracy principle if packs change). Describe the structure and link to the actual files.

## R5: `uf init` Standalone Documentation

**Decision**: Add a dedicated `uf init` subsection to developer.md, after the convention packs section.

**Source**: `internal/scaffold/scaffold.go` (Run function, Options struct, printSummary, detectLang, isToolOwned).

**Content structure**:

1. **What `uf init` Does** -- Scaffolds project files for working with the Unbound Force swarm. Deploys agents, commands, templates, scripts, convention packs, OpenSpec schema, and skills.
2. **Usage**: `uf init [--divisor] [--lang go|typescript] [--force]`
3. **Flags**:
   - `--divisor`: Deploy only PR review agents (Divisor personas) and convention packs. Useful for projects that only want code review, not the full swarm workflow.
   - `--lang`: Override language auto-detection for convention pack selection. Auto-detects from `go.mod`, `tsconfig.json`, `package.json`, `pyproject.toml`, `Cargo.toml`.
   - `--force`: Overwrite user-owned files (normally skipped). Use with caution.
4. **File Categories** -- What gets deployed:
   - Agents (12 files): 5 Divisor personas, Cobalt-Crush, Constitution Check, Mx F Coach, 4 reviewer agents
   - Commands (15 files): 9 Speckit commands, review-council, constitution-check, cobalt-crush, finale, uf-init, and more
   - Convention Packs (6 files): default, go, typescript (each with tool-owned and user-owned variants)
   - Templates (6 files): spec, plan, tasks, checklist, constitution, agent-file templates
   - Scripts (5 files): check-prerequisites, setup-plan, create-new-feature, update-agent-context, common
   - OpenSpec (6 files): config, schema, 4 templates
5. **File Ownership Model**:
   - Tool-owned: Commands, skills, canonical packs, OpenSpec schemas. Automatically updated when content differs from the embedded version. Version marker: `<!-- scaffolded by uf v{version} -->`
   - User-owned: Agent files, custom packs (`*-custom.md`), specify configs. Never overwritten on re-run (skipped if exists).
6. **Sub-Tool Initialization**: Creates `.unbound-force/config.yaml` (workflow configuration). If Dewey is available, also creates `.dewey/` workspace, auto-detects multi-repo sources, and builds the initial index.
7. **Summary Output**: Shows file dispositions (`+` created, `~` updated, `!` overwritten, `-` skipped) and context-aware next-step guidance.

**Documentation approach**: Focus on the practical -- what the flags do, what gets deployed, and the ownership model. The file counts (51 files, 12 agents, etc.) are useful for orientation but should be framed as approximate since the number changes between versions. Don't enumerate every single file -- describe categories with representative examples.

## R6: Navigation Configuration

**Decision**: Add sidebar entries for the constitution and artifacts pages to `config/_default/menus/menus.en.toml`.

**Source**: Existing `menus.en.toml` structure.

The new pages should have `weight` values that place them logically in the sidebar:

- Constitution: Between the existing team/knowledge pages and the contributing page. It's governance content -- related to contributing but more foundational.
- Artifacts: Near the common-workflows page, since artifacts flow through the lifecycle stages.

The exact weight values will be determined during implementation by examining the existing weights in the menu config. The Doks theme handles sidebar rendering automatically from the `weight` frontmatter in each `.md` file -- no explicit menu entries are needed if the files are in the correct directory with the correct weight.

**Rationale**: Doks auto-generates sidebar navigation from the directory structure and frontmatter weights. Adding files to `content/docs/getting-started/` with appropriate weights will make them appear in the sidebar automatically. Menu entries in `menus.en.toml` are only needed for top-level navigation items, not sidebar entries.

**Alternatives considered**: Creating a new "Architecture" top-level section was considered but rejected to keep the navigation flat and simple per the Minimal Footprint principle.
