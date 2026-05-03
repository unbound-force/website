## 1. Create blog post file with frontmatter

- [x] 1.1 Create `content/blog/review-pr-causality.md` with Hugo/Doks blog frontmatter (title, description, lead, slug, date, draft: false, weight, toc, categories: ["Engineering"], tags: ["review", "ci", "causality", "pull-request"], contributors)

## 2. Write the problem and solution sections

- [x] 2.1 Write "The Problem" section: CI failures on PRs — flaky tests, pre-existing violations, wasted investigation time. Frame universally per BA-007 (every developer faces this)
- [x] 2.2 Read upstream source file (`unbound-force/unbound-force/.opencode/command/review-pr.md`) to verify current features before writing
- [x] 2.3 Write "The Solution" section: `/review-pr` with CI causality analysis — one command that fetches CI results, classifies failures, and produces actionable findings

## 3. Write causality classification and fix branch sections

- [x] 3.1 Write CI causality classification section with the 2×2 table: base pass/fail × PR pass/fail → PR-caused, pre-existing, unknown
- [x] 3.2 Write "Fix It Forward" section: fix branch offering for pre-existing failures, dirty-tree guard, collision check, user confirmation
- [x] 3.3 Write in-line PR comments section: 15-comment cap, human confirmation gate before posting

## 4. Write review lifecycle and supplementary sections

- [x] 4.1 Write review lifecycle section: `/review-council` (pre-PR) + `/review-pr` (post-PR) as complementary commands, with timeline visualization
- [x] 4.2 Write call to action: install command verified at implementation time

## 5. Cross-references and verification

- [x] 5.1 Add cross-reference links to common-workflows docs and getting-started pages — only link to pages that exist at implementation time
- [x] 5.2 Run `npm run build` and confirm no build errors
- [x] 5.3 Verify the blog post appears in the blog listing page
- [x] 5.4 Verify all internal links resolve (no dead links)
- [ ] 5.5 Run `npm run dev` and verify the post renders correctly
- [ ] 5.6 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
