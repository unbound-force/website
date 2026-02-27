# Data Model: Blog Section and "Why Contract Coverage" Article

**Branch**: `003-blog-why-gaze` | **Date**: 2026-02-27

## Entities

### Blog Section Index (`content/blog/_index.md`)

| Field         | Type     | Required | Description                                        |
| ------------- | -------- | -------- | -------------------------------------------------- |
| `title`       | string   | Yes      | Section title displayed as page heading ("Blog")   |
| `description` | string   | Yes      | SEO meta description for the blog index page       |
| `lead`        | string   | No       | Subtitle/lead text below the section title         |
| `date`        | datetime | Yes      | Creation date (ISO 8601 format)                    |
| `draft`       | boolean  | Yes      | Must be `false` for published sections             |
| `weight`      | integer  | Yes      | Ordering weight (1 for top-level section)          |
| `toc`         | boolean  | Yes      | Table of contents toggle (`false` for index pages) |

### Blog Post (`content/blog/*.md`)

| Field          | Type     | Required | Description                                                 |
| -------------- | -------- | -------- | ----------------------------------------------------------- |
| `title`        | string   | Yes      | Post title displayed as H1                                  |
| `description`  | string   | Yes      | SEO meta description; used in blog list cards               |
| `lead`         | string   | No       | Brief summary displayed at top of post and in list excerpts |
| `date`         | datetime | Yes      | Publication date (ISO 8601); controls sort order            |
| `draft`        | boolean  | Yes      | Must be `false` for published posts                         |
| `weight`       | integer  | Yes      | Manual ordering override within the blog section            |
| `toc`          | boolean  | Yes      | Table of contents toggle (`true` for long-form articles)    |
| `categories`   | string[] | No       | Taxonomy categories (e.g., ["Engineering"])                 |
| `tags`         | string[] | No       | Taxonomy tags (e.g., ["gaze", "testing"])                   |
| `contributors` | string[] | No       | Author/contributor names for metadata display               |

### Homepage Article Card (rendered in `layouts/home.html`)

Not a stored entity — dynamically rendered from Blog Post data. Displays:

| Rendered Field | Source                                        | Description                        |
| -------------- | --------------------------------------------- | ---------------------------------- |
| Title          | Blog Post `.Title`                            | Linked heading to the full article |
| Date           | Blog Post `.Date`                             | Formatted publication date         |
| Description    | Blog Post `.Params.description` or `.Summary` | Brief excerpt                      |
| URL            | Blog Post `.Permalink`                        | Link target for the card           |

### Navigation Menu Entry (`menus.en.toml`)

| Field    | Type    | Value    | Description                                |
| -------- | ------- | -------- | ------------------------------------------ |
| `name`   | string  | "Blog"   | Display text in the navbar                 |
| `url`    | string  | "/blog/" | Target URL                                 |
| `weight` | integer | 5        | Ordering (before GitHub link at weight 10) |

## Relationships

```
Blog Section (1) ──contains──▶ Blog Post (many)
Homepage ──queries──▶ Blog Section ──selects 1 most recent──▶ Blog Post
Navigation Menu ──links to──▶ Blog Section index
Blog Post ──classified by──▶ Categories (taxonomy)
Blog Post ──tagged with──▶ Tags (taxonomy)
```

## Configuration Changes

| File            | Field          | Current Value       | New Value                   |
| --------------- | -------------- | ------------------- | --------------------------- |
| `params.toml:7` | `mainSections` | `["docs"]`          | `["docs", "blog"]`          |
| `hugo.toml:56`  | `[permalinks]` | `docs = "..."` only | Add `blog = "/blog/:slug/"` |
| `menus.en.toml` | `[[main]]`     | GitHub only         | Add Blog entry (weight 5)   |

## File Inventory

| File                                    | Operation | Purpose                         |
| --------------------------------------- | --------- | ------------------------------- |
| `content/blog/_index.md`                | Create    | Blog section index page         |
| `content/blog/why-contract-coverage.md` | Create    | "Why Contract Coverage" article |
| `config/_default/params.toml`           | Edit      | Add `"blog"` to `mainSections`  |
| `config/_default/hugo.toml`             | Edit      | Add blog permalink pattern      |
| `config/_default/menus/menus.en.toml`   | Edit      | Add Blog navbar entry           |
| `layouts/home.html`                     | Edit      | Add "Latest Articles" section   |
