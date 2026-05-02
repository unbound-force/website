## 1. Create sandbox documentation page

- [ ] 1.1 Create `content/docs/reference/sandbox.md` with required frontmatter (title: "Sandbox", description, weight for sidebar ordering after CLI reference)
- [ ] 1.2 Write introduction section: what `uf sandbox` does, why containerized sessions matter, prerequisites (Podman, Ollama, API key)
- [ ] 1.3 Write basic session management section: `start` (flags: `--mode`, `--detach`, `--image`, `--memory`, `--cpus`), `stop`, `attach`, `extract` (flag: `--yes`), `status` — sourced from upstream `internal/sandbox/` and `cmd/unbound-force/sandbox.go`
- [ ] 1.4 Write mount modes section: isolated (read-only, default) vs direct (read-write), with clear explanation of when to use each

## 2. Add UID mapping section

- [ ] 2.1 Write UID mapping prerequisites: Podman >= 4.3, `--userns=keep-id:uid=1000,gid=1000` (automatic)
- [ ] 2.2 Write macOS-specific setup: Podman machine configuration for virtiofs UID mapping
- [ ] 2.3 Document `--uidmap` flag as escape hatch for macOS
- [ ] 2.4 Add troubleshooting note for "files appear as root:nobody" issue

## 3. Add persistent workspaces section

- [ ] 3.1 Write persistent workspace lifecycle: `create` (flags: `--backend`, `--demo-ports`, `--name`) and `destroy` (flags: `--yes`, `--force`, confirmation prompt)
- [ ] 3.2 Document workspace detection behavior: `start`/`stop`/`extract` auto-detect and use existing persistent workspaces
- [ ] 3.3 Document bidirectional git sync (`setupGitSync`/`checkGitSync`)
- [ ] 3.4 Document `FormatWorkspaceStatus` output and demo endpoint port forwarding

## 4. Add CDE backend section (experimental)

- [ ] 4.1 Write CDE backend section with experimental callout/alert
- [ ] 4.2 Document `--backend` flag: `podman` (default) vs `che`
- [ ] 4.3 Document backend resolution matrix: flag > env var > config > auto-detect
- [ ] 4.4 Note validation status: API and mocked tests exist, end-to-end with production Che not validated

## 5. Add security model and API key forwarding

- [ ] 5.1 Write security model table: rootless Podman, read-only mounts, no push credentials, resource limits, SELinux `:Z` auto-detection, non-root user
- [ ] 5.2 Document API key forwarding: `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, `GEMINI_API_KEY`, `OPENROUTER_API_KEY`
- [ ] 5.3 Document Google Cloud / Vertex AI integration: `GOOGLE_CLOUD_PROJECT`, `VERTEX_LOCATION`, service account key auto-mount, gcloud ADC fallback

## 6. Verification

- [ ] 6.1 Run `npm run build` and confirm no build errors
- [ ] 6.2 Run `npm run dev` and verify sandbox page renders correctly with all sections
- [ ] 6.3 Verify page appears in Reference section navigation
- [ ] 6.4 Verify both light and dark mode rendering
