# Feature Specification: Gaze Docs Remaining

**Feature Branch**: `017-gaze-docs-remaining`
**Created**: 2026-04-06
**Status**: Draft
**Input**: Address remaining 11 Gaze documentation gaps from the gaze-website-docs-audit.md: sample output version, `--include-unexported`, `--ai-mapper`, deeper reviewer-testing/testreview docs, and 7 internal feature docs.

## Requirements _(mandatory)_

### Functional Requirements

**Stale Reference (1 item):**

- **FR-001**: The sample output version reference MUST be updated or contextualized (currently says "v1.2.3").

**Missing Significant Features (3 items):**

- **FR-002**: The `--include-unexported` flag MUST be documented with its auto-detection behavior for `package main`.
- **FR-003**: The `--ai-mapper` flag MUST be documented as an opt-in AI-assisted assertion mapping pass.
- **FR-004**: The `reviewer-testing` agent and `/speckit.testreview` command MUST have brief explanatory text beyond their listing in the `gaze init` table.

**Missing Internal Features (7 items):**

- **FR-005**: The `--max-gaze-crapload` threshold flag MUST be documented.
- **FR-006**: SSA degradation diagnostics (`ssa_degraded_packages`) MUST be mentioned in the limitations or architecture section.
- **FR-007**: The AST mutation fallback behavior MUST be mentioned.
- **FR-008**: The `no_test_coverage` classification reason MUST be mentioned.
- **FR-009**: The `New` naming prefix constructor signal MUST be mentioned.
- **FR-010**: The reduced GoDoc signal behavior MUST be mentioned.
- **FR-011**: The container unwrap assertion mapping MUST be mentioned.

**Cross-cutting:**

- **FR-012**: All changes MUST pass `npm run build` without errors.

## Success Criteria _(mandatory)_

- **SC-001**: Every feature from the 11-item remaining gap list is documented on at least one page.
- **SC-002**: `npm run build` succeeds with zero errors.

## Assumptions

- All features are documented via additions to the existing `content/docs/projects/gaze.md` page. The `--include-unexported` and `--ai-mapper` flags go in the CLI Usage section. The internal features go in a new "Advanced Features" or expanded "Architecture" section. The `reviewer-testing` and `testreview` expansion goes near the existing `gaze init` table.
- The sample output version line will note that the sample was generated with an earlier version and may differ from current output, rather than regenerating the entire sample.
- Source material comes from the Gaze README, PR descriptions, and issue context (discovered via Dewey).
