## ADDED Requirements

### Requirement: Blog Post Frontmatter

Each blog post MUST include YAML frontmatter with the following fields: `title`, `description`, `lead`, `slug`, `date`, `draft` (set to `false`), `weight`, `toc` (set to `true`), `categories`, `tags`, and `contributors`.

#### Scenario: Valid blog post frontmatter
- **GIVEN** a new blog post Markdown file in `content/blog/`
- **WHEN** Hugo processes the file during build
- **THEN** the file MUST have all required frontmatter fields and `npm run build` MUST succeed without errors

### Requirement: Blog Post Body Structure

Each blog post MUST start body content with `##` (H2) headings. The `title` frontmatter generates H1. Posts MUST use ATX-style headings and fenced code blocks with language identifiers where applicable.

#### Scenario: Correct heading hierarchy
- **GIVEN** a blog post with `title: "Example Title"` in frontmatter
- **WHEN** the post body begins
- **THEN** the first heading MUST be `##` (H2), not `#` (H1)

### Requirement: Blog Post Content Quality

Each blog post MUST follow the narrative arc defined in content pack BA-001: a problem statement, approach or evidence section, and a conclusion with a call to action. Posts MUST cover the key sections outlined in the corresponding GitHub issue. Key factual claims (version numbers, command names, configuration keys) SHOULD be verified against upstream repositories during implementation.

#### Scenario: Blog post narrative structure
- **GIVEN** a completed blog post
- **WHEN** reviewed against the content pack BA-001 narrative arc requirement
- **THEN** the post MUST contain a problem statement or motivation section, an approach/evidence/walkthrough section, and a conclusion with a call to action or next steps

### Requirement: CI Failure Classification Blog Post (#64)

The site MUST include a blog post at `content/blog/ci-failure-classification.md` covering how `/review-pr` separates pre-existing CI failures from PR-caused regressions.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "ci-failure-classification"`
- **WHEN** a user navigates to `/blog/ci-failure-classification/`
- **THEN** the page MUST render with the full article content

### Requirement: AGENTS.md Quality Blog Post (#65)

The site MUST include a blog post at `content/blog/agents-md-quality.md` covering why AGENTS.md quality determines AI-generated code quality, including the `/agent-brief` command and 5-tier scoring rubric.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "agents-md-quality"`
- **WHEN** a user navigates to `/blog/agents-md-quality/`
- **THEN** the page MUST render with the full article content

### Requirement: Dewey Knowledge Stores Blog Post (#69)

The site MUST include a blog post at `content/blog/dewey-knowledge-stores.md` covering file-backed learnings and curated knowledge stores in Dewey v3.1.0.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "dewey-knowledge-stores"`
- **WHEN** a user navigates to `/blog/dewey-knowledge-stores/`
- **THEN** the page MUST render with the full article content

### Requirement: Dewey Slash Command UX Blog Post (#72)

The site MUST include a blog post at `content/blog/dewey-slash-command-ux.md` covering the `/dewey-store` slash command and its three interaction modes.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "dewey-slash-command-ux"`
- **WHEN** a user navigates to `/blog/dewey-slash-command-ux/`
- **THEN** the page MUST render with the full article content

### Requirement: Pluggable LLM Providers Blog Post (#116)

The site MUST include a blog post at `content/blog/pluggable-llm-providers.md` covering Dewey's pluggable Embedder/Synthesizer architecture with Ollama and Vertex AI support.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "pluggable-llm-providers"`
- **WHEN** a user navigates to `/blog/pluggable-llm-providers/`
- **THEN** the page MUST render with the full article content

### Requirement: Gaze Baseline Comparison Blog Post (#150)

The site MUST include a blog post at `content/blog/gaze-baseline-comparison.md` covering per-function CRAP regression detection and CI gate integration.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "gaze-baseline-comparison"`
- **WHEN** a user navigates to `/blog/gaze-baseline-comparison/`
- **THEN** the page MUST render with the full article content

### Requirement: Council Review Action Blog Post (#191)

The site MUST include a blog post at `content/blog/council-review-action.md` covering how the council-review-action composite GitHub Action brings the Divisor Council to CI.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "council-review-action"`
- **WHEN** a user navigates to `/blog/council-review-action/`
- **THEN** the page MUST render with the full article content

### Requirement: Prompt Hardening Blog Post (#198)

The site MUST include a blog post at `content/blog/prompt-hardening.md` covering prompt engineering techniques for compression resilience, including the vulnerability taxonomy and hardening techniques.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "prompt-hardening"`
- **WHEN** a user navigates to `/blog/prompt-hardening/`
- **THEN** the page MUST render with the full article content

### Requirement: Multi-Platform Support Blog Post (#51)

The site MUST include a blog post at `content/blog/multi-platform-support.md` covering how `uf init` scaffolds for both OpenCode and Cursor using a strategy pattern.

#### Scenario: Post renders at correct URL
- **GIVEN** the blog post file exists with `slug: "multi-platform-support"`
- **WHEN** a user navigates to `/blog/multi-platform-support/`
- **THEN** the page MUST render with the full article content

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
