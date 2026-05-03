## Context

The website has a gateway reference page (PR #77) covering the command surface. Issue #63 provides a narrative angle, key messages, and source references for a blog post that explains the "why" behind credential isolation. The sandbox isolation blog post (PR #80) covers sandbox containerization; this post covers the complementary gateway credential proxy.

## Goals / Non-Goals

### Goals
- Write a blog post following the BA-001 narrative arc (problem → approach → evidence → CTA)
- Lead with "your container sees localhost, not Google Cloud" per BA-007
- Include a concrete before/after comparison (6 env vars → 2)
- Explain the three-provider support (Vertex AI, Bedrock, Anthropic) without drowning in implementation details
- Cover token refresh as a key reliability feature

### Non-Goals
- Duplicate the reference docs (command flags, full Vertex translation details) — link to them instead
- Deep SigV4 signing implementation details (internal concern)
- Performance benchmarks or latency analysis
- Troubleshooting provider-specific errors

## Decisions

1. **Slug and filename**: `gateway-credentials` — short, matches the issue's credential isolation framing. File at `content/blog/gateway-credentials.md`.

2. **Narrative structure**: Problem (credential forwarding headaches) → Solution (local reverse proxy) → How It Works (request flow, auto-detection) → Token Refresh → Before/After → Sandbox Integration → CTA.

3. **Before/after as a table**: Use a comparison table showing the concrete reduction in complexity. This is the "evidence" section of the narrative arc — scannable and precise.

4. **Light on Vertex internals**: Mention SSE filtering and model catalog briefly but don't explain the full translation pipeline. The reference docs cover that. The blog audience cares about "it works with Vertex" not "here's how SSE event filtering drops vertex_event frames."

5. **Frontmatter pattern**: Follow existing blog post frontmatter (title, description, lead, slug, date, draft, weight, toc, categories, tags, contributors).

6. **Cross-references**: Link to gateway reference page (`/docs/reference/gateway/`) only if it exists at implementation time. Link to getting-started pages which always exist.

## Content Sources

Authoritative upstream sources:
- Gateway implementation: `unbound-force/unbound-force/internal/gateway/` (provider.go, refresh.go, gateway.go, sse.go)
- CLI help: `uf gateway --help`
- Specs: `unbound-force/unbound-force/specs/033-gateway-command/`, `unbound-force/unbound-force/specs/034-gateway-vertex-translation/`

All technical claims must be verified against these upstream files at implementation time.

## Risks / Trade-offs

- **Risk**: The before/after metrics (6 env vars → 2) are derived from the current implementation. If the gateway's env var handling changes, the metrics go stale. Mitigated by using "Before gateway" / "With gateway" framing rather than version-specific claims.
- **Trade-off**: Light Vertex details may frustrate power users who want to understand the translation. Mitigated by linking to the reference docs for depth.
