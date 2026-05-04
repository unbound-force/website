## ADDED Requirements

### Requirement: Quality Gates Documentation Page

The website MUST include a documentation page at `content/docs/getting-started/quality-gates.md` presenting the complete quality assurance pipeline.

The page MUST cover all four feedback layers in order (CI, Gaze, Divisor Council, Constitution Check).

The page MUST explain the computational-first, inferential-second ordering rationale.

The page MUST cover gatekeeping value protection and the doer/judge separation pattern.

#### Scenario: Visitor reads quality gates page

- **GIVEN** a visitor navigates to the Getting Started section
- **WHEN** they click on the Quality Gates page
- **THEN** they see the complete quality pipeline from commit to merge

#### Scenario: Build succeeds

- **GIVEN** the quality gates page exists with proper frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
