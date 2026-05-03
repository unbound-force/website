## 1. Create tutorial page with frontmatter

- [x] 1.1 Create `content/docs/getting-started/code-review-tutorial.md` with Hugo/Doks docs frontmatter (title: "Code Review Tutorial", description, lead, date, weight after common-workflows, toc: true, draft: false)

## 2. Write prerequisites and /review-council section

- [x] 2.1 Write prerequisites section: `uf init` completed, `gh` CLI installed and authenticated
- [x] 2.2 Read upstream source files (`unbound-force/unbound-force/.opencode/command/review-council.md` and `review-pr.md`) to verify current behavior before writing
- [x] 2.3 Write `/review-council` walkthrough: run the command, explain what happens (CI gate, Gaze optional, 5+ Divisor personas), show representative output with verdict

## 3. Write /review-pr and causality sections

- [x] 3.1 Write `/review-pr` walkthrough: push and create PR first, then run `/review-pr`, show representative output
- [x] 3.2 Write CI causality analysis walkthrough: show the classification table (PR-caused, pre-existing, unknown) with a concrete example
- [x] 3.3 Write fix branch section: show the fix branch offering for pre-existing failures, dirty-tree guard, collision check

## 4. Write complete loop and decision table

- [x] 4.1 Write "The Complete Loop" section: specify → unleash → /review-council → /finale → /review-pr
- [x] 4.2 Write decision table: Before pushing → /review-council, After creating PR → /review-pr, Reviewing someone else's PR → /review-pr N, CI failed unsure if my fault → /review-pr (causality)

## 5. Cross-references and verification

- [x] 5.1 Add cross-reference links to common-workflows docs, blog posts, and getting-started pages — only link to pages that exist at implementation time
- [x] 5.2 Run `npm run build` and confirm no build errors
- [x] 5.3 Verify the tutorial page appears in the Getting Started sidebar
- [x] 5.4 Verify all internal links resolve (no dead links)
- [ ] 5.5 Run `npm run dev` and verify the tutorial renders correctly
- [ ] 5.6 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
