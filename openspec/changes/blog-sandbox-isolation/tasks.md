## 1. Create blog post file with frontmatter

- [x] 1.1 Create `content/blog/sandbox-isolation.md` with Hugo/Doks blog frontmatter (title, description, lead, slug, date, draft: false, weight, toc, categories: ["Engineering"], tags: ["sandbox", "podman", "isolation", "security"], contributors)
- [x] 1.2 Verify the current `uf` version by running `uf version` — use the actual version number in the post per BA-004 (no "recently" or "new")

## 2. Write the problem section

- [x] 2.1 Write "The Problem" section (2-3 paragraphs): agents with full filesystem access, real-world risk scenarios (accidental deletion, force push, corrupted state), why existing mitigations (careful prompting, git stash) are insufficient
- [x] 2.2 Lead with benefit framing per BA-007 — frame from the reader's perspective ("your repo is at risk") not the tool's perspective ("we added a sandbox")

## 3. Write the solution and walkthrough

- [x] 3.1 Write "The Solution" section (1-2 paragraphs): one-command containerized sessions, two modes (isolated default, direct for real-time sync)
- [x] 3.2 Run `uf sandbox start --help` and `uf sandbox extract --help` to verify current flags and behavior before writing the walkthrough
- [x] 3.3 Write walkthrough section: `uf sandbox start` (what happens step by step), working inside the sandbox (normal OpenCode session), `uf sandbox extract` (patch summary, review prompt, git am application), verify changes on host — use fenced `text` code blocks for terminal output
- [x] 3.4 Write brief lifecycle management subsection: `uf sandbox status`, `stop`, `attach`

## 4. Write security model and supplementary sections

- [x] 4.1 Write security model table: rootless Podman, read-only mounts, no push credentials, resource limits, SELinux, non-root user
- [x] 4.2 Write Google Cloud / Vertex AI section (short, 3-4 sentences): credential forwarding, service account key auto-mount, gateway auto-start
- [x] 4.3 Write Current Limitations section per VB-004: single container at a time, requires Podman (not Docker), fixed health check timeout — no weasel language
- [x] 4.4 Write "What's Next" section: CDE / Eclipse Che integration, link to containerfile repo for custom images
- [x] 4.5 Write call to action: `brew install unbound-force/tap/unbound-force && uf sandbox start`

## 5. Cross-references and verification

- [x] 5.1 Add cross-reference links to sandbox reference docs and getting-started pages — only link to pages that exist at implementation time
- [x] 5.2 Run `npm run build` and confirm no build errors
- [x] 5.3 Verify the blog post appears in the blog listing page
- [x] 5.4 Verify all internal links resolve (no dead links)
- [ ] 5.5 Run `npm run dev` and verify the post renders correctly
- [ ] 5.6 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
