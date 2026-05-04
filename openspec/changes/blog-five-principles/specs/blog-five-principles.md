## ADDED Requirements

### Requirement: Five Principles Blog Post

The website MUST include a blog post at `content/blog/five-principles-every-ai-agent-harness-discovers.md` that presents Unbound Force's system architecture through the lens of five convergence principles.

The post MUST cover all five principles:
1. Context beats instructions (three-tier context system)
2. Planning and execution must be separated (8-phase pipeline)
3. Feedback loops are non-negotiable (layered CI → Gaze → Divisor)
4. One thing at a time (forced incrementalism, phase boundaries)
5. The codebase is the documentation (embed.FS scaffold, AGENTS.md)

The post MUST attribute all claims from the Liu article to their source.

The post MUST NOT present "harness engineering" as Unbound Force's primary identity or branding.

The post SHOULD include a section on how the principles reinforce each other.

The post SHOULD be honest about harness weight and the "build to delete" concern.

#### Scenario: Visitor reads the blog post

- **GIVEN** a visitor navigates to the blog section
- **WHEN** they click on the five principles blog post
- **THEN** they see a narrative covering all five principles with concrete evidence from the Unbound Force codebase

#### Scenario: Blog post builds without errors

- **GIVEN** the blog post exists with proper frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build completes successfully with no errors

### Requirement: Blog Post Frontmatter

The post MUST include frontmatter with: title, description, lead, slug, date, draft (false), weight, toc (true), categories, tags, and contributors.

#### Scenario: Post appears in blog listing

- **GIVEN** the post has draft: false
- **WHEN** the blog index page renders
- **THEN** the post appears in the listing with its title and description

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
