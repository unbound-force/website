# Research: Blog Section and "Why Contract Coverage" Article

**Branch**: `003-blog-why-gaze` | **Date**: 2026-02-27

## R1: Doks Built-in Blog Support

**Decision**: Use the Doks theme's built-in blog templates (`blog/list.html`, `blog/single.html`) without custom overrides.

**Rationale**: Doks ships with first-class blog section support:

- `node_modules/@thulite/doks-core/layouts/blog/list.html` — paginated card-based post listing with metadata (date, categories, reading time)
- `node_modules/@thulite/doks-core/layouts/blog/single.html` — individual post rendering with title, metadata, summary, tags, and optional related posts
- `node_modules/@thulite/doks-core/layouts/_partials/main/blog-meta.html` — shared partial for date, categories, contributors, reading time
- These templates are already mounted via `config/_default/module.toml` (lines 29-33)

No custom layouts are needed for the blog section itself. This satisfies Constitution Principle II (Minimal Footprint) by using built-in features.

**Alternatives considered**:

- Custom blog list/single templates: Rejected — unnecessary when Doks provides them natively
- Using the default `list.html`/`single.html` (docs-oriented): Rejected — the blog-specific templates are better suited and avoid sidebar navigation leaking into blog pages

## R2: Hugo Configuration for Blog Section

**Decision**: Three configuration changes are required:

1. **`params.toml`**: Add `"blog"` to `mainSections` (line 7) — changes `["docs"]` to `["docs", "blog"]`. This ensures blog posts appear in RSS feeds and the search index.

2. **`hugo.toml`**: Add a permalink pattern for blog — `blog = "/blog/:slug/"` under `[permalinks]` (line 56). This produces clean URLs like `/blog/why-contract-coverage/`.

3. **`menus.en.toml`**: Add a `[[main]]` entry for Blog with `url = "/blog/"` and a weight that positions it before the GitHub external link.

**Rationale**: These are the minimum config changes to integrate a new top-level content section. The blog section does NOT need to be added to `sectionNav` (FR-003 prohibits sidebar nav for blog).

**Alternatives considered**:

- Adding blog to `sectionNav`: Rejected — spec explicitly prohibits sidebar navigation for the blog section
- Custom permalink pattern with date (e.g., `/blog/:year/:month/:slug/`): Rejected — unnecessary complexity for a project blog with low publishing frequency

## R3: Homepage "Latest Articles" Integration

**Decision**: Add a new section to `layouts/home.html` between the Projects section and the CTA section. Use Hugo's `site.GetPage "/blog"` to access the blog section, then query its pages sorted by date (descending), limited to 1 post.

**Rationale**: Hugo's `site.GetPage` + `.Pages.ByDate.Reverse` is the idiomatic way to dynamically pull content from another section. Limiting to 1 post matches the clarification decision. The section should be visually consistent with the existing Projects section (same spacing, card style, responsive grid).

**Alternatives considered**:

- Hugo's `where` function on `site.RegularPages`: Works but `site.GetPage` is more explicit about sourcing from the blog section specifically
- Hardcoding article references: Rejected — FR-016 requires dynamic sourcing

## R4: Article Content Adaptation

**Decision**: Adapt `temp/why-gaze.md` into `content/blog/why-contract-coverage.md` with 4 accuracy corrections applied:

1. **AI agent claim (FR-009)**: Reword "Optionally, project documentation... can be fed through an AI agent to refine confidence further" to clarify this is an external OpenCode agent workflow, not built into the Gaze binary.

2. **GazeCRAP formula (FR-010)**: Add `/100` divisor to make notation consistent with the CRAP formula. Both should read `(1 - coverage/100)^3`.

3. **CLI commands (FR-011)**: Change `gaze quality ./...` to `gaze quality ./internal/analysis` (single package). Remove the nonexistent `gaze crap --gaze ./...` example or replace with `--max-gaze-crapload`.

4. **Static analysis caveat (FR-012)**: Add a note that the `crap` subcommand can optionally run `go test` for coverage data.

**Rationale**: These corrections are based on verification of 15 claims against the Gaze repository source code. 11 claims were fully accurate; 4 were partially true and require specific corrections.

**Alternatives considered**:

- Publishing the article as-is: Rejected — Constitution Principle I (Content Accuracy) requires all claims to be verifiable

## R5: Blog Post Frontmatter Schema

**Decision**: Use the Doks blog frontmatter convention:

```yaml
---
title: "Why Contract Coverage: How Gaze Redefines What It Means to Have Good Tests"
description: "Line coverage tells you what code ran, not what was verified. Contract Coverage measures whether tests check what functions are actually responsible for."
lead: "Line coverage tells you what code ran. Contract Coverage tells you what was verified."
date: 2026-02-27T00:00:00+00:00
draft: false
weight: 100
toc: true
categories: ["Engineering"]
tags: ["gaze", "testing", "go", "static-analysis"]
contributors: ["Unbound Force"]
---
```

**Rationale**: The Doks blog templates expect `title`, `date`, and `summary` or `description`. Adding `categories`, `tags`, and `contributors` enables the Doks blog metadata partial to render rich metadata (reading time, category links, contributor display). The `lead` field is used by the Doks template for a summary/excerpt.

**Alternatives considered**:

- Minimal frontmatter (title, date, draft only): Rejected — would result in sparse metadata rendering in the blog list and single templates
- Adding `authors` instead of `contributors`: Rejected — Doks uses `contributors` taxonomy, not `authors`

## R6: Blog Section Index Page

**Decision**: Create `content/blog/_index.md` with minimal frontmatter and no body content. The Doks `blog/list.html` template renders the list of posts automatically.

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

**Rationale**: The blog index is a listing page. Body content is unnecessary — the Doks template handles rendering the post list. This follows the pattern established by the existing `content/_index.md` (homepage), which also has frontmatter-only content.

**Alternatives considered**:

- Adding body content to the index: Rejected — the Doks blog list template already provides the section header from frontmatter fields
