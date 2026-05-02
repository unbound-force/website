## Context

The `uf gateway` command implements an LLM reverse proxy with 3 subcommands and support for 3 cloud providers. It was built across two specs (033-gateway-command, 034-gateway-vertex-translation) and two follow-up changes (token refresh fix, global region error). The gateway is tightly integrated with the sandbox — it auto-starts when a cloud provider is detected during `uf sandbox start`.

## Goals / Non-Goals

### Goals
- Document all 3 subcommands with flags and usage
- Explain the credential isolation model clearly (this is the primary value proposition)
- Document provider auto-detection and priority order
- Cover Vertex AI translation specifics (the most complex provider)
- Document token refresh behavior and sandbox integration

### Non-Goals
- Deep Bedrock SigV4 signing implementation details (internal concern)
- Troubleshooting every provider-specific error (add as issues arise)
- Performance benchmarks or latency analysis

## Decisions

1. **Single page under reference section**: Place at `content/docs/reference/gateway.md`. The gateway is a reference topic — users need to look up flags, provider setup, and troubleshooting.

2. **Lead with credential isolation**: The "why" is security — start with the problem (credential forwarding to containers) and the solution (localhost proxy with dummy token). Technical details come after.

3. **Provider sections**: Give Vertex AI the most detail (it has the most transformation logic — SSE filtering, model catalog, header stripping). Bedrock and Anthropic get shorter sections.

4. **Sandbox integration as a cross-reference**: Don't duplicate sandbox docs. Include a brief "Gateway + Sandbox" section that explains auto-start and links to the sandbox page for full details.

## Risks / Trade-offs

- **Risk**: Gateway provider support may expand (e.g., OpenAI, Azure). Page structure should accommodate new providers without restructuring.
- **Trade-off**: Vertex AI detail may overwhelm users who just need Anthropic direct. Mitigated by placing Vertex-specific details in a subsection users can skip.
