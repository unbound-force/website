## 1. Create gateway documentation page

- [ ] 1.1 Create `content/docs/reference/gateway.md` with required frontmatter (title: "Gateway", description, weight for sidebar ordering after sandbox)
- [ ] 1.2 Write introduction section: what `uf gateway` does, the credential isolation problem it solves, "your container sees localhost, not Google Cloud"
- [ ] 1.3 Write command reference section: `start` (flags: `--port`, `--provider`, `--detach`), `stop`, `status` — with usage examples

## 2. Document provider support

- [ ] 2.1 Write provider auto-detection section: environment variable scanning, priority order (Vertex AI > Bedrock > Anthropic)
- [ ] 2.2 Write Vertex AI section: request/response translation details (model field removal, `anthropic_version` injection, `streamRawPredict`/`rawPredict`, SSE event filtering, header stripping)
- [ ] 2.3 Write Bedrock section: SigV4 signing, no AWS SDK dependency, credential sources
- [ ] 2.4 Write Anthropic direct section: pass-through behavior, API key forwarding
- [ ] 2.5 Document synthetic model catalog: 9 Vertex-available Claude models with capabilities metadata

## 3. Document token refresh and daemon behavior

- [ ] 3.1 Write token refresh section: background goroutines, proactive refresh within 5 minutes of expiry, `sync.Mutex.TryLock()` deduplication
- [ ] 3.2 Write daemon behavior section: background mode, PID file (`.uf/gateway.pid`), log file (`.uf/gateway.log`), `_UF_GATEWAY_CHILD` sentinel

## 4. Document sandbox integration

- [ ] 4.1 Write sandbox integration section: `autoStartGateway()` auto-start when cloud provider detected, `ANTHROPIC_BASE_URL`/`ANTHROPIC_AUTH_TOKEN` injection to container
- [ ] 4.2 Add cross-reference link to sandbox documentation page

## 5. Verification

- [ ] 5.1 Run `npm run build` and confirm no build errors
- [ ] 5.2 Run `npm run dev` and verify gateway page renders correctly with all sections
- [ ] 5.3 Verify page appears in Reference section navigation
- [ ] 5.4 Verify both light and dark mode rendering
