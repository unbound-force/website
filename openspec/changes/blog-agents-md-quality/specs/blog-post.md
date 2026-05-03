## ADDED Requirements

### Requirement: agents-md-blog-post

The website MUST have a blog post at `content/blog/agents-md-quality.md` explaining why AGENTS.md quality determines AI-generated code quality and introducing `/agent-brief` as a quality assessment tool.

#### Scenario: user reads agents-md blog post

- **GIVEN** a user navigates to the AGENTS.md quality blog post
- **WHEN** the page renders
- **THEN** the post MUST follow the BA-001 narrative arc: problem (weak context → weak code), approach (structured AGENTS.md with section taxonomy), evidence (5-tier rubric, structural checks), and call to action
- **AND** the post MUST lead with benefit framing per BA-007 (universal problem applicable to any AI coding agent)
- **AND** the post MUST mention at least two AI coding tools beyond OpenCode (e.g., Claude Code, Cursor, Copilot) to signal broad relevance

### Requirement: agents-md-blog-rubric

The blog post MUST include the 5-tier scoring rubric.

#### Scenario: user self-assesses their AGENTS.md

- **GIVEN** a user reads the scoring rubric section
- **WHEN** they compare their AGENTS.md against the criteria
- **THEN** all 5 tiers MUST be listed: Excellent, Strong, Adequate, Weak, Missing
- **AND** each tier MUST include its criteria summary

### Requirement: agents-md-blog-taxonomy

The blog post MUST explain the section taxonomy (Tier 1 / Tier 1C / Tier 2).

#### Scenario: user understands what to include

- **GIVEN** a user reads the section taxonomy
- **WHEN** they plan their AGENTS.md content
- **THEN** Tier 1 sections (essential) MUST be listed with examples
- **AND** Tier 1C sections (context-sensitive) MUST be explained as conditionally triggered
- **AND** Tier 2 sections (advanced) MUST be listed

### Requirement: agents-md-blog-frontmatter

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
