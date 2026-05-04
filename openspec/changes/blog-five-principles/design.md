## Context

The website has eight blog posts covering individual tools and features. No post presents the system-level architecture — how all the pieces compose into a coherent harness engineering system. Issue #89 calls for a flagship post using Liu's five convergence principles as the organizing framework.

The proposal (proposal.md) established constitution alignment: Content Accuracy (PASS), Minimal Footprint (PASS), Visitor Clarity (PASS).

## Goals / Non-Goals

### Goals
- Present each of Liu's five convergence principles with concrete Unbound Force evidence
- Show how the principles reinforce each other (not independent)
- Be honest about harness weight and the "build to delete" concern
- Attribute all Liu article data points to their source
- Use the superhero team identity as primary framing, harness engineering as secondary lens
- Link to getting-started guide as CTA

### Non-Goals
- Reproducing the full harness engineering analysis document
- Adding diagrams or custom HTML elements
- Modifying existing blog posts or pages
- Rebranding around "harness engineering"

## Decisions

**File location**: `content/blog/five-principles-every-ai-agent-harness-discovers.md` with slug `five-principles-ai-agent-harness`.

**Frontmatter pattern**: Follows the pattern from `unleash-in-practice.md`: title, description, lead, slug, date, draft: false, weight, toc, categories, tags, contributors.

**Content structure**: Introduction (the problem) → five principle sections → principles reinforce each other → honest about harness weight → CTA. Each principle section follows: what the principle is (citing Liu) → how Unbound Force implements it → why it matters.

**Attribution**: Cite as "Yanli Liu, 'Harness Engineering,' AI Advances, Apr 2026" with specific data points attributed inline.

**Cross-references**: Only link to pages that exist on main. Do not assume the architecture page from #88 exists on this branch.

## Risks / Trade-offs

**Risk: Overlap with architecture page (#88).** Both cover similar architectural concepts. Mitigation: the blog post is narrative-driven (external framework, industry context, "why you'll discover these") while the docs page is reference-oriented (what the system looks like). Different audiences, different purposes.

**Risk: Liu article framing may feel derivative.** Mitigation: the post leads with the problem (teams rediscovering the same principles) and uses Liu as evidence, not as the framing itself.
