# AGENTS.md

## Project Overview

The Unbound Force website is the public-facing site for the [Unbound Force](https://github.com/unbound-force) organization -- a superhero-themed AI agent swarm for software engineering. Built with Hugo + Doks/Thulite theme, deployed to GitHub Pages.

- **Stack**: Hugo (Go-based SSG) + Doks theme (`@thulite/doks-core`) + Node.js + SCSS
- **Deployment**: GitHub Pages via GitHub Actions (push to `main`)
- **License**: Apache 2.0
- **Reference implementation**: [complytime-website](https://github.com/complytime/complytime-website) uses the identical stack

## Core Mission

- **Strategic Architecture**: Engineers shift from manual coding to directing an "infinite supply of junior developers" (AI agents).
- **Outcome Orientation**: Focus on conveying business value and user intent rather than low-level technical sub-tasks.
- **Intent-to-Context**: Treat specs and rules as the medium through which human intent is manifested into code.

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
│   ├── templates/                 # Templates for spec artifacts
│   └── scripts/bash/              # Automation scripts
├── specs/                         # Feature specifications (numbered)
│   ├── 001-feature-name/          # spec.md, plan.md, tasks.md
│   └── 002-another-feature/       # spec.md
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
5. **Dewey-First Exploration**: When exploring cross-repo content, researching upstream tool behavior, or searching for context across sibling repositories, prefer Dewey MCP tools (`dewey_dewey_semantic_search`, `dewey_get_page`, `dewey_search`, `dewey_find_by_tag`) over grep, glob, or spawning explore subagents. Dewey indexes all sibling repos (`../gaze`, `../dewey`, `../unbound-force`), the org workspace (`../`), and GitHub issues/PRs — grep only sees the current repo. Use Dewey for discovery, then Read for targeted file access once you know the path.

### Gatekeeping Value Protection

Agents MUST NOT modify values that serve as quality or governance gates to make an implementation pass. The following categories are protected:

1. **Coverage thresholds and CRAP scores** — minimum coverage percentages, CRAP score limits, coverage ratchets
2. **Severity definitions and auto-fix policies** — CRITICAL/HIGH/MEDIUM/LOW boundaries, auto-fix eligibility rules
3. **Convention pack rule classifications** — MUST/SHOULD/MAY designations on convention pack rules (downgrading MUST to SHOULD is prohibited)
4. **CI flags and linter configuration** — `-race`, `-count=1`, `govulncheck`, `golangci-lint` rules, pinned action SHAs
5. **Agent temperature and tool-access settings** — frontmatter `temperature`, `tools.write`, `tools.edit`, `tools.bash` restrictions
6. **Constitution MUST rules** — any MUST rule in `.specify/memory/constitution.md` or hero constitutions
7. **Review iteration limits and worker concurrency** — max review iterations, max concurrent Swarm workers, retry limits
8. **Workflow gate markers** — `<!-- spec-review: passed -->`, task completion checkboxes used as gates, phase checkpoint requirements

**What to do instead**: When an implementation cannot meet a gate, the agent MUST stop, report which gate is blocking and why, and let the human decide whether to adjust the gate or rework the implementation. Modifying a gate without explicit human authorization is a constitution violation (CRITICAL severity).

### Workflow Phase Boundaries

Agents MUST NOT cross workflow phase boundaries:

- **Specify/Clarify/Plan/Tasks/Analyze/Checklist** phases: spec artifacts ONLY (`specs/NNN-*/` directory). No source code, test, agent, command, or config changes.
- **Implement** phase: source code changes allowed, guided by spec artifacts.
- **Review** phase: findings and minor fixes only. No new features.

A phase boundary violation is treated as a process error. The agent MUST stop and report the violation rather than proceeding with out-of-phase changes.

### CI Parity Gate

Before marking any implementation task complete or declaring a PR ready, agents MUST replicate the CI checks locally. Read `.github/workflows/` to identify the exact commands CI runs, then execute those same commands. Any failure is a blocking error — a task is not complete until all CI-equivalent checks pass locally. Do not rely on a memorized list of commands; always derive them from the workflow files, which are the source of truth.

### Review Council as PR Prerequisite

Before submitting a pull request, agents **must** run `/review-council` and resolve all REQUEST CHANGES findings until all reviewers return APPROVE. There must be **minimal to no code changes** between the council's APPROVE verdict and the PR submission — the council reviews the final code, not a draft that changes afterward.

Workflow:

1. Complete all implementation tasks
2. Run CI checks locally (build, test, vet)
3. Run `/review-council` — fix any findings, re-run until APPROVE
4. Commit, push, and submit PR immediately after council APPROVE
5. Do NOT make further code changes between APPROVE and PR submission

Exempt from council review:

- Constitution amendments (governance documents, not code)
- Documentation-only changes (README, AGENTS.md, spec artifacts)
- Emergency hotfixes (must be retroactively reviewed)

## Spec-First Development

All changes that modify production code, test code, agent prompts, embedded assets, or CI configuration **must** be preceded by a spec workflow. The constitution (`.specify/memory/constitution.md`) is the highest-authority document in this project — all work must align with it.

Two spec workflows are available:

| Workflow     | Location                 | Best For                                      |
| ------------ | ------------------------ | --------------------------------------------- |
| **Speckit**  | `specs/NNN-name/`        | Numbered feature specs with the full pipeline |
| **OpenSpec** | `openspec/changes/name/` | Targeted changes with lightweight artifacts   |

**What requires a spec** (no exceptions without explicit user override):

- New features or capabilities
- Refactoring that changes function signatures, extracts helpers, or moves code between packages
- Test additions or assertion strengthening across multiple functions
- Agent prompt changes
- CI workflow modifications
- Data model changes (new struct fields, schema updates)

**What is exempt** (may be done directly):

- Constitution amendments (governed by the constitution's own Governance section)
- Typo corrections, comment-only changes, single-line formatting fixes
- Emergency hotfixes for critical production bugs (must be retroactively documented)

When an agent is unsure whether a change is trivial, it **must** ask the user rather than proceeding without a spec. The cost of an unnecessary spec is minutes; the cost of an unplanned change is rework, drift, and broken CI.

### Website Documentation Gate

When a change affects user-facing behavior, hero capabilities, CLI commands, or workflows, a GitHub issue **MUST** be created in the `unbound-force/website` repository to track required documentation or website updates. The issue must be created before the implementing PR is merged.

```bash
gh issue create --repo unbound-force/website \
  --title "docs: <brief description of what changed>" \
  --body "<what changed, why it matters, which pages need updating>"
```

**Exempt changes** (no website issue needed):

- Internal refactoring with no user-facing behavior change
- Test-only changes
- CI/CD pipeline changes
- Spec artifacts (specs are internal planning documents)

**Examples requiring a website issue**:

- New CLI command or flag added
- Hero capabilities changed (new agent, removed feature)
- Installation steps changed (`uf setup` flow)
- New convention pack added
- Breaking changes to any user-facing workflow

## Speckit Workflow (Mandatory)

All non-trivial feature work **must** go through the Speckit pipeline. The constitution (`.specify/memory/constitution.md`) is the highest-authority document in this project — all work must align with it.

### Pipeline

The workflow is a strict, sequential pipeline. Each stage has a corresponding `/speckit.*` command:

```
constitution → specify → clarify → plan → tasks → analyze → checklist → implement
```

| Command                  | Purpose                                             |
| ------------------------ | --------------------------------------------------- |
| `/speckit.constitution`  | Create or update the project constitution           |
| `/speckit.specify`       | Create a feature specification from a description   |
| `/speckit.clarify`       | Reduce ambiguity in the spec before planning        |
| `/speckit.plan`          | Generate the technical implementation plan          |
| `/speckit.tasks`         | Generate actionable, dependency-ordered task list   |
| `/speckit.analyze`       | Non-destructive cross-artifact consistency analysis |
| `/speckit.checklist`     | Generate requirement quality validation checklists  |
| `/speckit.implement`     | Execute the implementation plan task by task        |
| `/speckit.taskstoissues` | Convert tasks.md into GitHub Issues                 |

### Ordering Constraints

1. Constitution must exist before specs.
2. Spec must exist before plan.
3. Plan must exist before tasks.
4. Tasks must exist before implementation and analysis.
5. Clarify should run before plan (skipping increases rework risk).
6. Analyze should run after tasks but before implementation.
7. All checklists must pass before implementation (or user must explicitly override).

### Spec Organization

Specs are numbered with 3-digit zero-padded prefixes and stored under `specs/`:

```
specs/
  001-feature-name/              # spec.md, plan.md, tasks.md
  002-another-feature/           # spec.md

.specify/
  memory/
    constitution.md              # Governance document (highest authority)
  templates/                     # Templates for all artifact types
  scripts/bash/                  # Automation scripts
```

Branch names follow the same numbering pattern (e.g., `001-feature-name`).

### Branch Conventions

Both tiers enforce branch-based workflows:

- **Speckit** branches: `NNN-<short-name>`
  (e.g., `013-binary-rename`). Created automatically by
  `/speckit.specify`. Validated by `check-prerequisites.sh`
  at every pipeline step (hard gate).
- **OpenSpec** branches: `opsx/<change-name>`
  (e.g., `opsx/doctor-ux-improvement`). Created by
  `/opsx-propose`. Validated by `/opsx-apply` before
  implementation (hard gate).

The `opsx/` prefix namespace ensures OpenSpec branches
are visually distinct from Speckit branches in
`git branch` output and do not collide with the
`NNN-*` numbering pattern.

### Task Completion Bookkeeping

When a task from `tasks.md` is completed during implementation, its checkbox **must** be updated from `- [ ]` to `- [x]` immediately. Do not defer this — mark tasks complete as they are finished, not in a batch after all work is done. This keeps the task list an accurate, real-time view of progress and prevents drift between the codebase and the plan.

### Spec Commit Gate

All spec artifacts (`spec.md`, `plan.md`, `tasks.md`, and any other files under `specs/`) **must** be committed and pushed before implementation begins. This ensures the planning record is preserved in version control before code changes start, and provides a clean baseline to diff against if implementation drifts from the plan. Run `/speckit.implement` only after the spec commit is on the remote.

### Constitution Check

A mandatory gate at the planning phase. The constitution's core principles must each receive a PASS before proceeding. Constitution violations are automatically CRITICAL severity and non-negotiable.

## Content Sources

Project content is sourced from upstream repositories. Cross-reference before updating:

| Project                  | Repository                    | Key Files                                        |
| ------------------------ | ----------------------------- | ------------------------------------------------ |
| Gaze                     | `unbound-force/gaze`          | `README.md`, `AGENTS.md`, `.specify/` specs      |
| Dewey                    | `unbound-force/dewey`         | `README.md`, `dewey-design-paper.md` (org-level) |
| Unbound Force (personas) | `unbound-force/unbound-force` | `unbound-force.md`                               |

Adapt content for a website audience -- less raw technical detail, more "why should I care" framing. Do not copy READMEs verbatim.

## CI/CD

GitHub Pages deployment via `.github/workflows/deploy-gh-pages.yml`:

- **Trigger**: Push to `main` branch
- **Steps**: Install Node.js + Hugo, run `hugo --minify --gc`, deploy to GitHub Pages
- **Security**: All GitHub Actions must use SHA-pinned versions (not tags)

## Knowledge Retrieval

Agents SHOULD prefer Dewey MCP tools over grep/glob/read for cross-repo context, design decisions, and architectural patterns. Dewey provides semantic search across all indexed Markdown files, specs, and web documentation — returning ranked results with provenance metadata that grep cannot match.

### Tool Selection Matrix

| Query Intent             | Dewey Tool                       | When to Use                        |
| ------------------------ | -------------------------------- | ---------------------------------- |
| Conceptual understanding | `dewey_semantic_search`          | "How does X work?"                 |
| Keyword lookup           | `dewey_search`                   | Known terms, FR numbers            |
| Read specific page       | `dewey_get_page`                 | Known document path                |
| Relationship discovery   | `dewey_find_connections`         | "How are X and Y related?"         |
| Similar documents        | `dewey_similar`                  | "Find specs like this one"         |
| Tag-based discovery      | `dewey_find_by_tag`              | "All pages tagged #decision"       |
| Property queries         | `dewey_query_properties`         | "All specs with status: draft"     |
| Filtered semantic        | `dewey_semantic_search_filtered` | Semantic search within source type |
| Graph navigation         | `dewey_traverse`                 | Dependency chain walking           |

### When to Fall Back to grep/glob/read

Use direct file operations instead of Dewey when:

- **Dewey is unavailable** — MCP tools return errors or are not configured
- **Exact string matching is needed** — searching for a specific error message, variable name, or code pattern
- **Specific file path is known** — reading a file you already know the path to (use Read directly)
- **Binary/non-Markdown content** — Dewey indexes Markdown; use grep for Go source, JSON, YAML, etc.

### Graceful Degradation (3-Tier Pattern)

**Tier 3 (Full Dewey)** — semantic + structured search:

- `dewey_semantic_search` — natural language queries
- `dewey_search` — keyword queries
- `dewey_get_page`, `dewey_find_connections`, `dewey_traverse` — structured navigation
- `dewey_find_by_tag`, `dewey_query_properties` — metadata queries

**Tier 2 (Graph-only, no embedding model)** — structured search only:

- `dewey_search` — keyword queries (no embeddings needed)
- `dewey_get_page`, `dewey_traverse`, `dewey_find_connections` — graph navigation
- `dewey_find_by_tag`, `dewey_query_properties` — metadata queries
- Semantic search unavailable — use exact keyword matches

**Tier 1 (No Dewey)** — direct file access:

- Use Read tool for direct file access
- Use Grep for keyword search across the codebase
- Use Glob for file pattern matching

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

- Markdown (Hugo content file) + Hugo (via @thulite), Doks theme (`@thulite/doks-core`) (021-dewey-curator-blog)
- N/A (static Markdown file) (021-dewey-curator-blog)
- Markdown (Hugo content files) + Hugo (via @thulite), Doks theme (`@thulite/doks-core`) + N/A (static Markdown file edits only) (023-docs-v10-v11-sync)

- Hugo (Go-based SSG) via `@thulite` npm packages + `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3` (013-unleash-visibility)

- Hugo (via npm/thulite) + Go 1.23 (module resolution) + Node.js >= 20.11.0 + `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1` (001-site-scaffold)
- N/A (static site, no database) (001-site-scaffold)
- Hugo (via npm/thulite) + Go 1.23 + Node.js >= 20.11.0 + `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1` (003-blog-why-gaze)
- N/A (static site, Markdown files) (003-blog-why-gaze)
- Markdown (Hugo content files) + Hugo (via @thulite), Doks theme (005-getting-started-guides)
- N/A (static Markdown files) (005-getting-started-guides)
- Hugo (Go-based SSG) via `@thulite` npm packages; Node.js >= 20.11.0; Go >= 1.23 + `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3` (006-binary-rename)
- Hugo (via npm/thulite) + Go 1.23 + `thulite ^2.6.3`, (007-dewey-docs)
- Markdown (Hugo content files) + Hugo (via @thulite), Doks theme (012-project-docs-update)
- N/A (static Markdown files) (012-project-docs-update)

## Recent Changes

- 001-site-scaffold: Added Hugo (via npm/thulite) + Go 1.23 (module resolution) + Node.js >= 20.11.0 + `thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1`
