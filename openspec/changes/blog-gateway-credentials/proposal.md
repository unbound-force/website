## Why

AI agents running in containers need to call LLM APIs, which means cloud credentials (GCP OAuth tokens, AWS session credentials, Anthropic API keys) must somehow reach the container. The standard approach — forwarding environment variables or mounting credential files — creates security surface, platform friction, and token expiry headaches. `uf gateway` solves this by running a local reverse proxy on the host that accepts standard Anthropic API requests and translates + authenticates upstream. The container sees `localhost:53147` and a dummy token. Zero cloud credentials inside the container.

This is a compelling engineering story with concrete before/after metrics (6 env vars forwarded → 2, zero AWS SDK dependency for Bedrock, proactive token refresh). The gateway reference docs (PR #77) cover the command surface, but there is no narrative content explaining the credential isolation design and why it matters.

This change addresses [GitHub issue #63](https://github.com/unbound-force/website/issues/63).

## What Changes

A new blog post at `content/blog/gateway-credentials.md` with the narrative arc:

1. **The problem** — credential forwarding to containers: leaked env vars, stale tokens, provider-specific SDK quirks
2. **The solution** — `uf gateway` as a local reverse proxy: one proxy, three clouds (Vertex AI, Bedrock, Anthropic)
3. **How it works** — request flow diagram (container → gateway → cloud), provider auto-detection, translation mechanics
4. **Token refresh** — proactive refresh with 55-minute safety margins, thundering-herd deduplication
5. **Before/after** — concrete comparison: 6 env vars → 2, credential mounts → none, SDK dependencies → none
6. **Sandbox integration** — auto-start when cloud provider detected

## Capabilities

### New Capabilities
- `blog/gateway-credentials`: Blog post covering gateway credential isolation design

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- 1 new blog post (Markdown content file)
- No layout, style, or configuration changes
- Cross-references to gateway reference docs (PR #77, if merged) and getting-started pages

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

All technical claims (provider detection priority, token refresh timing, SigV4 signing, SSE event filtering) will be verified against the upstream implementation at `unbound-force/unbound-force/internal/gateway/` and `uf gateway --help` output at implementation time. The before/after metrics (6 env vars → 2) are derived from the actual implementation.

### II. Minimal Footprint

**Assessment**: PASS

Single Markdown blog post file. No custom layouts, CSS, JavaScript, or dependencies. Uses the existing Doks blog infrastructure.

### III. Visitor Clarity

**Assessment**: PASS

Follows the BA-001 narrative arc (problem → approach → evidence → CTA). Leads with "your container sees localhost, not Google Cloud" per BA-007 benefit framing. The before/after comparison gives visitors a concrete mental model of what the gateway eliminates.
