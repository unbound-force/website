## 1. Create Reference section and CLI reference page

- [x] 1.1 Create `content/docs/reference/_index.md` section index with frontmatter (title: "Reference", weight appropriate for sidebar placement)
- [x] 1.2 Create `content/docs/reference/cli.md` with full CLI reference: all current command groups, subcommands, primary flags, and brief descriptions — sourced from `uf --help` and `uf <command> --help` output. Include a version marker indicating which `uf` version the reference was generated from.
- [x] 1.3 Update `config/_default/menus/menus.en.toml` to add Reference section to top-level navigation

## 2. Create Changelog page

- [x] 2.1 Create `content/docs/changelog/_index.md` with frontmatter and the current release entry only — sourced from actual release artifacts (git tags, release notes, merged PRs)
- [x] 2.2 Add a `[[docs]]` entry for Changelog in `config/_default/menus/menus.en.toml` with appropriate weight

## 3. Update stack positioning table

- [x] 3.1 Update `content/docs/getting-started/_index.md` stack table and "Three Tools, One Workflow" heading/prose to reflect the 4-layer model with `uf` CLI as the first/top layer
- [x] 3.2 Update `content/docs/getting-started/quick-start.md` stack table and surrounding prose to add `uf` CLI layer (4 layers total)

## 4. Update code review persona table scope

- [x] 4.1 Add clarifying note to the Code Review persona table in `content/docs/getting-started/common-workflows.md` stating that the 3 content personas (Scribe, Herald, Envoy) are invoked separately and do not participate in code review

## 5. Verification

- [x] 5.1 Run `npm run build` and confirm no build errors
- [x] 5.2 Run `uf --help` and `uf <command> --help` for all command groups; verify the CLI reference page matches the actual command names, subcommands, and primary flags
- [ ] 5.3 Run `npm run dev` and verify: CLI reference page renders with all command groups and version marker, changelog page renders with current release, stack table shows 4 layers on getting-started index and quick-start, Reference and Changelog sections appear in navigation
- [x] 5.4 Verify all internal links in new pages resolve to existing pages (no dead links)
- [ ] 5.5 Verify both light and dark mode rendering for new pages
<!-- spec-review: passed -->
<!-- code-review: passed -->
