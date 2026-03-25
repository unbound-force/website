# Research: Dewey Documentation

**Date**: 2026-03-25
**Branch**: `007-dewey-docs`

## R1: Content Source Strategy

**Decision**: Derive all Dewey content from the design
paper (`../dewey-design-paper.md`) and the Dewey repo's
README, adapted for a website audience.

**Rationale**: The design paper is comprehensive (988
lines covering motivation, architecture, embedding
model, content sources, query model, agent impact,
workflow impact, practical considerations). The Dewey
repo README covers CLI commands and setup. Both need
to be adapted: less technical depth, more "why should
I care" framing per Constitution Principle III.

**Source locations**:

- Design paper: `https://github.com/unbound-force/unbound-force/blob/main/../dewey-design-paper.md`
  (lives at the org level, not in any single repo --
  available locally at `~/Projects/github/unbound-force/dewey-design-paper.md`)
- Spec 014: `https://github.com/unbound-force/unbound-force/tree/main/specs/014-dewey-architecture/`
- Dewey repo README: `https://github.com/unbound-force/dewey/blob/main/README.md`
- Dewey MCP tool names: `dewey_search`, `dewey_semantic_search`,
  `dewey_similar`, `dewey_semantic_search_filtered`,
  `dewey_find_by_tag`, `dewey_query_properties`,
  `dewey_get_page`, `dewey_traverse`, `dewey_find_connections`

**Alternatives considered**:

- Copy the design paper as-is: Rejected because it's
  too technical for website visitors. The paper targets
  architects; the website targets developers evaluating
  the tool.
- Write from scratch: Rejected because the design paper
  already has the right content. Adapting is faster and
  ensures accuracy (Principle I).

## R2: Page Naming and URLs

**Decision**: Getting-started guide at
`content/docs/getting-started/knowledge.md` (URL:
`/docs/getting-started/knowledge/`). Tool page at
`content/docs/team/dewey.md` (URL:
`/docs/team/dewey/`).

**Rationale**: The filename `knowledge.md` is generic
enough to cover the concept of knowledge retrieval
without coupling to the tool name. If the tool were
renamed, the URL would still make sense. The tool page
uses the tool name directly since it's specifically
about Dewey.

**Alternatives considered**:

- `dewey.md` in getting-started: Rejected because the
  URL `/docs/getting-started/dewey/` is less
  discoverable than `/docs/getting-started/knowledge/`.
- `semantic-search.md`: Rejected because Dewey
  provides more than just semantic search (structured
  queries, content sources, persistence).

## R3: Frontmatter Weight Values

**Decision**: Getting-started guide gets `weight: 80`
(after common-workflows at 70, maintaining the existing
10-increment scheme). Tool page gets `weight: 70`
(after mx-f at 60, maintaining the 10-increment scheme).

**Rationale**: The existing getting-started weights use
a 10-increment pattern (quick-start=20, developer=30,
tester=40, product-owner=50, product-manager=60,
common-workflows=70). Weight 80 places the Dewey guide
after all role-specific guides. The team section uses
the same pattern (muti-mind=20 through mx-f=60). Weight
70 places Dewey after all heroes as an infrastructure
component, not a persona.

## R4: Role-Specific Dewey Additions

**Decision**: Each role guide gets a short "Knowledge
Retrieval with Dewey" subsection (3-5 paragraphs)
added within the existing page structure, not a
separate section. This keeps the pages cohesive.

**Rationale**: Adding a full new section to each role
guide would make Dewey feel more prominent than the
hero itself. A subsection within the relevant context
(e.g., under "Working with Swarm" for the developer
guide) keeps the focus on the hero's role while
naturally introducing Dewey as a context enhancement.

**Content per role** (from Spec 014 research R3):

- **Developer**: toolstack API docs, implementation
  patterns from other repos
- **Product Owner**: cross-repo backlog patterns, past
  acceptance criteria, issue discovery
- **Product Manager**: cross-repo velocity trends,
  retrospective outcomes, coaching patterns
- **Tester**: test quality patterns, CRAP score
  baselines, known failure modes
