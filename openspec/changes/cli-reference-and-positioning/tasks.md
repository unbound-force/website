## 1. Create Reference section and CLI reference page

- [ ] 1.1 Create `content/docs/reference/_index.md` section index with frontmatter (title: "Reference", weight appropriate for sidebar placement)
- [ ] 1.2 Create `content/docs/reference/cli.md` with full CLI reference: all 6 command groups (`init`, `setup`, `doctor`, `config`, `sandbox`, `gateway`), subcommands, primary flags, and brief descriptions — sourced from upstream `unbound-force` repo
- [ ] 1.3 Update `config/_default/menus/menus.en.toml` to add Reference section to top-level navigation if needed

## 2. Create Changelog page

- [ ] 2.1 Create `content/docs/changelog/_index.md` with frontmatter and initial release entries (current release + key milestones)
- [ ] 2.2 Add changelog to navigation menu if not auto-discovered by Hugo

## 3. Update stack positioning table

- [ ] 3.1 Update `content/_index.md` stack table to add `uf` CLI as the first/top layer (4 layers total)
- [ ] 3.2 Update `content/docs/getting-started/quick-start.md` stack table to add `uf` CLI layer
- [ ] 3.3 Update `layouts/home.html` if the homepage template renders the stack table from HTML rather than Markdown

## 4. Update code review persona table scope

- [ ] 4.1 Add clarifying note to the Code Review persona table in `content/docs/getting-started/common-workflows.md` stating that the 3 content personas (Scribe, Herald, Envoy) are invoked separately and do not participate in code review

## 5. Verification

- [ ] 5.1 Run `npm run build` and confirm no build errors
- [ ] 5.2 Run `npm run dev` and verify: CLI reference page renders with all command groups, changelog page renders, stack table shows 4 layers on homepage and quick-start, Reference section appears in navigation
- [ ] 5.3 Verify both light and dark mode rendering for new pages
