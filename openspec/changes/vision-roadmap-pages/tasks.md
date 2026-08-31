<!--
  [P] marks tasks eligible for parallel execution.
  Add [P] when a task: (a) touches different files from
  other [P] tasks in the group, (b) has no dependency
  on prior tasks in the group, (c) can safely execute
  without ordering constraints.
  Do NOT add [P] when tasks modify the same file —
  parallel workers will cause merge conflicts.
  Tasks without [P] run sequentially first, then [P]
  tasks run in parallel.
-->

## 1. Create Content Pages

- [x] 1.1 [P] Create `content/docs/vision/_index.md` — adapt VISION.md from `/Users/jflowers/Projects/github/unbound-force/VISION.md` with Hugo YAML frontmatter (title, description, lead, date, weight: 5, toc: true, draft: false). Shift all headings down one level (H1→H2, H2→H3, etc.). Preserve all substantive content.
- [x] 1.2 [P] Create `content/docs/roadmap/_index.md` — adapt ROADMAP.md from `/Users/jflowers/Projects/github/unbound-force/ROADMAP.md` with Hugo YAML frontmatter (title, description, lead, date, weight: 6, toc: true, draft: false). Shift all headings down one level. Replace `[VISION.md](VISION.md)` with `[Vision](/docs/vision/)`. Preserve all GitHub issue/discussion URLs as-is.

## 2. Update Navigation

- [x] 2.1 Add `[[docs]]` entries for Vision (weight 5, identifier "vision", url "/docs/vision/") and Roadmap (weight 6, identifier "roadmap", url "/docs/roadmap/") to `config/_default/menus/menus.en.toml`. Place them before the existing Getting Started entry.
- [x] 2.2 Add `[[main]]` entries for Vision (weight 2, url "/docs/vision/") and Roadmap (weight 3, url "/docs/roadmap/") to `config/_default/menus/menus.en.toml`. Place them before the existing Blog entry.

## 3. Validation

- [x] 3.1 Run `npm run build` and verify exit code 0 with no errors or warnings related to the new pages.
- [x] 3.2 Verify constitution alignment: Composability (each page is independently removable) and Testability (build validates without external services) — both assessed as PASS in proposal.

<!-- spec-review: passed -->
<!-- code-review: passed -->
