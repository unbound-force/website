## 1. Create Architecture Page

- [x] 1.1 Create `content/docs/getting-started/architecture.md` with YAML frontmatter (title, description, lead, date, draft: false, weight: 82, toc: true)
- [x] 1.2 Write the three-tier context system section (static docs + versioned rules + dynamic semantic memory)
- [x] 1.3 Write the layered governance model section (constitution > convention packs > agent personas > commands > CI)
- [x] 1.4 Write the control matrix section with Markdown table (feedforward/feedback x computational/inferential, all four quadrants populated)
- [x] 1.5 Write the doer/judge separation section (tool access restrictions, read-only review agents)
- [x] 1.6 Write the plan/execute separation section (8-phase pipeline, hard gates between phases)
- [x] 1.7 Write the composability section (independently useful components, graceful degradation)
- [x] 1.8 Add cross-references to existing pages (constitution.md, developer.md, knowledge.md, common-workflows.md, the-divisor.md, tester.md) — only link to pages that exist on the current branch

## 2. Verification

- [x] 2.1 Run `npm run build` and confirm no errors
- [x] 2.2 Verify page appears in correct sidebar position (after knowledge.md, before constitution.md)
- [x] 2.3 Verify all cross-reference links resolve to existing pages
- [x] 2.4 Verify constitution alignment: Content Accuracy (claims sourced from upstream), Minimal Footprint (pure Markdown, no custom elements), Visitor Clarity (clear architecture narrative for website visitors)
