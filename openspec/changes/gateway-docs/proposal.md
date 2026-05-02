## Why

`uf gateway` is a local LLM reverse proxy that isolates cloud credentials from AI agent containers. It accepts standard Anthropic API requests and translates + authenticates upstream to Vertex AI, Bedrock, or Anthropic. This is a security-critical feature — containers see `localhost:8080` and a dummy token instead of real cloud credentials. None of this is documented on the website.

This change addresses [GitHub issue #57](https://github.com/unbound-force/website/issues/57).

## What Changes

A new documentation page for the `uf gateway` command covering:

1. **Command surface**: `start` (flags: `--port`, `--provider`, `--detach`), `stop`, `status` — with descriptions and usage examples
2. **Provider support**: Vertex AI, Bedrock, Anthropic — auto-detection via environment variables, priority order
3. **Credential isolation model**: How the gateway eliminates credential forwarding to containers
4. **Vertex AI translation**: Request/response transformation, SSE event filtering, synthetic model catalog
5. **Token refresh**: Background proactive refresh, thundering-herd deduplication
6. **Sandbox integration**: Auto-start when cloud provider detected, `ANTHROPIC_BASE_URL`/`ANTHROPIC_AUTH_TOKEN` injection

## Capabilities

### New Capabilities
- `docs/reference/gateway`: Comprehensive gateway documentation page

### Modified Capabilities
- None (CLI reference page linkage is handled by the cli-reference-and-positioning change)

### Removed Capabilities
- None

## Impact

- 1 new content page
- Cross-references to sandbox docs and CLI reference page

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

Documents `uf gateway` commands sourced from the upstream implementation at `unbound-force/unbound-force/internal/gateway/`. Provider auto-detection, Vertex AI translation behavior, token refresh, and sandbox integration are all verified against the shipped code. Content must be cross-referenced with `uf gateway --help` output at implementation time.

### II. Minimal Footprint

**Assessment**: PASS

Single Markdown page under the existing Reference section. No custom layouts, CSS, or JavaScript. The Reference section was established by the `cli-reference-and-positioning` change (PR #73) — this adds a page to it.

### III. Visitor Clarity

**Assessment**: PASS

Leads with the "why" (credential isolation — a security concern visitors care about) before technical details. Provider sections are structured so users can skip to their provider. Cross-references to sandbox docs prevent duplication.
