# Quickstart: Getting Started Guides

**Branch**: `005-getting-started-guides` | **Date**: 2026-03-22

## Validation Steps

### Build Validation

```bash
npm run build
```

The build MUST succeed with zero errors. Hugo will fail if
any frontmatter is malformed, any internal links are broken,
or any Markdown syntax is invalid.

### Visual Validation

```bash
npm run dev
```

1. Open `http://localhost:1313/docs/getting-started/`
2. Verify the sidebar shows all 6 pages in order:
   - What is Unbound Force?
   - Quick Start
   - Developer / Engineer
   - Tester
   - Product Owner
   - Product Manager
   - Common Workflows
3. Click each guide and verify:
   - Title renders correctly
   - Table of contents appears (toc: true)
   - All H2/H3 sections are present per FR-011 through FR-015
   - No placeholder or "Coming Soon" content
   - Code blocks render with proper formatting
4. Toggle dark mode and verify all pages render correctly

### Cross-Reference Validation

For each guide page, click every internal link and verify
it navigates to the correct target page. Links to check:

- Developer guide → Common Workflows
- Tester guide → Common Workflows, Gaze project page
- Product Owner guide → Common Workflows, team pages
- Product Manager guide → Common Workflows, team pages
- Common Workflows → Developer guide, Tester guide,
  Product Owner guide, Product Manager guide

### Content Accuracy Spot Check

For the developer guide, verify these specific claims:

- `unbound setup` is described as a single command
- The Speckit pipeline stages match the actual commands
  (specify, clarify, plan, tasks, implement)
- The Swarm session lifecycle matches swarmtools.ai docs
  (hive_ready, /swarm, hive_sync + git push)
