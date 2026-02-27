# Quick Start: Blog Section and "Why Contract Coverage" Article

**Branch**: `003-blog-why-gaze` | **Date**: 2026-02-27

## Prerequisites

- Node.js >= 20.11.0
- Go >= 1.23 (for Hugo module resolution)
- Dependencies installed: `npm install`

## Implementation Order

### 1. Configuration Changes

Edit three config files to register the blog section:

**`config/_default/params.toml`** — Add `"blog"` to `mainSections`:

```toml
mainSections = ["docs", "blog"]
```

**`config/_default/hugo.toml`** — Add blog permalink under `[permalinks]`:

```toml
[permalinks]
  docs = "/docs/:sections[1:]/:slug/"
  blog = "/blog/:slug/"
```

**`config/_default/menus/menus.en.toml`** — Add Blog to navbar:

```toml
[[main]]
  name = "Blog"
  url = "/blog/"
  weight = 5
```

### 2. Blog Section Index

Create `content/blog/_index.md`:

```yaml
---
title: "Blog"
description: "Technical articles and insights from the Unbound Force project."
lead: ""
date: 2026-02-27T00:00:00+00:00
draft: false
weight: 1
toc: false
---
```

### 3. Article Content

Create `content/blog/why-contract-coverage.md` adapted from `temp/why-gaze.md` with:

- Proper YAML frontmatter (title, description, lead, date, categories, tags, contributors)
- Body starting with `##` (H2)
- All 4 accuracy corrections applied (FR-009 through FR-012)
- Fenced code blocks with language identifiers

### 4. Homepage Integration

Edit `layouts/home.html` — add a "Latest Articles" section between the Projects section and the CTA section. Use Hugo's `site.GetPage "/blog"` to dynamically query the most recent blog post.

### 5. Verification

```bash
npm run build              # Must succeed with zero errors
npm run dev                # Visually verify:
                           #   - Blog link in navbar
                           #   - Blog index page lists the article
                           #   - Article renders with code blocks, tables
                           #   - Homepage shows "Latest Articles" section
                           #   - Dark mode renders correctly on all pages
                           #   - Existing pages unchanged
```
