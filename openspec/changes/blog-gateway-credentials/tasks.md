## 1. Create blog post file with frontmatter

- [x] 1.1 Create `content/blog/gateway-credentials.md` with Hugo/Doks blog frontmatter (title, description, lead, slug, date, draft: false, weight, toc, categories: ["Engineering"], tags: ["gateway", "security", "credentials", "vertex-ai", "bedrock"], contributors)

## 2. Write the problem and solution sections

- [x] 2.1 Write "The Problem" section: credential forwarding to containers (leaked env vars, stale tokens, provider-specific SDK quirks, platform friction)
- [x] 2.2 Write "The Solution" section: `uf gateway` as a local reverse proxy — one proxy, three clouds. Lead with benefit framing per BA-007 ("your container sees localhost, not Google Cloud")
- [x] 2.3 Write "How It Works" section: request flow (container → gateway → cloud provider), provider auto-detection priority order, brief mention of Vertex translation and SSE filtering without deep implementation details

## 3. Write token refresh and before/after sections

- [x] 3.1 Read upstream source files (`unbound-force/unbound-force/internal/gateway/refresh.go`, `provider.go`) to verify token refresh timing and deduplication mechanism before writing
- [x] 3.2 Write token refresh section: proactive refresh with 55-minute safety margin (5 minutes before typical 60-minute expiry), thundering-herd deduplication via `sync.Mutex.TryLock()`
- [x] 3.3 Write before/after comparison table: environment variables forwarded, credential file mounts, SDK dependencies, token management — showing concrete reduction in complexity

## 4. Write supplementary sections

- [x] 4.1 Write sandbox integration section: gateway auto-starts when `uf sandbox start` detects a cloud provider
- [x] 4.2 Write scope/limitations note per VB-004: gateway proxies Anthropic Messages API only, not arbitrary HTTP or non-Anthropic APIs
- [x] 4.3 Write call to action: install command verified at implementation time

## 5. Cross-references and verification

- [x] 5.1 Add cross-reference links to gateway reference docs and getting-started pages — only link to pages that exist at implementation time
- [x] 5.2 Run `npm run build` and confirm no build errors
- [x] 5.3 Verify the blog post appears in the blog listing page
- [x] 5.4 Verify all internal links resolve (no dead links)
- [ ] 5.5 Run `npm run dev` and verify the post renders correctly
- [ ] 5.6 Verify both light and dark mode rendering
<!-- spec-review: passed -->
<!-- code-review: passed -->
