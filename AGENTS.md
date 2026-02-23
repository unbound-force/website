# AGENTS.md

## Project Overview

The Unbound Force website is the public-facing site for the [Unbound Force](https://github.com/unbound-force) organization -- a superhero-themed AI agent swarm for software engineering. Built with Hugo + Doks/Thulite theme, deployed to GitHub Pages.

- **Stack**: Hugo (Go-based SSG) + Doks theme (`@thulite/doks-core`) + Node.js + SCSS
- **Deployment**: GitHub Pages via GitHub Actions (push to `main`)
- **License**: Apache 2.0
- **Reference implementation**: [complytime-website](https://github.com/complytime/complytime-website) uses the identical stack

## Build & Dev Commands

```bash
npm install                # Install all dependencies (Node.js >= 20.11.0 required)
npm run dev                # Dev server with live reload at http://localhost:1313/
npm run build              # Production build (output -> public/)
npm run preview            # Preview production build locally
npm run format             # Format all files with Prettier
```

Hugo is installed via npm (through `@thulite`). Go >= 1.23 is required for Hugo module resolution.

There is no test suite. Validation is manual:
1. `npm run build` must succeed without errors
2. `npm run dev` -- visually verify pages render correctly
3. Check for broken links, missing images, correct navigation
4. Verify dark mode renders correctly (Doks supports auto/light/dark)

## Project Structure

```
website/
├── .specify/
│   ├── memory/
│   │   └── constitution.md        # Governance document (highest authority)
│   ├── specs/                     # Feature specifications (numbered)
│   ├── templates/                 # Templates for spec artifacts
│   └── scripts/bash/              # Automation scripts
├── assets/scss/common/
│   ├── _variables-custom.scss     # Theme colors, fonts (Bootstrap vars)
│   └── _custom.scss               # Additional custom CSS rules
├── config/_default/
│   ├── hugo.toml                  # Core Hugo settings (title, baseURL)
│   ├── params.toml                # Doks theme parameters
│   └── menus/menus.en.toml        # Navigation menu definitions
├── content/                       # All Markdown content
│   ├── _index.md                  # Homepage frontmatter only
│   └── docs/                      # Documentation sections
│       ├── getting-started/       # What is Unbound Force, quick start
│       ├── projects/              # Project pages (Gaze, etc.)
│       ├── team/                  # Agent persona pages
│       └── contributing/          # How to contribute
├── layouts/
│   └── home.html                  # Custom homepage template
├── static/                        # Static assets (favicons, icons)
├── .github/workflows/
│   └── deploy-gh-pages.yml        # CI/CD: build + deploy on push to main
├── package.json                   # Node dependencies and npm scripts
├── go.mod                         # Hugo module resolution
└── AGENTS.md
```

## Content Conventions

### Frontmatter (Required)

Every `.md` file in `content/` requires YAML frontmatter:

```yaml
---
title: "Page Title"
description: "Short description for SEO and previews."
lead: "Brief intro shown at the top of the page."
date: 2026-01-01T00:00:00+00:00
draft: false
weight: 100
toc: true
---
```

- `weight` controls sidebar ordering (lower number = higher position).
- `toc: true` generates a table of contents from headings.
- `draft: true` excludes the page from the built site.
- Section index pages (folder landing pages) must be named `_index.md`.

### Markdown Rules

- Start body content with `##` (H2). The `title` frontmatter generates H1.
- Use ATX-style headings (`##`, not underlines).
- Use fenced code blocks with language identifiers (e.g., ` ```go `).
- No placeholder or "Coming Soon" content. Only publish pages with real content.

### Navigation

Edit `config/_default/menus/menus.en.toml` to add or modify top-level navigation items.

### Homepage

The homepage is built from two files:
1. `content/_index.md` -- frontmatter only (title, description, lead text)
2. `layouts/home.html` -- full HTML template (hero, features, projects, CTA)

## Code Style Guidelines

### Commit Messages

Use Conventional Commits: `type: description`

Valid types: `feat:`, `fix:`, `docs:`, `chore:`, `style:`, `refactor:`, `ci:`

Examples:
- `feat: add Gaze project page`
- `fix: correct broken link in team navigation`
- `docs: update getting started guide`

### SCSS / CSS

- Custom variables go in `assets/scss/common/_variables-custom.scss`.
- Custom rules go in `assets/scss/common/_custom.scss`.
- Use Bootstrap CSS variables (`var(--bs-*)`) for dark mode compatibility.
- Do not write custom CSS when the Doks theme provides the feature natively.
- Check [Doks docs](https://getdoks.org/) before adding custom styles.

### Hugo Templates

- Use `{{ with .Param "key" }}` for nil-safe access to optional values.
- Comment complex template logic with `{{/* explanation */}}`.
- Prefer Doks built-in partials and shortcodes over custom layouts.
- Only override theme templates when the built-in behavior is insufficient.

### Configuration (TOML)

- All Hugo config lives in `config/_default/`.
- Use TOML format (not YAML or JSON) for Hugo configuration files.
- Keep `hugo.toml` minimal; theme-specific settings go in `params.toml`.

## Behavioral Constraints

These rules are non-negotiable:

1. **Zero-Waste Mandate**: No unused pages, dead links, orphaned assets, or placeholder content.
2. **Neighborhood Rule**: Changes to navigation, layout, or styling must be verified against ALL existing pages. Run `npm run build` and check for regressions.
3. **Content Accuracy**: Project descriptions must be derived from actual repository content (README.md, AGENTS.md, specs). Never fabricate features or overstate maturity.
4. **Minimal Maintenance**: Prefer Markdown over custom HTML. Use Doks built-in features before writing custom layouts or CSS.

## Speckit Workflow (Mandatory)

All non-trivial feature work **must** go through the Speckit pipeline. The constitution (`.specify/memory/constitution.md`) is the highest-authority document in this project — all work must align with it.

### Pipeline

The workflow is a strict, sequential pipeline. Each stage has a corresponding `/speckit.*` command:

```
constitution → specify → clarify → plan → tasks → analyze → checklist → implement
```

| Command | Purpose |
|---------|---------|
| `/speckit.constitution` | Create or update the project constitution |
| `/speckit.specify` | Create a feature specification from a description |
| `/speckit.clarify` | Reduce ambiguity in the spec before planning |
| `/speckit.plan` | Generate the technical implementation plan |
| `/speckit.tasks` | Generate actionable, dependency-ordered task list |
| `/speckit.analyze` | Non-destructive cross-artifact consistency analysis |
| `/speckit.checklist` | Generate requirement quality validation checklists |
| `/speckit.implement` | Execute the implementation plan task by task |
| `/speckit.taskstoissues` | Convert tasks.md into GitHub Issues |

### Ordering Constraints

1. Constitution must exist before specs.
2. Spec must exist before plan.
3. Plan must exist before tasks.
4. Tasks must exist before implementation and analysis.
5. Clarify should run before plan (skipping increases rework risk).
6. Analyze should run after tasks but before implementation.
7. All checklists must pass before implementation (or user must explicitly override).

### Spec Organization

Specs are numbered with 3-digit zero-padded prefixes and stored under `.specify/specs/`:

```
.specify/
  memory/
    constitution.md              # Governance document (highest authority)
  specs/
    001-feature-name/            # spec.md, plan.md, tasks.md
    002-another-feature/         # spec.md
  templates/                     # Templates for all artifact types
  scripts/bash/                  # Automation scripts
```

Branch names follow the same numbering pattern (e.g., `001-feature-name`).

### Task Completion Bookkeeping

When a task from `tasks.md` is completed during implementation, its checkbox **must** be updated from `- [ ]` to `- [x]` immediately. Do not defer this — mark tasks complete as they are finished, not in a batch after all work is done. This keeps the task list an accurate, real-time view of progress and prevents drift between the codebase and the plan.

### Spec Commit Gate

All spec artifacts (`spec.md`, `plan.md`, `tasks.md`, and any other files under `.specify/specs/`) **must** be committed and pushed before implementation begins. This ensures the planning record is preserved in version control before code changes start, and provides a clean baseline to diff against if implementation drifts from the plan. Run `/speckit.implement` only after the spec commit is on the remote.

### Constitution Check

A mandatory gate at the planning phase. The constitution's core principles must each receive a PASS before proceeding. Constitution violations are automatically CRITICAL severity and non-negotiable.

## Content Sources

Project content is sourced from upstream repositories. Cross-reference before updating:

| Project | Repository | Key Files |
|---------|------------|-----------|
| Gaze | `unbound-force/gaze` | `README.md`, `AGENTS.md`, `.specify/` specs |
| Unbound Force (personas) | `unbound-force/unbound-force` | `unbound-force.md` |

Adapt content for a website audience -- less raw technical detail, more "why should I care" framing. Do not copy READMEs verbatim.

## CI/CD

GitHub Pages deployment via `.github/workflows/deploy-gh-pages.yml`:

- **Trigger**: Push to `main` branch
- **Steps**: Install Node.js + Hugo, run `hugo --minify --gc`, deploy to GitHub Pages
- **Security**: All GitHub Actions must use SHA-pinned versions (not tags)

## Common Tasks

### Adding a new documentation page

1. Create `content/docs/<section>/page-name.md` with required frontmatter.
2. Set `weight` relative to sibling pages for correct sidebar ordering.
3. Run `npm run dev` and verify the page renders and appears in navigation.
4. Run `npm run build` to confirm no build errors.

### Adding a new project page

1. Create `content/docs/projects/<project>.md` with frontmatter.
2. Source content from the project's repository (README, specs).
3. Add a project card to `layouts/home.html` if it should appear on the homepage.
4. Update `config/_default/menus/menus.en.toml` if top-level nav is needed.

### Modifying styles

1. For color/font changes: edit `assets/scss/common/_variables-custom.scss`.
2. For custom rules: edit `assets/scss/common/_custom.scss`.
3. Always verify both light and dark mode rendering after style changes.

## Active Technologies
- Hugo (via npm/thulite) + Go 1.23 (module resolution) + Node.js >= 20.11.0 + `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1` (001-site-scaffold)
- N/A (static site, no database) (001-site-scaffold)

## Recent Changes
- 001-site-scaffold: Added Hugo (via npm/thulite) + Go 1.23 (module resolution) + Node.js >= 20.11.0 + `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1`
