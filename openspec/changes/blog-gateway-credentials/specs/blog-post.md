## ADDED Requirements

### Requirement: gateway-blog-post

The website MUST have a blog post at `content/blog/gateway-credentials.md` explaining why credential isolation matters and how `uf gateway` eliminates credential forwarding to containers.

#### Scenario: user reads gateway blog post

- **GIVEN** a user navigates to the gateway credentials blog post
- **WHEN** the page renders
- **THEN** the post MUST follow the BA-001 narrative arc: problem statement (credential forwarding risks), approach (local reverse proxy), evidence (before/after comparison), and call to action
- **AND** the post MUST lead with benefit framing per BA-007 (why credential isolation matters to the reader, not what commands were added)
- **AND** the post MUST explain how the gateway eliminates credential forwarding: container sees localhost + dummy token, gateway handles authentication upstream

### Requirement: gateway-blog-before-after

The blog post MUST include a before/after comparison showing the concrete reduction in complexity when using the gateway.

#### Scenario: user evaluates gateway value

- **GIVEN** a user reads the before/after section
- **WHEN** they compare the two approaches
- **THEN** the comparison MUST show the reduction in environment variables forwarded to the container
- **AND** the comparison MUST show the elimination of credential file mounts

### Requirement: gateway-blog-token-refresh

The blog post MUST cover token refresh as a reliability feature.

#### Scenario: user reads token refresh section

- **GIVEN** a user reads the token refresh section
- **WHEN** they review the refresh mechanism
- **THEN** the section MUST explain proactive refresh before token expiry
- **AND** the section MUST mention deduplication to prevent thundering-herd refreshes

### Requirement: gateway-blog-frontmatter

The blog post MUST use valid Hugo/Doks blog frontmatter consistent with existing posts.

#### Scenario: blog post builds correctly

- **GIVEN** the blog post file exists with frontmatter
- **WHEN** `npm run build` is executed
- **THEN** the build MUST succeed with no errors
- **AND** the post MUST appear in the blog listing page
- **AND** the frontmatter MUST include: title, description, lead, slug, date, draft (false), weight, toc, categories, tags, contributors

### Requirement: gateway-blog-limitations

The blog post SHOULD include honest caveats about what the gateway does not solve, per VB-004.

#### Scenario: user reads limitations

- **GIVEN** a user reads the post
- **WHEN** they evaluate the gateway's scope
- **THEN** the post SHOULD note that the gateway proxies LLM API requests only — it does not proxy arbitrary HTTP traffic or non-Anthropic APIs

## MODIFIED Requirements

_None._

## REMOVED Requirements

_None._
