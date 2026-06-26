## ADDED Requirements

### Requirement: review-pr-blog-post

The website MUST have a blog post at `content/blog/review-pr-causality.md` explaining the CI causality analysis pattern and how `/review-pr` separates PR-caused failures from pre-existing ones.

#### Scenario: user reads review-pr blog post

- **GIVEN** a user navigates to the review-pr causality blog post
- **WHEN** the page renders
- **THEN** the post MUST follow the BA-001 narrative arc: problem (CI failures obscure real regressions), approach (causality classification), evidence (classification table, fix branch offering), and call to action
- **AND** the post MUST lead with benefit framing per BA-007 (developer's CI triage frustration, not tool announcement)

### Requirement: review-pr-blog-causality-table

The blog post MUST include the CI causality classification table.

#### Scenario: user reads causality classification

- **GIVEN** a user reads the causality section
- **WHEN** they review the classification logic
- **THEN** the table MUST show the 2×2 matrix: base branch pass/fail × PR check pass/fail → classification (PR-caused, pre-existing, unknown)

### Requirement: review-pr-blog-fix-branch

The blog post MUST cover the fix branch offering for pre-existing failures.

#### Scenario: user reads fix branch section

- **GIVEN** a user reads the fix branch section
- **WHEN** they understand the "fix it forward" pattern
- **THEN** the section MUST explain that `/review-pr` offers to create a fix branch for pre-existing failures
- **AND** the section MUST mention the dirty-tree guard and collision check

### Requirement: review-pr-blog-lifecycle

The blog post MUST position `/review-council` and `/review-pr` as a complete review lifecycle.

#### Scenario: user understands review lifecycle

- **GIVEN** a user reads the review lifecycle section
- **WHEN** they evaluate which command to use
- **THEN** the section MUST show the timeline: develop → `/review-council` (pre-PR) → push → create PR → `/review-pr` (post-PR)

### Requirement: review-pr-blog-frontmatter

The blog post MUST use valid Hugo/Doks blog frontmatter consistent with existing posts.

#### Scenario: blog post builds correctly

- **GIVEN** the blog post file exists with frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build MUST succeed with no errors
- **AND** the frontmatter MUST include: title, description, lead, slug, date, draft (false), weight, toc, categories, tags, contributors

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
