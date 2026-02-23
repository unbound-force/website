# Tasks: Site Scaffold and Deployment

**Input**: Design documents from `/specs/001-site-scaffold/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, quickstart.md

**Tests**: No automated tests requested. Validation is manual (`npm run build` + visual inspection).

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Static site (Hugo SSG)**: `config/`, `content/`, `layouts/`, `assets/`, `static/` at repository root
- No `src/`, `tests/`, or `backend/` directories (this is a static site, not an application)

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Initialize the Hugo + Doks/Thulite project with all package dependencies and core project files.

- [x] T001 Create `package.json` at repository root with Doks/Thulite dependencies (`thulite ^2.6.3`, `@thulite/doks-core ^1.8.3`, `@thulite/images ^3.3.1`, `@thulite/inline-svg ^1.2.0`, `@thulite/seo ^2.4.1`, `@tabler/icons ^3.34.1`), dev dependencies (`prettier ^3.6.2`, `vite ^7.0.6`), and npm scripts (`dev`, `build`, `preview`, `format`). Use exact values from research.md Section 1.
- [x] T002 Create `go.mod` at repository root with module path `github.com/unbound-force/website`, Go version 1.23, and `gopkg.in/yaml.v3 v3.0.1` dependency. Create corresponding `go.sum` with checksums from research.md Section 1.
- [x] T003 Create `.gitignore` at repository root excluding `public/`, `resources/_gen/`, `.hugo_build.lock`, `hugo_stats.json`, `node_modules/`, `.DS_Store`, `Thumbs.db`, `.idea/`, `*.swp`, `*.swo`, `*~`, `.env`, `.netlify`. Adapt from complytime-website pattern in research.md Section 8, removing complytime-specific entries.
- [x] T004 Run `npm install` to generate `node_modules/` and `package-lock.json`. Verify all dependencies install without errors.

**Checkpoint**: Project dependencies installed. `node_modules/` populated. No Hugo config yet — site will not build.

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Hugo configuration and module mounts that MUST be complete before any content or templates work.

**CRITICAL**: No user story work can begin until this phase is complete. Hugo cannot build without these files.

- [x] T005 Create `config/_default/hugo.toml` with core settings: `title = "Unbound Force"`, `baseurl = "https://unboundforce.dev/"`, `disableAliases = true`, `disableHugoGeneratorInject = true`, `enableEmoji = true`, `enableGitInfo = false`, `enableRobotsTXT = true`, `languageCode = "en-US"`, `copyRight`, multilingual config (disable de/nl), output formats (`searchIndex`, `SITEMAP`), sitemap, caches, taxonomies, permalinks, minify, pagination, related content, and imaging settings. Use data-model.md Entity 1 and research.md Section 1 hugo.toml reference as source.
- [x] T006 Create `config/_default/module.toml` with Hugo module mount declarations for all Thulite packages: content, data (doks-core + local), layouts (local + doks-core with `excludeFiles = "home.html"` + thulite core + seo + images + inline-svg), i18n, archetypes, assets (thulite core + doks-core + tabler icons + images + local), and static (doks-core + local). Use exact mount structure from research.md Section 2.
- [x] T007 Create `config/_default/params.toml` with Doks theme parameters: `title = "Unbound Force"`, description, `[doks]` section with `colorMode = "auto"`, `navbarSticky = true`, `flexSearch = true`, `docsRepo = "https://github.com/unbound-force/website"`, brand colors (`textDark = "#e2e8f0"`, `accentDark = "#818cf8"`, `textLight = "#0f172a"`, `accentLight = "#3b82f6"`), and all other Doks settings. Use data-model.md Entity 2 and research.md Section 1 params.toml reference. Do NOT include Google Fonts import (FR-013 prohibits external fonts).
- [x] T008 Create `config/_default/menus/menus.en.toml` with navigation menus: `[[main]]` (Docs link), `[[social]]` (GitHub org link to `https://github.com/unbound-force`), `[[docs]]` entries (Getting Started, Projects, Team, Contributing with appropriate weights and identifiers), and `[[footer]]` links. Use data-model.md Entity 3.
- [x] T009 Create `content/_index.md` with homepage frontmatter only (title: "Unbound Force", description, lead text, date, `draft: false`). No body content — the homepage template handles all rendering. Use data-model.md Entity 4.
- [x] T010 Verify Hugo builds successfully by running `npm run build`. The build should complete without errors and produce output in `public/`. This validates that module mounts, config, and minimal content are correctly wired.

**Checkpoint**: Foundation ready — Hugo builds successfully with Doks theme. Default Doks homepage renders (no custom template yet). User story implementation can now begin.

---

## Phase 3: User Story 1 — Site Builds and Deploys (Priority: P1) MVP

**Goal**: A contributor can clone the repo, run `npm install && npm run build`, and get a successful production build. Dev server and preview also work.

**Independent Test**: Run `npm install && npm run build` from a clean clone. Build completes without errors, produces `public/`. Run `npm run dev` — server starts at localhost:1313. Run `npm run preview` — preview works.

### Implementation for User Story 1

- [x] T011 [US1] Verify `npm run build` produces a clean production build in `public/` with zero errors and zero warnings. If any warnings appear from Phase 2 config, fix them now.
- [x] T012 [US1] Verify `npm run dev` starts a local dev server at `http://localhost:1313/` with live reload functional. If errors occur, fix Hugo config or module mount issues.
- [x] T013 [US1] Verify `npm run preview` previews the production build locally via Vite. Run `npm run build` first, then `npm run preview`.
- [x] T014 [US1] Verify `npm run format` runs Prettier without errors across all project files.

**Checkpoint**: User Story 1 complete. All four npm scripts work. A contributor can clone, install, build, dev, preview, and format without errors.

---

## Phase 4: User Story 2 — Homepage Communicates Value (Priority: P1)

**Goal**: The homepage renders a hero section, features cards, Gaze project card, and closing CTA. All sections work in both light and dark mode.

**Independent Test**: Load the homepage in a browser. Verify hero (title, lead, CTAs), features (4 cards), projects (Gaze card with accurate description), and CTA sections are visible. Toggle dark mode — all sections render correctly.

### Implementation for User Story 2

- [x] T015 [US2] Create `layouts/home.html` with the homepage template using three Hugo blocks: `{{ define "main" }}` (hero section with badge "Open Source AI Agent Swarm", title from frontmatter `{{ .Title }}`, lead from `{{ .Params.lead }}`, CTA buttons "Get Started" and "View on GitHub"), `{{ define "sidebar-prefooter" }}` (features section with 4 cards — Agent Personas, Speckit Workflow, Quality-First, Open Source; projects section with 1 Gaze card; closing CTA section), and `{{ define "sidebar-footer" }}` (conditional footer CTA). Use research.md Section 3 for block structure and Section 4 for Gaze description accuracy. Gaze card MUST describe: "Static analysis for Go that detects observable side effects in functions and computes CRAP scores combining cyclomatic complexity with test coverage." Do NOT mention GazeCRAP, transitive analysis, or P3-P4 detection. Use Tabler icons via `{{ partial "inline-svg" (dict "src" "tabler-icons/outline/icon-name") }}`. All elements MUST have WCAG 2.1 AA compliant markup (alt text, ARIA labels, keyboard-navigable interactive elements, semantic HTML).
- [x] T016 [US2] Verify the homepage renders all four sections correctly in a browser by running `npm run dev` and navigating to `http://localhost:1313/`. Check: hero has title + lead + 2 CTA buttons, features shows 4 cards, projects shows Gaze card, CTA section has action links.
- [x] T017 [US2] Verify dark mode rendering by toggling the Doks color mode switch. All sections should render correctly with appropriate colors in both light and dark themes. No elements should become invisible or unreadable.

**Checkpoint**: User Story 2 complete. Homepage communicates value with all four sections. Both light and dark mode work correctly.

---

## Phase 5: User Story 3 — CI/CD Deploys Automatically (Priority: P1)

**Goal**: Pushes to `main` automatically build and deploy the site to GitHub Pages via GitHub Actions.

**Independent Test**: Inspect `.github/workflows/deploy-gh-pages.yml` — all actions are SHA-pinned. Workflow structure matches: checkout → setup-node → setup-hugo → npm ci → hugo build → upload artifact → deploy.

### Implementation for User Story 3

- [x] T018 [US3] Create `.github/workflows/deploy-gh-pages.yml` with GitHub Actions workflow: trigger on push to `main` + `workflow_dispatch`, permissions (`contents: read`, `pages: write`, `id-token: write`), concurrency group `"pages"`, build job (checkout with SHA `de0fac2e4500dabe0009e67214ff5f5447ce83dd`, setup-node with SHA `6044e13b5dc448c55e2357c09f80417699197238` for Node 22, peaceiris/actions-hugo with SHA `75d2e84710de30f6ff7268e08f310b60ef14033f` for Hugo 0.155.1 extended, `npm ci`, `hugo --minify --gc --baseURL "https://unboundforce.dev/"`, upload-pages-artifact with SHA `7b1f4a764d45c48632c6b24a0339c27f5614fb0b`), and deploy job (deploy-pages with SHA `d6db90164ac5ed86f2b6aed7e0febac5b3c0c03e`). Use exact SHA pins from research.md Section 5.
- [x] T019 [US3] Verify all GitHub Action references in the workflow use SHA-pinned versions (no mutable tags like `@v4`). Inspect each `uses:` line and confirm it contains a full 40-character SHA hash.

**Checkpoint**: User Story 3 complete. CI/CD workflow is ready. Deployment will activate when code is pushed to `main`.

---

## Phase 6: User Story 4 — Custom Domains Work (Priority: P2)

**Goal**: The site is accessible via `unboundforce.dev` with HTTPS. CNAME file in the repo configures GitHub Pages custom domain.

**Independent Test**: Verify `static/CNAME` contains exactly `unboundforce.dev`. After deployment, navigate to `https://unboundforce.dev/` and confirm the site loads.

### Implementation for User Story 4

- [x] T020 [US4] Create `static/CNAME` containing exactly one line: `unboundforce.dev` (no trailing newline or whitespace). This configures GitHub Pages to serve the site on the custom domain.
- [x] T021 [US4] Verify the CNAME file is included in the build output by running `npm run build` and checking that `public/CNAME` exists with correct content.

**Checkpoint**: User Story 4 complete. Custom domain configuration is in place. DNS configuration and redirect setup for alternative domains are out of scope (handled via DNS registrar per clarification).

---

## Phase 7: User Story 5 — Brand Styling Applied (Priority: P2)

**Goal**: The site has the Unbound Force brand colors (electric blue primary, violet accent) applied consistently in both light and dark modes. No external fonts.

**Independent Test**: Load the site and verify electric blue (#3b82f6) is the primary color and violet (#8b5cf6) is the accent. Toggle dark mode — colors adjust appropriately. Check browser network tab — no external font requests.

### Implementation for User Story 5

- [x] T022 [P] [US5] Create `assets/scss/common/_variables-custom.scss` with Bootstrap variable overrides: `$primary: #3b82f6` (electric blue). Define brand color variables for use in custom styles. Include `.feature-card` and `.project-card` component styles with hover effects (border highlight, subtle translateY, box-shadow) for both light and dark modes using `[data-bs-theme="dark"]` selector. Keep scope minimal — only variables and card component styles. Do NOT add Google Fonts imports or external font references. Use data-model.md Entity 6 and research.md Section 6.
- [x] T023 [P] [US5] Create `assets/scss/common/_custom.scss` with homepage section styles: hero section spacing, features section padding, projects section styling, CTA section layout, and responsive adjustments. Only include styles that Doks does not provide natively. Do NOT import external fonts. Keep scope minimal per Constitution Principle II (Minimal Footprint).
- [x] T024 [US5] Verify brand colors render correctly in both light and dark modes by running `npm run dev` and inspecting the site. Confirm: primary color is electric blue (#3b82f6) in light mode, accent elements use violet, dark mode adjusts appropriately. Check that no external font requests appear in the browser network tab.
- [x] T025 [US5] Run a final `npm run build` and verify zero errors, zero warnings. The build confirms all SCSS compiles correctly and no broken references exist.

**Checkpoint**: User Story 5 complete. Brand styling applied, both modes work, no external fonts loaded.

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Final validation, static assets, and cross-cutting quality checks.

- [x] T026 [P] Create `static/favicon.png` and `static/favicon-512x512.png` placeholder favicons for the site. These should be simple Unbound Force-branded icons (can be minimal/geometric for now).
- [x] T027 Verify WCAG 2.1 AA compliance: check all homepage elements for alt text on images, keyboard navigability of interactive elements (CTA buttons, dark mode toggle, nav links), and color contrast ratio >= 4.5:1 for all text against backgrounds in both light and dark modes. Fix any violations found.
- [x] T028 Run `npm run build` one final time and verify: zero errors, zero warnings, `public/` contains all expected files (index.html, CNAME, favicons, CSS, search index).
- [x] T029 Run `npm run dev` and perform a full visual walkthrough: homepage hero, features, projects, CTA sections render correctly. Navigation works. Dark mode toggle works. Search is functional (though no docs content yet). No console errors.
- [x] T030 Run quickstart.md verification checklist: confirm all items pass (npm install, npm run build, npm run dev, homepage sections visible, dark mode works, no external fonts). Note: Lighthouse >= 90 (SC-009) requires a running server + Chrome and is deferred to post-deployment validation. Verify locally if possible by running `npx lighthouse http://localhost:1313 --output=json` during `npm run dev`, otherwise validate after first GitHub Pages deployment.

**Checkpoint**: All stories complete. Site is ready for deployment to GitHub Pages.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion (T004 npm install) — BLOCKS all user stories
- **User Story 1 (Phase 3)**: Depends on Foundational (Phase 2) — verification of build/dev/preview
- **User Story 2 (Phase 4)**: Depends on Foundational (Phase 2) — can start after T010
- **User Story 3 (Phase 5)**: No dependency on other stories — can start after Phase 1 (only needs package.json)
- **User Story 4 (Phase 6)**: No dependency on other stories — can start after Phase 1
- **User Story 5 (Phase 7)**: Depends on Foundational (Phase 2) — needs Hugo config for SCSS compilation
- **Polish (Phase 8)**: Depends on all user stories being complete

### User Story Dependencies

- **US1 (Site Builds)**: Depends on Phase 2 only. No dependency on other stories.
- **US2 (Homepage)**: Depends on Phase 2 only. No dependency on other stories. Independent of US5 styling (styling enhances but doesn't block).
- **US3 (CI/CD)**: Depends on Phase 1 only (needs package.json for npm ci). Fully independent of all other stories.
- **US4 (Custom Domain)**: Depends on Phase 1 only. Fully independent. Just a CNAME file.
- **US5 (Brand Styling)**: Depends on Phase 2 (needs Hugo config for SCSS compilation). Independent of US2 content.

### Within Each User Story

- Configuration/files before verification
- Verification tasks run sequentially after their prerequisites
- Story complete before checkpoint

### Parallel Opportunities

- **Phase 1**: T001, T002, T003 can all run in parallel (different files, no dependencies)
- **Phase 2**: T005, T006, T007, T008 can all run in parallel (different config files). T009 can also parallel. T010 depends on all prior Phase 2 tasks.
- **After Phase 2**: US1, US2, US3, US4, US5 can all start in parallel (if team capacity allows)
- **Phase 5 (US3)**: Can start as early as after Phase 1 (only needs package.json context)
- **Phase 6 (US4)**: Can start as early as after Phase 1 (just a CNAME file)
- **Phase 7 (US5)**: T022 and T023 can run in parallel (different SCSS files)

---

## Parallel Example: Phase 2 (Foundational)

```bash
# Launch all config files in parallel (different files, no dependencies):
Task: "Create config/_default/hugo.toml"       # T005
Task: "Create config/_default/module.toml"     # T006
Task: "Create config/_default/params.toml"     # T007
Task: "Create config/_default/menus/menus.en.toml"  # T008
Task: "Create content/_index.md"               # T009

# Then sequentially:
Task: "Verify Hugo builds successfully"        # T010 (depends on all above)
```

## Parallel Example: User Stories After Phase 2

```bash
# All user stories can proceed in parallel after foundational phase:
Task: "US1 - Verify build scripts"    # T011-T014
Task: "US2 - Create homepage template" # T015-T017
Task: "US3 - Create CI/CD workflow"   # T018-T019 (can start after Phase 1)
Task: "US4 - Create CNAME"            # T020-T021 (can start after Phase 1)
Task: "US5 - Create brand SCSS"       # T022-T025
```

---

## Implementation Strategy

### MVP First (User Stories 1 + 2 + 3)

1. Complete Phase 1: Setup (T001-T004)
2. Complete Phase 2: Foundational (T005-T010)
3. Complete Phase 3: US1 — Site Builds (T011-T014)
4. **STOP and VALIDATE**: Site builds and all npm scripts work
5. Complete Phase 4: US2 — Homepage (T015-T017)
6. **STOP and VALIDATE**: Homepage renders all 4 sections correctly
7. Complete Phase 5: US3 — CI/CD (T018-T019)
8. **STOP and VALIDATE**: Workflow file ready for deployment
9. Deploy MVP to GitHub Pages

### Incremental Delivery

1. Setup + Foundational + US1 → Site builds locally (MVP foundation)
2. Add US2 → Homepage communicates value (MVP visible)
3. Add US3 → CI/CD ready (MVP deployable)
4. Add US4 → Custom domain configured (brand identity)
5. Add US5 → Brand styling applied (polish)
6. Polish phase → Final validation (release candidate)
7. Each story adds value without breaking previous stories

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story is independently completable and testable
- No automated tests — validation is manual (build + visual inspection)
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Gaze project card content MUST be verified against actual repository README (Constitution Principle I)
- No external fonts (FR-013) — do NOT add Google Fonts imports in SCSS
- SCSS limited to exactly two files: `_variables-custom.scss` and `_custom.scss` (FR-012)
