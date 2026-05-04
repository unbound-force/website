## ADDED Requirements

### Requirement: Doer/Judge Blog Post

The website MUST include a blog post explaining the structural doer/judge separation in the Divisor Council.

The post MUST cover structural vs behavioral enforcement of review boundaries.

The post MUST attribute Anthropic findings to their source (Liu article).

The post MUST describe the layered feedback stack and the 3-iteration review cap.

#### Scenario: Visitor reads doer/judge post

- **GIVEN** a visitor navigates to the blog
- **WHEN** they read the doer/judge separation post
- **THEN** they understand why review agents are structurally prevented from modifying files

#### Scenario: Build succeeds

- **GIVEN** the blog post exists with proper frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
