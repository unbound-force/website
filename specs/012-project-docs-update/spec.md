# Feature Specification: Project Documentation Update

**Feature Branch**: `012-project-docs-update`
**Created**: 2026-03-30
**Status**: Ready
**Input**: User description: "Update all project documentation to match current tool versions -- Gaze v1.4.7, Dewey v1.4.2, and swarm workflow features (unleash, finale, review council, Divisor refinements). Combined spec from three audit documents: website-documentation-gaps.md, gaze-website-docs-audit.md, and dewey-gaps-2026-03-30.md."

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Fix Critical Gaze Errors (Priority: P1)

A developer reads the Gaze project page to understand how to set up and use the tool. The page currently contains 3 critical factual errors: it says `gaze init` creates 4 files (actually 8), it references a `/classify-docs` command that was removed, and it states assertion mapping accuracy as 78.8% (actually 84.7%). These errors cause immediate confusion when the developer's actual experience doesn't match the documentation.

**Why this priority**: Factually wrong content on a live website is the highest-severity gap. Users following the setup instructions will see 8 files, not 4, and will search in vain for a `/classify-docs` command that no longer exists. This directly undermines trust.

**Independent Test**: Read `projects/gaze.md` and verify: (a) `gaze init` file count matches the actual tool's output, (b) no references to `/classify-docs` exist anywhere on the site, (c) assertion accuracy reads "~84.7%", (d) the architecture table includes the `aireport` package.

**Acceptance Scenarios**:

1. **Given** a developer reading the Gaze setup section, **When** they look for what `gaze init` creates, **Then** the documentation lists 8 files across 3 directories matching the actual tool's output.
2. **Given** a developer searching the site for `/classify-docs`, **When** they search across all pages, **Then** zero results are found -- all references to the removed command have been eliminated.
3. **Given** a developer reading the Current Limitations section, **When** they check the assertion mapping accuracy, **Then** it reads "~84.7%" with the 90% target still noted.
4. **Given** a developer reading the Architecture section, **When** they look for the AI integration package, **Then** `aireport` appears in the package table with a description of the AI CLI adapter.

---

### User Story 2 — Add Missing Gaze Features (Priority: P1)

A developer wants to use Gaze's newer capabilities but the website doesn't document them. The `--ai=opencode` adapter (only `--ai=claude` is shown), `--coverprofile` flag (eliminates CI double-test-run), `/gaze fix` command (AI-assisted test generation), and fix strategy labels (remediation guidance per function) are all absent from the website.

**Why this priority**: These are major features that users cannot discover from the website. The `--coverprofile` flag in particular saves significant CI time, and `/gaze fix` is a new command that enables a complete test remediation workflow. Documenting these alongside the error fixes (US1) ensures the Gaze pages are both accurate and complete.

**Independent Test**: Read `projects/gaze.md` and `tester.md` and verify: (a) `--ai=opencode` appears as an adapter option, (b) `--coverprofile` appears in the CLI usage and CI integration pattern, (c) `/gaze fix` command is documented with its workflow, (d) fix strategy labels are explained.

**Acceptance Scenarios**:

1. **Given** a developer reading the Gaze CLI usage, **When** they look for AI adapter options, **Then** both `--ai=opencode` and `--ai=claude` are listed as supported backends.
2. **Given** a developer reading the CI integration pattern, **When** they look for how to avoid running tests twice, **Then** they find the `--coverprofile` flag with the recommended pattern: run `go test -coverprofile=coverage.out` separately, then pass the profile to `gaze report`.
3. **Given** a developer reading about test remediation, **When** they look for how to generate missing tests, **Then** they find the `/gaze fix` command with a description of the AI-assisted test generation workflow.
4. **Given** a developer reading a Gaze quality report, **When** they look for what to do about a high-CRAP function, **Then** they find fix strategy labels explained (`add_tests`, `add_assertions`, `decompose`, `decompose_and_test`).

---

### User Story 3 — Update Gaze Minor Items (Priority: P2)

A developer reads the Gaze documentation and encounters stale numbers and imprecise limitation descriptions. The sample output references v1.2.3 (current is v1.4.7), the CI example uses an unrealistically low `--max-crapload=15` threshold, the "single package loading" limitation is overly broad, and the P3-P4 detection text implies P2 is also undetected.

**Why this priority**: These are not factually wrong (unlike US1) but they erode precision. A developer who sets `--max-crapload=15` will have a failing CI pipeline on most real projects. The imprecise limitation descriptions may cause users to avoid features that actually work.

**Independent Test**: Read the Gaze pages and verify: (a) version references are updated or contextualized, (b) CI threshold example uses a realistic starting value, (c) single-package limitation is scoped to `analyze` and `quality` only, (d) P3-P4 text does not imply P2 is undetected.

**Acceptance Scenarios**:

1. **Given** a developer reading the CI integration pattern, **When** they look at the `--max-crapload` example, **Then** the value is realistic (e.g., 50) with a note that it depends on the project.
2. **Given** a developer reading the Current Limitations, **When** they check single-package loading, **Then** it is scoped to `gaze analyze` and `gaze quality` only (not `gaze crap` or `gaze report`).
3. **Given** a developer reading the Current Limitations, **When** they check side effect detection tiers, **Then** the text clearly states P2 IS detected and only P3-P4 are undetected.

---

### User Story 4 — Update Dewey Source Configuration and Ignore Behavior (Priority: P1)

A user configures Dewey content sources but the documentation doesn't mention `.gitignore` support (shipped today in Spec 006), the `ignore` field for custom patterns, or the `recursive` field for controlling indexing depth. The documentation also claims Dewey "indexes all Markdown files including hidden directories," which is no longer accurate.

**Why this priority**: The `.gitignore` support was just merged. Without this documentation, users with `node_modules/`, `vendor/`, or other large gitignored directories won't know Dewey automatically skips them. Users also cannot discover the new `ignore` and `recursive` configuration fields.

**Independent Test**: Read `knowledge.md` and verify: (a) the Local Disk source section mentions automatic `.gitignore` respect, (b) the YAML example includes `ignore` and `recursive` fields, (c) the claim about indexing "all Markdown files" is updated to reflect `.gitignore` filtering.

**Acceptance Scenarios**:

1. **Given** a user reading the Local Disk source configuration, **When** they look for how to exclude directories, **Then** they find that `.gitignore` at the source root is automatically respected, with no configuration needed.
2. **Given** a user reading the YAML configuration reference, **When** they look for custom ignore patterns, **Then** they find the `ignore` field with syntax documentation and examples.
3. **Given** a user reading the YAML configuration reference, **When** they want to limit indexing to top-level files only, **Then** they find the `recursive: false` field.

---

### User Story 5 — Add Missing Dewey CLI Commands and Troubleshooting (Priority: P2)

A user is troubleshooting a Dewey issue (slow startup, missing embeddings, stale index) but the documentation doesn't mention `dewey doctor` (diagnostics), `dewey reindex` (clean rebuild), or the global debugging flags (`--verbose`, `--log-file`). There is no troubleshooting guidance at all.

**Why this priority**: `dewey doctor` is the primary diagnostic tool but is not mentioned anywhere on the website. Users troubleshooting MCP timeouts or missing search results have no documented path to resolution. The `--verbose` flag was already added to the MCP config but not explained.

**Independent Test**: Read `knowledge.md` and verify: (a) `dewey doctor` is documented with its diagnostic sections, (b) `dewey reindex` is documented for clean index rebuilds, (c) global CLI flags (`--verbose`, `--log-file`, `--no-embeddings`, `--vault`) are documented, (d) a troubleshooting section covers common issues with resolution steps.

**Acceptance Scenarios**:

1. **Given** a user experiencing Dewey issues, **When** they look for diagnostic tools, **Then** they find `dewey doctor` with a description of what it checks (environment, workspace, database, sources, embeddings, MCP server).
2. **Given** a user with a corrupted index, **When** they look for how to rebuild, **Then** they find `dewey reindex` with a description of its behavior (deletes graph.db and re-indexes from scratch).
3. **Given** a user troubleshooting MCP timeouts, **When** they look for debugging options, **Then** they find a troubleshooting section with common issues and resolution steps, including `--verbose`, `--log-file`, and `.gitignore` checks.

---

### User Story 6 — Document /unleash and /finale Commands (Priority: P2)

A developer wants to use the flagship autonomous pipeline (`/unleash`) or the end-of-branch workflow (`/finale`) but neither command is documented on the website. These are the two most impactful workflow commands -- unleash builds, finale ships -- and together they represent the complete development loop.

**Why this priority**: `/unleash` is the single-command autonomous pipeline (Spec 018) and `/finale` automates commit-push-PR-merge. Both were shipped recently and represent the primary entry points for the swarm-powered workflow. Without documentation, developers won't discover these capabilities.

**Independent Test**: Read `common-workflows.md` and verify: (a) `/unleash` is documented with its 8-step pipeline, exit points, and resumability, (b) `/finale` is documented with its 9-step workflow, CI watching, and branch safety.

**Acceptance Scenarios**:

1. **Given** a developer reading the common workflows page, **When** they look for the autonomous pipeline, **Then** they find `/unleash` with a description of the 8 steps (clarify, plan, tasks, spec review, implement, code review, retrospective, demo), exit points, Dewey-powered clarification, parallel Swarm workers, and how to resume.
2. **Given** a developer reading the common workflows page, **When** they look for how to finalize a branch, **Then** they find `/finale` with a description of the workflow (stage, commit, push, PR, watch checks, merge, return to main), branch safety guardrails, and the relationship to `/unleash` (unleash builds, finale ships).

---

### User Story 7 — Document Review Council CI Gate and Divisor Refinements (Priority: P3)

A developer or contributor wants to understand the review council's CI hard gate (Phase 1a: lint + vuln check) and Gaze quality analysis (Phase 1b: CRAP scores passed to Divisor agents). The developer guide also needs to reflect the Divisor council refinements from Spec 019: the shared severity standard (`severity.md`), exclusive ownership boundaries, and prior learnings integration.

**Why this priority**: These are enhancements to existing documented features, not new missing features. The review council and Divisor persona documentation exists but is incomplete. The severity pack adds a 7th convention pack file that our documentation currently says is 6.

**Independent Test**: Read `common-workflows.md`, `developer.md`, and `the-divisor.md` and verify: (a) review council documentation covers Phase 1a (CI) and Phase 1b (Gaze), (b) convention packs section lists 7 files including `severity.md`, (c) Divisor team page mentions exclusive ownership, severity standard, and prior learnings.

**Acceptance Scenarios**:

1. **Given** a developer reading the code review workflow, **When** they look at what the review council does before delegating to Divisor agents, **Then** they find Phase 1a (CI commands derived from workflow files, including lint and vulnerability check) and Phase 1b (Gaze quality analysis passed as context to reviewers).
2. **Given** a developer reading the convention packs section, **When** they count the pack files, **Then** they find 7 files listed (6 existing + `severity.md`) with the severity pack described as the shared calibration standard.
3. **Given** a developer reading the Divisor team page, **When** they look at how personas avoid duplicate findings, **Then** they find the exclusive ownership model (each review dimension owned by exactly one persona) and prior learnings integration (Hivemind search before review).

---

### User Story 8 — Update uf init and uf setup Documentation (Priority: P3)

A developer reads the setup documentation but it doesn't reflect that `opencode.json` management moved from `uf setup` to `uf init` (Spec 017), reducing setup from 16 to 13 steps. The `uf init` section also doesn't mention `opencode.json` creation or the `--force` flag's effect on Dewey re-indexing.

**Why this priority**: This is a process change that doesn't cause errors (users who run `uf setup` still get everything working) but is factually inaccurate. The step count and ownership details should be correct.

**Independent Test**: Read `common-workflows.md` and `developer.md` and verify: (a) `uf setup` step count reflects current behavior (13 steps, not 16), (b) `uf init` documentation mentions `opencode.json` management, (c) `--force` flag description includes Dewey re-indexing behavior.

**Acceptance Scenarios**:

1. **Given** a developer reading the setup documentation, **When** they count the setup steps, **Then** the number reflects the current `uf setup` behavior after the `opencode.json` move.
2. **Given** a developer reading the `uf init` documentation, **When** they look for `opencode.json` management, **Then** they find that `uf init` creates/updates `opencode.json` with Dewey MCP server entry (when dewey is in PATH) and Swarm plugin entry (when `.hive/` exists).

---

### Edge Cases

- What happens when Gaze documentation references features that require a specific minimum version? Add version context (e.g., "added in v1.3.0") for features that may not be available in older installations.
- What happens when a user reads the Dewey troubleshooting section but doesn't have Ollama installed? The troubleshooting section must cover the Ollama-not-running scenario with clear resolution steps.
- What happens when a user reads about `/unleash` but doesn't have Dewey configured? The documentation must describe graceful degradation (falls back to human-driven clarification).
- What happens when the convention pack count changes again in a future version? This spec intentionally hardcodes "7 files" as the current count (FR-022, SC-006). Future specs that add or remove packs should update all references. The implementation should describe the pack structure alongside the count so readers understand the pattern, not just the number.

## Requirements _(mandatory)_

### Functional Requirements

**Gaze (US1-US3):**

- **FR-001**: The Gaze project page MUST state that `gaze init` creates 8 files across 3 directories (agents, commands, references) with each file listed.
- **FR-002**: All references to the `/classify-docs` command MUST be removed from the entire website.
- **FR-003**: The assertion mapping accuracy MUST read "~84.7%" with the ratchet floor of 84.0% noted.
- **FR-004**: The architecture table MUST include the `aireport` package.
- **FR-005**: The Gaze project page and tester guide MUST document `--ai=opencode` as a supported AI adapter alongside `--ai=claude`.
- **FR-006**: The tester guide CI integration pattern MUST document the `--coverprofile` flag with the recommended two-step pattern.
- **FR-007**: The Gaze project page or tester guide MUST document the `/gaze fix` command for AI-assisted test generation.
- **FR-008**: The Gaze project page MUST document fix strategy labels (`add_tests`, `add_assertions`, `decompose`, `decompose_and_test`).
- **FR-009**: The CI integration example MUST use a realistic `--max-crapload` threshold value (not 15).
- **FR-010**: The single-package limitation MUST be scoped to `gaze analyze` and `gaze quality` only.
- **FR-011**: The side effect detection limitation MUST clearly state P2 IS detected and only P3-P4 are undetected.

**Dewey (US4-US5):**

- **FR-012**: The knowledge getting-started page MUST document automatic `.gitignore` respect for disk sources.
- **FR-013**: The Local Disk YAML example MUST include `ignore` and `recursive` fields with syntax documentation.
- **FR-014**: The claim about indexing "all Markdown files" MUST be updated to reflect `.gitignore` filtering.
- **FR-015**: The knowledge page MUST document `dewey doctor` with a description of its diagnostic sections.
- **FR-016**: The knowledge page MUST document `dewey reindex` for clean index rebuilds.
- **FR-017**: The knowledge page MUST document global CLI flags: `--verbose`, `--log-file`, `--no-embeddings`, `--vault`.
- **FR-018**: The knowledge page MUST include a troubleshooting section covering common issues (MCP timeout, Ollama not running, lock file conflicts, low embedding coverage) with resolution steps.

**Swarm Workflow (US6-US8):**

- **FR-019**: The common-workflows page MUST document `/unleash` with its 8-step pipeline, exit points, Dewey-powered clarification, parallel Swarm workers, resumability, and demo output.
- **FR-020**: The common-workflows page MUST document `/finale` with its 9-step workflow, branch safety, CI check watching, and rebase-merge strategy.
- **FR-021**: The common-workflows code review section MUST document Phase 1a (CI hard gate derived from workflow files) and Phase 1b (conditional Gaze quality analysis).
- **FR-022**: The developer guide convention packs section MUST list 7 pack files (adding `severity.md`) with the severity pack described as the shared calibration standard for CRITICAL/HIGH/MEDIUM/LOW findings.
- **FR-023**: The Divisor team page MUST document exclusive ownership boundaries, the shared severity standard, and prior learnings integration.
- **FR-024**: The `uf init` section MUST document `opencode.json` management (Dewey MCP entry, Swarm plugin entry).
- **FR-025**: The `uf setup` section MUST reflect the reduced step count after the `opencode.json` move to `uf init`.

**Cross-cutting (Quality Requirements):**

- **FR-026**: All documentation changes MUST pass `npm run build` without errors.
- **FR-027**: All documentation changes MUST maintain cross-page consistency (no contradictory claims between pages).

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: Zero factually incorrect statements remain on Gaze documentation pages -- verified by cross-referencing every claim against Gaze v1.4.7 source.
- **SC-002**: Every Gaze feature available in v1.4.7 that affects the user's CLI experience is documented on at least one website page -- zero undiscoverable features.
- **SC-003**: Every Dewey CLI command (`serve`, `index`, `reindex`, `status`, `doctor`, `init`) is documented on at least one website page.
- **SC-004**: Every Dewey source configuration field (`ignore`, `recursive`) is documented with syntax examples.
- **SC-005**: `/unleash` and `/finale` commands each appear on at least one user-facing documentation page with usage syntax and step descriptions.
- **SC-006**: Convention pack count is consistent across all pages that reference it (7 files everywhere).
- **SC-007**: `npm run build` succeeds with zero errors after all documentation changes.
- **SC-008**: Zero cross-page factual contradictions remain -- verified by searching for key claims (pack count, setup step count, accuracy numbers) across all pages.

## Assumptions

- The Gaze codebase at v1.4.7 and the gaze-website-docs-audit.md are the authoritative sources for Gaze-related claims.
- The Dewey codebase at v1.4.2 (plus main branch for Spec 006 `.gitignore` support) and dewey-gaps-2026-03-30.md are the authoritative sources for Dewey-related claims.
- The unbound-force meta repo (AGENTS.md, command files, spec files for 017/018/019) is the authoritative source for swarm workflow features.
- The `/unleash` and `/finale` documentation will be added as new sections in `common-workflows.md` rather than as separate standalone pages, to keep the workflow documentation consolidated.
- The Dewey troubleshooting section will be added to the existing `knowledge.md` page rather than creating a new page, to keep the information co-located with the setup instructions.
- A dedicated Dewey CLI reference page is deferred. The individual commands (`doctor`, `reindex`) and global flags will be documented inline where they are most useful (in the getting-started/troubleshooting context) rather than as a standalone reference page.
- The `severity.md` convention pack details will be added to the existing convention packs section in `developer.md`, not as a separate section.
