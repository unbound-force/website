## Why

Five content review agents (Curator, Envoy, Herald, Scribe, TechWriter) unanimously identified the absence of an architecture/design philosophy page as the single highest-impact content gap on the website. The site explains what each tool does and how to use it, but never synthesizes why the system is structured the way it is. Visitors who want to understand the overall design -- the three-tier context system, the layered governance model, the doer/judge separation, the 8-phase pipeline -- have to piece it together from scattered references across multiple pages.

GitHub Issue: #88

## What Changes

- Add a new documentation page at `content/docs/getting-started/architecture.md` presenting the unified system architecture and design philosophy
- The page synthesizes concepts already described across constitution.md, developer.md, knowledge.md, common-workflows.md, and team pages into a single cohesive architecture overview
- Content draws on the Yanli Liu "Harness Engineering" article (AI Advances, Apr 2026) as a secondary framing lens, with proper attribution

## Capabilities

### New Capabilities
- `architecture-page`: A documentation page that presents the overall system architecture including the three-tier context system, layered governance model, control matrix, doer/judge separation, plan/execute separation, and composability

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- New file: `content/docs/getting-started/architecture.md`
- Weight positioned between existing getting-started pages (suggested: weight 82, after knowledge.md at 80 and before constitution.md at 85)
- No changes to existing pages, navigation menus, or styles
- Cross-references to existing pages (constitution.md, developer.md, knowledge.md, common-workflows.md, the-divisor.md, tester.md) use relative links to pages that already exist on main

## Constitution Alignment

Assessed against the **Unbound Force Website** constitution (.specify/memory/constitution.md).

### I. Content Accuracy

**Assessment**: PASS

The architecture page synthesizes concepts from verified upstream sources: the harness engineering analysis (temp/harness-engineering-analysis.md), the actual AGENTS.md structure, convention pack files, and agent persona files. All claims about the three-tier context system, layered governance, and doer/judge separation are verifiable against the source repositories. The Liu article data points are attributed to their source.

### II. Minimal Footprint

**Assessment**: PASS

The page is pure Markdown content -- no custom HTML, CSS, JavaScript, or template overrides. It uses standard Doks frontmatter and relies entirely on built-in theme rendering. No new dependencies added.

### III. Visitor Clarity

**Assessment**: PASS

This page directly serves visitor clarity by providing the missing "why is the system structured this way" narrative. It connects existing pages into a coherent architecture story, making the overall system design discoverable from a single page rather than requiring visitors to piece it together from scattered references.
