## ADDED Requirements

### Requirement: Convention Packs Blog Post

The website MUST include a blog post explaining convention packs as reusable, portable harness templates for AI agent coding standards.

The post MUST show real examples from actual pack files.

The post MUST cover the -custom.md extension pattern.

#### Scenario: Visitor reads convention packs post

- **GIVEN** a visitor navigates to the blog
- **WHEN** they read the convention packs post
- **THEN** they understand what convention packs are, how they are structured, and why they matter

#### Scenario: Build succeeds

- **GIVEN** the blog post exists with proper frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
