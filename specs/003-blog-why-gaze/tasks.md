# Tasks: Blog Section and "Why Contract Coverage" Article

**Input**: Design documents from `/specs/003-blog-why-gaze/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, quickstart.md

**Tests**: No automated test suite exists for this project. Validation is manual via `npm run build` (zero errors) and visual verification via `npm run dev`.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

---

## Phase 1: Foundational (Hugo Configuration)

**Purpose**: Register the blog section in Hugo's content pipeline, navigation, and permalink system. These changes MUST be complete before any blog content can be created.

**Why blocking**: Without these config changes, Hugo won't recognize `content/blog/` as a content section, won't generate correct URLs, and won't include blog content in RSS or search.

- [x] T001 [P] Add `"blog"` to `mainSections` in `config/_default/params.toml` — change line 7 from `mainSections = ["docs"]` to `mainSections = ["docs", "blog"]`
- [x] T002 [P] Add blog permalink pattern in `config/_default/hugo.toml` — add `blog = "/blog/:slug/"` under the existing `[permalinks]` section (after line 56)
- [x] T003 [P] Add Blog navbar entry in `config/_default/menus/menus.en.toml` — add a `[[main]]` entry with name="Blog", url="/blog/", weight=5 (before the existing GitHub link at weight 10)

**Checkpoint**: Configuration complete. Hugo will recognize blog content, generate correct URLs, and show "Blog" in the navbar.

---

## Phase 2: User Story 2 - Blog Section Infrastructure (Priority: P1)

**Goal**: Create the blog section so it builds, renders an index page, and is navigable via the navbar.

**Independent Test**: Run `npm run build` (zero errors). Run `npm run dev` and click "Blog" in the navbar — the blog index page loads with a title and description. Verify the blog section appears in the RSS feed.

- [x] T004 [US2] Create blog section index page at `content/blog/_index.md` with frontmatter: title="Blog", description="Technical articles and insights from the Unbound Force project.", lead="", date=2026-02-27T00:00:00+00:00, draft=false, weight=1, toc=false. No body content — the Doks `blog/list.html` template handles the post listing.
- [x] T005 [US2] Run `npm run build` and verify zero errors. Confirm the blog index page is generated in the build output.

**Checkpoint**: Blog section exists and builds successfully. "Blog" link in navbar navigates to the index page. No blog posts are listed yet (empty section). FR-001 through FR-005 satisfied.

---

## Phase 3: User Story 1 - "Why Contract Coverage" Article (Priority: P1) MVP

**Goal**: Publish the "Why Contract Coverage" article as the first blog post with all 4 accuracy corrections applied.

**Independent Test**: Navigate to `/blog/why-contract-coverage/`. The full article renders with all sections, code blocks with syntax highlighting, and comparison tables. Cross-reference technical claims against the Gaze repository.

- [x] T006 [US1] Create the article file at `content/blog/why-contract-coverage.md` by adapting `temp/why-gaze.md`. Add YAML frontmatter per research.md R5: title="Why Contract Coverage: How Gaze Redefines What It Means to Have Good Tests", description="Line coverage tells you what code ran, not what was verified. Contract Coverage measures whether tests check what functions are actually responsible for.", lead="Line coverage tells you what code ran. Contract Coverage tells you what was verified.", date=2026-02-27T00:00:00+00:00, draft=false, weight=100, toc=true, categories=["Engineering"], tags=["gaze", "testing", "go", "static-analysis"], contributors=["Unbound Force"]. Remove the H1 title from the body (frontmatter generates it). Ensure body starts with `##`. Ensure all code blocks use fenced syntax with language identifiers (`go` for Go code, `bash` for shell commands).
- [x] T007 [US1] Apply accuracy correction FR-009 in `content/blog/why-contract-coverage.md`: Reword the sentence about AI agent classification refinement. Change "Optionally, project documentation (architecture docs, specs, READMEs) can be fed through an AI agent to refine confidence further" to clearly state this is an optional external workflow via OpenCode agents that analyzes project documentation to refine classification, not a built-in feature of the Gaze binary.
- [x] T008 [US1] Apply accuracy correction FR-010 in `content/blog/why-contract-coverage.md`: Update the GazeCRAP formula to use consistent `/100` notation matching the CRAP formula. Change `GazeCRAP(m) = complexity^2 * (1 - contractCoverage)^3 + complexity` to `GazeCRAP(m) = complexity^2 * (1 - contractCoverage/100)^3 + complexity`. Both the CRAP and GazeCRAP formulas should use the same percentage notation.
- [x] T009 [US1] Apply accuracy correction FR-011 in `content/blog/why-contract-coverage.md`: Fix CLI command examples. Change `gaze quality ./...` to `gaze quality ./internal/analysis` (the quality command takes a single package argument, not the `./...` multi-package wildcard). Fix or remove the `gaze crap --gaze ./...` example — the `--gaze` flag does not exist. Replace with a valid example using the actual flags (e.g., `gaze crap --max-gaze-crapload=3 ./...`).
- [x] T010 [US1] Apply accuracy correction FR-012 in `content/blog/why-contract-coverage.md`: Add a note clarifying that while Gaze's core side effect detection and classification are purely static analysis (AST + SSA, no test execution), the `crap` subcommand can optionally invoke `go test -coverprofile` to generate line coverage data for the traditional CRAP formula. Add this caveat near the description of Gaze as a "static analysis tool" or in the comparison table's "Runtime Cost" column.
- [x] T011 [US1] Run `npm run build` and verify zero errors. Run `npm run dev` and verify the article renders at `/blog/why-contract-coverage/` with all sections, syntax-highlighted code blocks, and comparison tables.

**Checkpoint**: Article is published and all 4 accuracy corrections are applied. Blog index now lists the article with title, date, and description. FR-006 through FR-014 satisfied.

---

## Phase 4: User Story 3 - Homepage "Latest Articles" Section (Priority: P2)

**Goal**: Add a "Latest Articles" section to the homepage that dynamically shows the single most recent blog post.

**Independent Test**: Load the homepage. Scroll past the Projects section to find "Latest Articles" showing the blog post title, date, and description. Click through to the full article. Verify dark mode renders correctly. Verify existing homepage sections are unchanged.

- [x] T012 [US3] Add "Latest Articles" section to `layouts/home.html` between the Projects section (ending at line 111 `</section>`) and the CTA section (starting at line 113 `{{/* CTA Section */}}`). Use Hugo template logic: `{{ with site.GetPage "/blog" }}` to access the blog section, then `{{ range first 1 .Pages.ByDate.Reverse }}` to select the most recent post. Render an article card displaying the post title (`.Title`), formatted publication date (`.Date`), description (`.Params.description`), and a link to the full article (`.Permalink`). Style consistently with the existing Projects section — use the same container structure, `section-md` spacing, responsive grid (`row`, `col-lg-6`), and card styling (`rounded-4`, `p-4`, `h-100`). Use Bootstrap CSS variables (`var(--bs-*)` and semantic classes like `text-body`, `text-body-secondary`, `bg-body-tertiary`) for dark mode compatibility. Add a section heading "Latest Articles" with a lead paragraph. Include an `aria-label` attribute on the section element for accessibility.
- [x] T013 [US3] Run `npm run build` and verify zero errors. Run `npm run dev` and verify: (1) "Latest Articles" section appears between Projects and CTA, (2) article card shows title, date, and description, (3) clicking the card navigates to the article, (4) dark mode renders correctly, (5) existing homepage sections (hero, features, projects, CTA, sidebar footer) are visually unchanged.

**Checkpoint**: Homepage integration complete. The most recent blog post appears dynamically on the homepage. FR-015 through FR-018, FR-022 satisfied.

---

## Phase 5: Polish & Final Verification

**Purpose**: Confirm all success criteria are met and no regressions exist.

- [x] T014 Run `npm run build` and confirm zero errors (SC-003)
- [x] T015 Run `npm run dev` and perform full visual verification: (1) "Blog" navbar link present on all pages (SC-001), (2) blog index page lists the article with title, date, description, (3) article renders at `/blog/why-contract-coverage/` with all sections, code blocks with syntax highlighting, and tables (SC-002), (4) homepage "Latest Articles" shows the single most recent post with title, date, description, linking to the article (SC-005), (5) dark mode renders correctly on homepage, blog index, and article pages (SC-008), (6) existing pages unchanged (SC-009), (7) blog section does NOT display sidebar navigation — confirm no sidebar appears on blog index or article pages (FR-003)
- [x] T016 Verify article appears in RSS feed output — check `public/index.xml` or the RSS output for blog post entry (SC-007)
- [x] T017 Verify article appears in search index — check `public/search-index.json` for blog post entry, or use the site search in `npm run dev` to search for "contract coverage" (SC-006)

**Checkpoint**: All success criteria (SC-001 through SC-010) validated. Feature is complete.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Foundational (Phase 1)**: No dependencies — can start immediately. All 3 config tasks (T001-T003) are parallelizable (different files).
- **US2 - Blog Infrastructure (Phase 2)**: Depends on Phase 1 completion (config must be in place before creating content).
- **US1 - Article Content (Phase 3)**: Depends on Phase 2 (blog section must exist). This is the MVP delivery.
- **US3 - Homepage Integration (Phase 4)**: Depends on Phase 3 (needs at least one blog post to display dynamically).
- **Polish (Phase 5)**: Depends on all previous phases.

### User Story Dependencies

- **User Story 2 (P1)**: Depends on Foundational config (Phase 1). No other story dependencies.
- **User Story 1 (P1)**: Depends on US2 (blog section must exist for article to be published).
- **User Story 3 (P2)**: Depends on US1 (homepage section queries blog posts — needs at least one to render).

### Within Each User Story

- Article tasks T007-T010 (accuracy corrections) can be done in any order but all depend on T006 (article creation).
- Homepage task T012 is a single file edit — no internal parallelism.

### Parallel Opportunities

Within Phase 1 (Foundational):

```
T001 (params.toml) | T002 (hugo.toml) | T003 (menus.en.toml) — all in parallel
```

Within Phase 3 (US1 - Article):

```
After T006 completes:
T007 (FR-009 correction) | T008 (FR-010 correction) | T009 (FR-011 correction) | T010 (FR-012 correction) — all in parallel (different sections of same file, but non-overlapping edits)
```

---

## Implementation Strategy

### MVP First (User Stories 1 + 2)

1. Complete Phase 1: Foundational config (3 parallel tasks)
2. Complete Phase 2: Blog section index (US2)
3. Complete Phase 3: Article with corrections (US1)
4. **STOP and VALIDATE**: Blog is functional — article accessible via navbar, correct URL, accurate content
5. This is the MVP: a working blog with one published article

### Full Delivery

6. Complete Phase 4: Homepage integration (US3)
7. Complete Phase 5: Polish and final verification
8. All success criteria validated

---

## Notes

- No automated tests — validation is `npm run build` + visual verification via `npm run dev`
- The Doks theme provides built-in blog templates (`blog/list.html`, `blog/single.html`) — no custom layout files needed for the blog section itself
- The only template modification is to `layouts/home.html` for the "Latest Articles" homepage section
- Article accuracy corrections (T007-T010) were derived from verifying 15 claims against the Gaze repository source code
- Commit after each phase checkpoint to maintain clean git history
