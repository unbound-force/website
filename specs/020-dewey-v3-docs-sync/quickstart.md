# Quickstart: Dewey v3.0.0 Documentation Sync

## Overview

Update 3 Markdown files to reflect Dewey v3.0.0 changes: tool count (40→48), source types (3→4), knowledge lifecycle (compile/lint/promote), trust tiers, updated store_learning API, and web crawler reliability note.

## Implementation Order

Work through files in dependency order — pages that other pages link to should be updated first.

### Phase 1: Project Page (P1)

The project page is the entry point for Dewey evaluation. Other pages reference it.

1. **`content/docs/projects/dewey.md`** — Update:
   - Tool count: 40 → 48 (heading, body text, intro paragraph)
   - Category count: 10 → 12
   - Source types: "Three" → "Four" (heading and bullet list — add code)
   - Architecture section: add code to pluggable sources list
   - Add "Knowledge Lifecycle" section (compile, lint, promote)

### Phase 2: Knowledge Guide (P1)

The knowledge guide is the detailed reference. Most new content goes here.

2. **`content/docs/getting-started/knowledge.md`** — Update:
   - Source types: "three" → "four" in Configure Content Sources intro
   - Add "### Code" source configuration section (after Web Crawl)
   - Add web crawler reliability note (complex sites like pkg.go.dev)
   - Add "## Knowledge Lifecycle" section (compile, lint, promote CLI commands)
   - Add "## Trust Tiers" section (authored, validated, draft)
   - Add "## Storing and Retrieving Learnings" section (store_learning API)

### Phase 3: Team Page (P1/P2)

3. **`content/docs/team/dewey.md`** — Update:
   - Tool count: 40 → 48 in Query Capabilities intro
   - Category count: 10 → 12
   - Source types: "three" → "four" in Content Sources intro
   - Add "### Code" source subsection
   - Add compile, lint, promote to MCP tools table (new Knowledge Management category)
   - Add store_learning to MCP tools table (new Learning category)

## Verification

After all changes:

```bash
npm run build          # Must succeed with zero errors
npm run dev            # Visual check: all updated pages render correctly
```

### Cross-Page Consistency Checks

Search the entire built site for these patterns and verify consistency:

| Search Pattern                  | Expected Value              | Pages to Check                                 |
| ------------------------------- | --------------------------- | ---------------------------------------------- |
| "MCP tools" + number            | 48                          | projects/dewey.md, team/dewey.md               |
| "categories" + number           | 12                          | projects/dewey.md, team/dewey.md               |
| "source types" or "source type" | four / 4                    | projects/dewey.md, team/dewey.md, knowledge.md |
| "compile"                       | present                     | projects/dewey.md, team/dewey.md, knowledge.md |
| "lint"                          | present                     | projects/dewey.md, team/dewey.md, knowledge.md |
| "promote"                       | present                     | projects/dewey.md, team/dewey.md, knowledge.md |
| "trust tier"                    | present                     | knowledge.md                                   |
| "store_learning"                | present with correct params | knowledge.md                                   |

### Visual Verification

1. Navigate to each updated page in the dev server
2. Verify content renders correctly in both light and dark mode
3. Verify table formatting (MCP tools table on team page)
4. Verify code blocks render correctly (CLI command examples)
5. Verify all internal links resolve (click each link)
6. Verify the two-click path: homepage → Dewey project page → compile/lint/promote descriptions
