## Context

The Dewey documentation on the Unbound Force website spans three pages (`projects/dewey.md`, `getting-started/knowledge.md`, `team/dewey.md`) and is stale relative to the current Dewey v3.2.0 release. Twelve upstream changes need to be reflected. The proposal (proposal.md) established that all changes are additive updates to existing pages with no structural, navigation, or layout changes required. Constitution alignment is N/A for Autonomous Collaboration, Composability First, and Security by Default; PASS for Observable Quality and Testability.

## Goals / Non-Goals

### Goals
- Update all three Dewey documentation pages to accurately reflect Dewey v3.2.0
- Maintain cross-page consistency for shared facts (tool count, installation methods, provider list)
- Preserve existing page structure and section ordering on each page
- Source all content from upstream GitHub issues with traceable provenance
- Keep documentation website-audience appropriate (not raw README content)

### Non-Goals
- Creating new documentation pages (all content fits existing pages)
- Documenting Dewey internals or implementation details (website is user-facing)
- Covering unreleased or in-progress Dewey features
- Modifying navigation, layouts, templates, or SCSS
- Updating non-Dewey documentation pages

## Decisions

### D1: Update in-place rather than restructure

**Decision**: Modify existing sections within each page rather than reorganizing the page structure.

**Rationale**: The current page structure is well-established and linked from multiple locations. Restructuring would risk breaking internal cross-references and user bookmarks. The twelve changes all fit naturally into existing sections (installation, configuration, environment variables, diagnostics, etc.).

### D2: Group related changes by target section

**Decision**: Apply changes to each page in section order (top to bottom) rather than by issue number.

**Rationale**: This minimizes edit conflicts and ensures each section is touched once with all relevant updates applied together. For example, the installation section on `knowledge.md` receives both the RPM addition (#186) and the Homebrew fix note (#213) in a single editing pass.

### D3: Tool count follows cumulative progression

**Decision**: The final tool count is 50 across all pages. The progression is: 48 (baseline) → 49 (`curate` from #41) → 50 (`store_compiled` from #113).

**Rationale**: Both tools are documented in their respective issues. The count must be consistent across `projects/dewey.md` (feature list), `getting-started/knowledge.md` (if mentioned), and `team/dewey.md` (tool catalog header).

### D4: Environment variable documentation pattern

**Decision**: New env vars (`DEWEY_CHUNK_MAX_CHARS`, `DEWEY_SYNTHESIS_ENDPOINT`, `DEWEY_AUTHOR`) follow the existing table format established in `knowledge.md` for `DEWEY_EMBEDDING_MODEL` and related variables.

**Rationale**: Consistency with the existing documentation pattern. The env var reference table in `knowledge.md` already uses a consistent format with variable name, description, and default value columns.

### D5: Provider configuration uses side-by-side examples

**Decision**: Document Ollama and Vertex AI provider configuration with separate `config.yaml` code blocks showing each provider's settings, rather than a single combined example.

**Rationale**: Users typically configure one provider, not both. Separate examples are easier to copy-paste and reduce confusion about which fields apply to which provider. The `region: global` behavior for Vertex AI (#240) is documented inline within the Vertex AI example.

### D6: Synthesis endpoint precedence documented as a callout

**Decision**: The inverted precedence chain for `DEWEY_SYNTHESIS_ENDPOINT` (config.yaml > env var, opposite of embedding) is documented with an explicit callout/note rather than buried in a table.

**Rationale**: This is a surprising behavior that could cause user confusion. Making it visually prominent prevents misconfiguration. References issue #243.

### D7: Content sanitization as a new subsection within existing pages

**Decision**: Add a "Content Sanitization" subsection to `knowledge.md` under the existing content sources or configuration section, rather than creating a standalone page.

**Rationale**: Sanitization is configured per-source in `sources.yaml` and is part of the content pipeline. It fits naturally alongside the existing content source documentation. The pattern catalog (10 regex patterns across 3 severity levels) is concise enough for a subsection.

## Risks / Trade-offs

### R1: Page length growth

**Risk**: `knowledge.md` is already 555 lines. Adding provider configuration, sanitization, curated stores, and new env vars could push it past 700 lines.

**Mitigation**: Use collapsible sections or concise formatting where appropriate. The page already has a table of contents (`toc: true`) which helps navigation. If the page becomes unwieldy during implementation, individual sections can be extracted to dedicated pages in a follow-up change.

### R2: Upstream drift during implementation

**Risk**: New Dewey changes could land while this documentation sync is in progress.

**Mitigation**: This change covers a specific set of 12 issues. Any new upstream changes will be tracked by new GitHub issues and addressed in a subsequent sync. The branch-based workflow isolates this work.

### R3: Accuracy of feature descriptions

**Risk**: Documentation is derived from GitHub issue descriptions, not from direct testing of Dewey features.

**Mitigation**: All content is traceable to specific issue numbers. The issues contain detailed technical descriptions authored by the Dewey maintainers. Any uncertainty should be resolved by checking the linked PRs in the upstream repository. The zero-waste mandate prevents fabricating features.
