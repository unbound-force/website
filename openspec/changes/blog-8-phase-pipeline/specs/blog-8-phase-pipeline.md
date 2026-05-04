## ADDED Requirements

### Requirement: 8-Phase Pipeline Blog Post

The website MUST include a blog post explaining the architectural rationale for the 8-phase pipeline.

The post MUST distinguish the "why" (rationale) from the "how" (mechanics) already covered in unleash-in-practice.

The post MUST cover the enforcement mechanisms that make phase boundaries real.

#### Scenario: Visitor reads the pipeline post

- **GIVEN** a visitor navigates to the blog
- **WHEN** they read the 8-phase pipeline post
- **THEN** they understand why 8 phases exist instead of 2 and how the gates are enforced

#### Scenario: Build succeeds

- **GIVEN** the blog post exists with proper frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
