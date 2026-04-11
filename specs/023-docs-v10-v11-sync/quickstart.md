# Quickstart: v0.10.0 / v0.11.0 Documentation Sync

**Branch**: `023-docs-v10-v11-sync` | **Date**: 2026-04-11

## Overview

This is a content-only documentation sync — no new pages, no layout changes, no SCSS, no JavaScript. All changes are Markdown edits to 6 existing pages in `content/`.

## Verification Steps

### 1. Build Verification

```bash
npm run build
```

Must complete with zero errors. This is the primary gate — Hugo will fail if any frontmatter is malformed or internal links are broken.

### 2. Persona Count Consistency Check

After all changes, verify zero occurrences of "eight" in the context of Divisor/personas remain:

```bash
grep -rn "eight" content/docs/team/the-divisor.md content/docs/team/_index.md content/docs/contributing/_index.md
```

Expected: Only the word "eight" should appear in the context of the **eight protected value categories** (gatekeeping section), NOT in the context of persona counts.

### 3. Visual Verification

```bash
npm run dev
```

Check each affected page in the browser at `http://localhost:1313/`:

| Page             | URL                                       | What to Check                                                                                                          |
| ---------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| The Divisor      | `/docs/team/the-divisor/`                 | Curator section present, persona count = nine, ownership table has Curator row, Guard/Adversary audit items documented |
| Team Index       | `/docs/team/`                             | Divisor summary says "nine sub-personas"                                                                               |
| Contributing     | `/docs/contributing/`                     | Divisor description says "nine sub-personas"                                                                           |
| Common Workflows | `/docs/getting-started/common-workflows/` | Code review table has 6 rows (includes Curator)                                                                        |
| Developer Guide  | `/docs/getting-started/developer/`        | Gatekeeping section present, `.gitignore` in `uf init` docs                                                            |
| Quick Start      | `/docs/getting-started/quick-start/`      | `.gitignore` mentioned in setup description                                                                            |

### 4. Dark Mode Verification

Toggle dark mode (Doks supports auto/light/dark) and verify all 6 pages render correctly. No content should be invisible or unreadable in dark mode.

### 5. Cross-Reference Consistency

Verify the blog post link-through is consistent:

1. Navigate to `/blog/the-curator-why-documentation-triage-needed-its-own-agent/`
2. Click the link to the Divisor team page
3. Confirm the team page now says "nine personas" — matching the blog post

## What This Spec Does NOT Change

- No blog posts are modified (the Curator blog post is already published and correct)
- No new pages are created
- No navigation changes
- No homepage changes
- No SCSS/CSS changes
- No Hugo template changes
- No dependency changes

## File Change Map

```
content/docs/team/the-divisor.md          # ~50 lines added/changed (Curator section, gatekeeping audit items, count updates)
content/docs/team/_index.md               # ~1 line changed (eight → nine)
content/docs/contributing/_index.md        # ~1 line changed (eight → nine)
content/docs/getting-started/common-workflows.md  # ~1 line added (Curator table row)
content/docs/getting-started/developer.md  # ~30 lines added (gatekeeping section, .gitignore docs)
content/docs/getting-started/quick-start.md # ~1 line changed (.gitignore mention)
```

Total estimated changes: ~85 lines across 6 files. All Markdown, no code.
