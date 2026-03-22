# Research: Binary Rename -- Website Updates

**Branch**: `006-binary-rename` | **Date**: 2026-03-22

## Research Summary

No NEEDS CLARIFICATION items existed in the Technical Context. This research documents the key decisions, their rationale, and alternatives considered for the content update approach.

## Decision 1: Primary CLI Name in Documentation

**Decision**: Use `uf` as the primary command name in all CLI examples and inline references.

**Rationale**: The orchestration plan (dewey-orchestration-plan.md, Phase 0.5) establishes `uf` as the short alias and `unbound-force` as the canonical full name. For documentation, brevity matters -- `uf setup` is easier to read, type, and remember than `unbound-force setup`. The first mention on each page explains the relationship between the two names.

**Alternatives considered**:

- Use `unbound-force` everywhere: Rejected because it's verbose for repeated CLI examples and code blocks. Makes command lines harder to scan.
- Use both interchangeably: Rejected because inconsistency creates confusion. A single canonical documentation form is clearer.

## Decision 2: Alias Explanation Placement

**Decision**: Include a brief inline explanation of the `uf` alias at the first CLI reference on each page (FR-003).

**Rationale**: Readers may land on any page directly (deep links, search results). Each page must be self-contained enough that a reader encountering `uf` for the first time understands it's the Unbound Force CLI. A brief parenthetical or inline note on first mention achieves this without cluttering subsequent references.

**Alternatives considered**:

- Explain only on the developer.md page: Rejected because common-workflows.md and product-manager.md can be entry points too.
- Add a dedicated "CLI naming" section: Rejected as over-engineering for a simple alias note. Violates Minimal Footprint principle.
- Add a glossary page: Rejected as unnecessary infrastructure for a single term.

## Decision 3: Homebrew Formula Name

**Decision**: Use `brew install unbound-force/tap/unbound-force` as the documented install command.

**Rationale**: The orchestration plan specifies the formula will be renamed from `unbound` to `unbound-force` in the Homebrew tap. The tap namespace remains `unbound-force/tap`. This is the full, unambiguous install command that will work after the upstream rename is complete.

**Alternatives considered**:

- `brew install unbound-force/tap/uf`: Rejected because Homebrew formulae conventionally use the full binary name, not aliases. The `uf` symlink is created by the formula's post-install hook.

## Decision 4: Scope Boundary -- Organization Name References

**Decision**: Only update CLI binary/command references. Organization name references ("Unbound Force" in prose, GitHub URLs like `github.com/unbound-force/`) remain unchanged.

**Rationale**: The organization name has not changed. The rename only affects the CLI binary. Changing prose references would be incorrect and would violate Content Accuracy. The formatting convention (backticks for CLI, plain text for org name) provides sufficient visual distinction.

**Alternatives considered**:

- None -- this boundary is unambiguous.

## Decision 5: No New Files or Structural Changes

**Decision**: This feature requires only content edits to 3 existing Markdown files. No data-model.md, contracts/, or quickstart.md artifacts are needed.

**Rationale**: There are no data entities (static site, content-only change), no external interfaces (no APIs, CLIs, or protocols exposed by this change), and no new setup procedures. Creating empty placeholder artifacts would violate the Zero-Waste Mandate from AGENTS.md.

**Alternatives considered**:

- None -- the scope is strictly content updates.

## Dependency Analysis

| Dependency                            | Status                        | Impact                                                                             |
| ------------------------------------- | ----------------------------- | ---------------------------------------------------------------------------------- |
| Spec 013 (binary rename in meta repo) | Must be merged upstream first | CLI commands documented on the site must actually work when copy-pasted            |
| Homebrew tap formula update           | Must be published first       | `brew install unbound-force/tap/unbound-force` must resolve to the correct formula |
| Dewey orchestration Phase 0.5.4       | This spec IS Phase 0.5.4      | This work item corresponds directly to the website portion of the binary rename    |

## Risk Assessment

| Risk                                                | Likelihood                               | Impact                                | Mitigation                                                                                     |
| --------------------------------------------------- | ---------------------------------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Upstream rename not yet merged when website deploys | Low (deployment is gated on main branch) | High (documented commands would fail) | Website PR should not merge until Spec 013 is confirmed complete                               |
| Missed CLI reference in content                     | Low (comprehensive grep performed)       | Medium (inconsistent user experience) | Post-implementation grep of `public/` build output for residual `unbound` CLI patterns         |
| Organization name accidentally changed              | Low (clear formatting convention)        | Medium (incorrect content)            | FR-005 explicitly protects org name references; review focuses on backtick-formatted text only |
