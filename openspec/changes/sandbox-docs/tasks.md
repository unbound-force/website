## 1. Create Reference section (if needed) and sandbox documentation page

- [x] 1.1 Create `content/docs/reference/_index.md` section index and add Reference `[[docs]]` entry to `config/_default/menus/menus.en.toml` if they do not already exist. This change fully owns Reference section creation if it doesn't exist yet.
- [x] 1.2 Read upstream source files (`unbound-force/unbound-force/internal/sandbox/`) and run `uf sandbox --help` and `uf sandbox <subcommand> --help` to verify current command surface before writing content
- [x] 1.3 Create `content/docs/reference/sandbox.md` with required frontmatter (title: "Sandbox", description, weight for sidebar ordering)
- [x] 1.4 Write introduction section: what `uf sandbox` does, why containerized sessions matter, prerequisites (Podman, API key)
- [x] 1.5 Write basic session management section: `start`, `stop`, `attach`, `extract`, `status` — with flags and copy-pasteable usage examples
- [x] 1.6 Write mount modes section: isolated (read-only, default) vs direct (read-write), with clear guidance on when to use each

## 2. Add UID mapping section

- [x] 2.1 Write UID mapping prerequisites: Podman >= 4.3, automatic `--userns=keep-id:uid=1000,gid=1000`
- [x] 2.2 Write macOS-specific setup: Podman machine configuration for virtiofs UID mapping
- [x] 2.3 Document `--uidmap` flag as escape hatch for macOS
- [x] 2.4 Add troubleshooting note for "files appear as root:nobody" issue

## 3. Add persistent workspaces section

- [x] 3.1 Write persistent workspace lifecycle: `create` (flags: `--backend`, `--demo-ports`, `--name`) and `destroy` (flags: `--yes`, `--force`, confirmation prompt)
- [x] 3.2 Document workspace detection behavior: `start`/`stop`/`extract` auto-detect and use existing persistent workspaces
- [x] 3.3 Document bidirectional git sync and demo endpoint port forwarding

## 4. Add CDE backend section (experimental)

- [x] 4.1 Write CDE backend section with experimental callout
- [x] 4.2 Document `--backend` flag: `podman` (default) vs `che`, and backend resolution matrix (flag > env var > config > auto-detect)
- [x] 4.3 Note validation status: API and mocked tests exist, end-to-end with production Che not validated

## 5. Add security model and API key forwarding

- [x] 5.1 Write security model table: rootless Podman, read-only mounts, no push credentials, resource limits, SELinux `:Z` auto-detection, non-root user
- [x] 5.2 Document API key forwarding and Google Cloud / Vertex AI integration
- [x] 5.3 Add cross-reference links to gateway docs and other existing pages — only link to pages that exist at implementation time

## 6. Verification

- [x] 6.1 Run `npm run build` and confirm no build errors
- [x] 6.2 Verify all internal links resolve (no dead links)
- [ ] 6.3 Run `npm run dev` and verify sandbox page renders correctly with all sections
- [x] 6.4 Verify page appears in Reference section navigation
- [ ] 6.5 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
