## Context

The Unbound Force website has thorough reference documentation for sandbox, CLI, configuration, and gateway — but no task-oriented guide that walks a user through DevPod-based development environment setup end-to-end. Seven upstream issues (#83, #87, #120, #121, #199, #50, #119) have landed changes that are partially reflected in reference pages but lack cohesive narrative documentation.

The proposal (proposal.md) identified all constitution principles as N/A for this change — it is purely static Markdown content with no impact on agent artifacts, composability, output formats, or testability. This design follows that assessment.

## Goals / Non-Goals

### Goals
- Provide a task-oriented "Development Environment Setup" guide in Getting Started that takes a new user from zero to a running DevPod sandbox session
- Document multi-platform scaffold support (`uf init --platform`) for OpenCode and Cursor
- Update sandbox.md reference to reflect DevPod as primary backend (Che deprecated), IDE selection (`--ide`), and provider pre-flight simplification
- Update cli.md reference with `uf ollama-proxy` command and current sandbox flags
- Update changelog with recent release entries
- All new content sourced from upstream issues and repository artifacts — no fabricated features

### Non-Goals
- Rewriting the entire reference section or restructuring site navigation hierarchy
- Creating a dedicated DevPod project page under Projects (DevPod is a dependency, not an Unbound Force project)
- Documenting Cursor editor features beyond what `uf init --platform cursor` generates
- Covering Eclipse Che/Dev Spaces in the new guides (retained as legacy note in reference only)
- Adding custom HTML layouts or SCSS — all changes are Markdown content

## Decisions

### D1: Two new Getting Started guides instead of one combined page

**Decision**: Create separate pages for "Development Environment Setup" (DevPod/sandbox workflow) and "Multi-Platform Support" (scaffold platform selection).

**Rationale**: These serve different user intents. A user setting up DevPod for the first time needs a linear walkthrough. A user choosing between OpenCode and Cursor platforms needs a comparison and configuration guide. Combining them would create an unfocused page that serves neither audience well.

**Alternatives considered**: A single "Advanced Setup" page was considered but rejected because sandbox setup is not "advanced" — it's a core workflow that new users need.

### D2: Place new guides in Getting Started, not Reference

**Decision**: New guides go under `content/docs/getting-started/` with weights that position them after quick-start.md (weight 20) and before role-specific guides.

**Rationale**: Getting Started is for task-oriented content ("how do I..."). Reference is for command-syntax content ("what does flag X do"). The new guides are workflows, not API references.

### D3: Update existing reference pages in-place rather than creating new ones

**Decision**: Modify sandbox.md and cli.md directly to incorporate issue changes rather than creating supplementary pages.

**Rationale**: Reference pages are the canonical location for command syntax and flag documentation. Splitting this across pages creates maintenance burden and confuses users looking for the single source of truth on a command.

### D4: Retain Eclipse Che/Dev Spaces as legacy note

**Decision**: Keep Che/Dev Spaces documentation in sandbox.md with a clear "legacy/deprecated" label. Do not remove it.

**Rationale**: The `--backend che` flag still exists in the CLI (returns a migration error). Users encountering this error need documentation explaining the migration path to DevPod.

### D5: Source all content from upstream issues

**Decision**: All new documentation content is derived from the seven identified GitHub issues and existing repository documentation. No features are described that aren't implemented.

**Rationale**: Content Accuracy behavioral constraint requires project descriptions to be derived from actual repository content. This ensures the website never overstates maturity or fabricates capabilities.

## Risks / Trade-offs

### R1: Upstream changes may land between spec and implementation

**Risk**: Issues #199 (provider pre-flight) and #119 (docs overhaul sync) are in progress. Documentation may describe behavior that shifts slightly before merge.

**Mitigation**: Document based on the issue descriptions (which represent intended behavior). Flag any in-progress items with appropriate language ("when available" or version-gated notes).

### R2: Sandbox reference page is already 277 lines

**Risk**: Adding more content to sandbox.md could make it unwieldy.

**Mitigation**: The updates primarily replace outdated Che content and refine existing DevPod sections rather than adding net-new bulk. The new getting-started guide absorbs the narrative content that would otherwise bloat the reference page.

### R3: Navigation sidebar ordering requires careful weight assignment

**Risk**: Incorrect weight values could place new pages in confusing positions relative to existing content.

**Mitigation**: Follow the established pattern — Getting Started pages use weight 10-100 range. Assign weights that place new guides logically after quick-start (20) and before role-specific guides (developer=30, tester=40, etc.). Use weight 25 for devenv-setup and 27 for multi-platform.
