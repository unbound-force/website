## 1. Create tutorial page with frontmatter

- [x] 1.1 Create `content/docs/getting-started/config-tutorial.md` with Hugo/Doks docs frontmatter (title: "Configuration Tutorial", description, lead, date, weight after code-review-tutorial, toc: true, draft: false)

## 2. Write setup and config init section

- [x] 2.1 Write prerequisites section: `uf` CLI installed
- [x] 2.2 Read upstream source files and run `uf config init --help`, `uf config show --help`, `uf config validate --help` to verify current behavior before writing
- [x] 2.3 Write `uf config init` walkthrough: run the command, explain the generated template with commented defaults

## 3. Write customization scenarios

- [x] 3.1 Write Fedora/RHEL package manager scenario: `setup.package_manager: dnf` with verification via `uf config show`
- [x] 3.2 Write embedding model scenario: change model and dimensions with verification
- [x] 3.3 Write gateway port scenario: `gateway.port: 9090` with verification
- [x] 3.4 Write skip list scenario: `setup.skip: [gaze, mxf]` with verification

## 4. Write precedence and validation sections

- [x] 4.1 Write layered loading precedence section: show a concrete override example (user config vs repo config) with `uf config show` output proving the merge
- [x] 4.2 Write `uf config validate` section: run validation, show error output for an invalid key

## 5. Cross-references and verification

- [x] 5.1 Add cross-reference links to config reference docs and getting-started pages — only link to pages that exist at implementation time
- [x] 5.2 Run `npm run build` and confirm no build errors
- [x] 5.3 Verify the tutorial page appears in the Getting Started sidebar
- [x] 5.4 Verify all internal links resolve (no dead links)
- [ ] 5.5 Run `npm run dev` and verify the tutorial renders correctly
- [ ] 5.6 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
