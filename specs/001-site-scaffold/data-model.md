# Data Model: Site Scaffold and Deployment

**Feature**: 001-site-scaffold | **Date**: 2026-02-23

## Overview

This is a static Hugo website with no database, API, or dynamic data layer. The "data model" consists of Hugo configuration entities (TOML files), content frontmatter schemas, and template data structures consumed by the homepage layout.

## Entities

### 1. Hugo Site Configuration (`hugo.toml`)

| Field | Type | Required | Value |
|-------|------|----------|-------|
| `title` | string | yes | `"Unbound Force"` |
| `baseurl` | string | yes | `"https://unboundforce.dev/"` |
| `disableAliases` | bool | yes | `true` |
| `disableHugoGeneratorInject` | bool | yes | `true` |
| `enableEmoji` | bool | yes | `true` |
| `enableGitInfo` | bool | yes | `false` |
| `enableRobotsTXT` | bool | yes | `true` |
| `languageCode` | string | yes | `"en-US"` |
| `copyRight` | string | yes | `"Copyright (c) 2025-2026 Unbound Force"` |

**Outputs**: `home = ["HTML", "RSS", "searchIndex"]`, `section = ["HTML", "RSS", "SITEMAP"]`

**Relationships**: Referenced by Hugo build process. Values consumed by templates and SEO partials.

### 2. Doks Theme Parameters (`params.toml`)

| Field | Type | Required | Value |
|-------|------|----------|-------|
| `title` | string | yes | `"Unbound Force"` |
| `description` | string | yes | Site-wide meta description |
| `[doks].colorMode` | string | yes | `"auto"` |
| `[doks].navbarSticky` | bool | yes | `true` |
| `[doks].flexSearch` | bool | yes | `true` |
| `[doks].docsRepo` | string | yes | `"https://github.com/unbound-force/website"` |
| `[doks].textDark` | string | yes | `"#e2e8f0"` |
| `[doks].accentDark` | string | yes | `"#818cf8"` |
| `[doks].textLight` | string | yes | `"#0f172a"` |
| `[doks].accentLight` | string | yes | `"#3b82f6"` |

**Relationships**: Consumed by Doks theme partials. Colors flow through to Bootstrap CSS variables.

### 3. Navigation Menus (`menus.en.toml`)

**Menu Types**:

| Menu | Purpose | Entries |
|------|---------|---------|
| `[[main]]` | Top navbar | Docs link |
| `[[social]]` | Social icons | GitHub org link |
| `[[docs]]` | Sidebar navigation | Getting Started, Projects, Team, Contributing |
| `[[footer]]` | Footer links | As needed |

**Entry Schema**:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | yes | Display label |
| `url` | string | yes | Target path or URL |
| `weight` | int | yes | Sort order (lower = higher position) |
| `identifier` | string | for `[[docs]]` | Unique ID for sidebar section |

### 4. Homepage Frontmatter (`content/_index.md`)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | yes | Page title (used in `<title>` and hero) |
| `description` | string | yes | Meta description for SEO |
| `lead` | string | yes | Subtitle text displayed in hero section |
| `date` | datetime | yes | Publication date |
| `draft` | bool | yes | Must be `false` for production |

**Relationships**: Consumed by `layouts/home.html`. The `title` and `lead` fields are rendered in the hero section via `{{ .Title }}` and `{{ .Params.lead }}`.

### 5. Homepage Template Data Structures (`layouts/home.html`)

The template does not consume external data files. All content is hardcoded in the template (feature cards, project cards, CTA text). This is intentional -- the homepage changes infrequently and hardcoding avoids unnecessary abstraction.

**Feature Card** (repeated 4x):

| Field | Type | Description |
|-------|------|-------------|
| Icon | SVG reference | Tabler icon name |
| Title | string | Feature name |
| Description | string | 1-2 sentence explanation |

**Project Card** (repeated 1x for Gaze):

| Field | Type | Description |
|-------|------|-------------|
| Icon | SVG reference | Project icon |
| Title | string | Project name ("Gaze") |
| Description | string | Accurate summary from repository |
| Link | string | URL to project page or repository |

### 6. Brand Colors (SCSS Variables)

| Variable | Light Mode | Dark Mode |
|----------|-----------|-----------|
| Primary | `#3b82f6` (electric blue) | -- |
| Accent | `#8b5cf6` (violet) | `#818cf8` (lighter violet) |
| Text | `#0f172a` (slate-900) | `#e2e8f0` (slate-200) |

**Relationships**: Defined in `_variables-custom.scss` as Bootstrap variable overrides (`$primary`). Also set in `params.toml` for Doks theme integration.

## State Transitions

N/A. This is a static site with no stateful entities.

## Validation Rules

- `baseurl` MUST end with a trailing slash
- All color values MUST be valid hex codes
- Menu `weight` values MUST be unique within each menu type
- Homepage frontmatter `draft` MUST be `false`
- CNAME file MUST contain exactly one line: `unboundforce.dev`
