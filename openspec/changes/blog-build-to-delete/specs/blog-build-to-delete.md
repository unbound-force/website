## ADDED Requirements

### Requirement: Build to Delete Blog Post

The website MUST include a blog post covering harness decay and the discipline of pruning AI agent system components.

The post MUST distinguish between decay-resistant layers (organizational intent) and decay-prone layers (model capability compensators).

The post MUST provide a practical pruning methodology.

The post MUST attribute Anthropic model progression data and Liu article findings to their sources.

#### Scenario: Visitor reads build-to-delete post

- **GIVEN** a visitor navigates to the blog
- **WHEN** they read the build-to-delete post
- **THEN** they understand which harness components persist across model upgrades and how to test for decay

#### Scenario: Build succeeds

- **GIVEN** the blog post exists with proper frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
