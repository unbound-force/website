## 1. Create Reference section (if needed) and gateway documentation page

- [x] 1.1 Create `content/docs/reference/_index.md` section index and add Reference `[[docs]]` entry to `config/_default/menus/menus.en.toml`. If these already exist (from a previously merged PR), skip. This change fully owns Reference section creation if it doesn't exist yet.
- [x] 1.2 Read upstream source files (`unbound-force/unbound-force/internal/gateway/`) and run `uf gateway --help` (if available) or read upstream CLI help text to verify current command surface, flags, and provider behavior before writing content
- [x] 1.3 Create `content/docs/reference/gateway.md` with required frontmatter (title: "Gateway", description, weight for sidebar ordering)
- [x] 1.4 Write introduction section: what `uf gateway` does, the credential isolation problem it solves

## 2. Document command reference

- [x] 2.1 Write command reference section: `start` (flags: `--port`, `--provider`, `--detach`), `stop`, `status` — with copy-pasteable usage examples

## 3. Document provider support

- [x] 3.1 Write provider auto-detection section: environment variable scanning, detection priority order
- [x] 3.2 Write Vertex AI section: request/response translation, SSE event filtering, synthetic model catalog
- [x] 3.3 Write Bedrock section: SigV4 signing, credential sources
- [x] 3.4 Write Anthropic direct section: pass-through behavior, API key forwarding

## 4. Document token refresh and daemon behavior

- [x] 4.1 Write token refresh section: background proactive refresh, deduplication
- [x] 4.2 Write daemon behavior section: background mode, PID file, log file

## 5. Document sandbox integration

- [x] 5.1 Write sandbox integration section: auto-start when cloud provider detected, env var injection to container
- [x] 5.2 Add cross-reference links to sandbox docs, CLI reference, and configuration docs — only link to pages that exist at implementation time. Omit links for pages not yet created.

## 6. Verification

- [x] 6.1 Run `npm run build` and confirm no build errors
- [ ] 6.2 Run `npm run dev` and verify gateway page renders correctly with all sections
- [x] 6.3 Verify page appears in Reference section navigation
- [x] 6.4 Verify all internal links resolve (no dead links)
- [ ] 6.5 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
