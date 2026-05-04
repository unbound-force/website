## Context

The website has pages for individual tools (Gaze, Dewey), team personas, workflows, and guides, but lacks a page that presents the overall system architecture. Visitors must piece together the design philosophy from scattered references across multiple pages. Issue #88 was unanimously identified by all five content review agents as the highest-impact content gap.

The proposal (proposal.md) established constitution alignment: Content Accuracy (PASS -- sourced from verified upstream repos), Minimal Footprint (PASS -- pure Markdown, no custom templates), Visitor Clarity (PASS -- fills the key missing narrative).

## Goals / Non-Goals

### Goals
- Present the unified system architecture in a single page that connects existing concepts
- Cover the six key architectural concepts: three-tier context, layered governance, control matrix, doer/judge separation, plan/execute separation, composability
- Frame content for a website audience ("why should I care") rather than raw technical detail
- Use the Liu "Harness Engineering" article as a secondary lens with proper attribution, not as the primary framing
- Cross-reference existing pages (constitution, developer, knowledge, common-workflows, team pages) where those pages contain deeper detail

### Non-Goals
- Rebranding around "harness engineering" -- the superhero team identity remains primary
- Reproducing the full harness engineering analysis -- this is a website page, not an internal document
- Adding custom HTML, diagrams, or interactive elements -- pure Markdown only
- Modifying existing pages to link back to this one (can be done in a separate cross-references change)
- Adding navigation menu entries (the page is discoverable via the existing Getting Started sidebar)

## Decisions

**Page location**: `content/docs/getting-started/architecture.md` with weight 82, placing it after knowledge.md (80) and before constitution.md (85). This positions it as a synthesis page near the end of the getting-started section, after the reader has been introduced to individual components.

**Content structure**: Six sections mapping to the six architectural concepts identified in the issue. Each section explains what the concept is, how Unbound Force implements it, and why it matters. The control matrix section uses a simple Markdown table (no custom HTML).

**Attribution approach**: Liu article findings are cited as "researchers have identified" or "industry analysis shows" with a named citation. Data points from the analysis document are attributed to their sources. The page does not present Liu's framework as Unbound Force's own.

**Cross-reference strategy**: Only link to pages that exist on the current branch (main). Do not link to pages created by other unmerged PRs (e.g., reference section pages from other branches).

## Risks / Trade-offs

**Risk: Content may drift from upstream repos.** The architecture page synthesizes claims about the system's design. If the upstream implementation changes (e.g., the Divisor Council gains or loses agents), this page could become inaccurate. Mitigation: the page describes architectural principles rather than exact counts, reducing drift sensitivity.

**Risk: Overlap with existing pages.** The constitution page, developer page, and knowledge page already describe aspects of the architecture. Mitigation: the architecture page provides a synthesis narrative and links to existing pages for detail, rather than duplicating their content.

**Trade-off: Weight positioning.** Placing the page at weight 82 puts it near the end of getting-started. An argument could be made for placing it earlier (to set context before diving into specifics), but the current ordering assumes readers arrive via quick-start and want practical content first.
