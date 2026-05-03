## ADDED Requirements

### Requirement: sandbox-blog-post

The website MUST have a blog post at `content/blog/sandbox-isolation.md` explaining why containerized AI agent sessions matter and walking through the round-trip workflow.

#### Scenario: user reads sandbox blog post

- **GIVEN** a user navigates to the sandbox isolation blog post
- **WHEN** the page renders
- **THEN** the post MUST follow the BA-001 narrative arc: problem statement, approach, walkthrough with terminal output, and call to action
- **AND** the post MUST lead with benefit framing per BA-007 (why isolation matters to the reader, not what commands were added)
- **AND** the post MUST include a step-by-step walkthrough of: `uf sandbox start`, working inside the sandbox, `uf sandbox extract`, and verifying changes on the host

### Requirement: sandbox-blog-security-model

The blog post MUST include a security model section as a scannable table.

#### Scenario: user evaluates sandbox security

- **GIVEN** a user reads the security model section
- **WHEN** they review the isolation properties
- **THEN** the table MUST include: rootless Podman, read-only mounts (isolated mode), no push credentials, resource limits, SELinux, non-root user

### Requirement: sandbox-blog-limitations

The blog post MUST include a Current Limitations section per VB-004 (honesty about current state).

#### Scenario: user reads limitations

- **GIVEN** a user reads the limitations section
- **WHEN** they evaluate adoption readiness
- **THEN** the section MUST state: single container at a time, Podman required (not Docker), and any other current constraints
- **AND** the section MUST NOT use weasel language to minimize the limitations

### Requirement: sandbox-blog-frontmatter

The blog post MUST use valid Hugo/Doks blog frontmatter consistent with existing posts.

#### Scenario: blog post builds correctly

- **GIVEN** the blog post file exists with frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build MUST succeed with no errors
- **AND** the post MUST appear in the blog listing page
- **AND** the frontmatter MUST include: title, description, lead, slug, date, draft (false), weight, toc, categories, tags, contributors

### Requirement: sandbox-blog-supplementary-sections

The blog post SHOULD include brief supplementary sections for Google Cloud / Vertex AI users and future directions.

#### Scenario: user reads supplementary sections

- **GIVEN** a user reads the blog post
- **WHEN** they reach the supplementary sections
- **THEN** a Google Cloud / Vertex AI section MAY be present explaining credential handling for cloud provider users
- **AND** a "What's Next" section MAY be present, but MUST clearly frame future directions as planned work, not shipped features

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
