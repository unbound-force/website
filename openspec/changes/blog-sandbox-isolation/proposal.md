## Why

The `uf sandbox` command shipped in v0.12.0 with 7 subcommands, two mount modes, UID mapping, and persistent workspaces. The reference documentation (PR #79) covers the command surface, but there is no narrative content explaining *why* containerized agent sessions matter. Engineers evaluating Unbound Force need a concrete walkthrough showing the round-trip workflow: start a sandbox, work inside it, extract changes back to the host. The security benefits (read-only mounts, no credential forwarding, resource limits) are significant but not self-evident from a reference page.

This change addresses [GitHub issue #40](https://github.com/unbound-force/website/issues/40).

## What Changes

A new blog post at `content/blog/sandbox-isolation.md` with the narrative arc:

1. **The problem** — AI agents running with full filesystem access create real risk: accidental deletion, force push, corrupted state
2. **The solution** — `uf sandbox start` wraps OpenCode in a Podman container with controlled access
3. **Walkthrough** — Step-by-step round-trip: start, work, extract, verify
4. **Security model** — Table of isolation properties
5. **Current limitations** — Honest about single container, Podman-only, fixed health check timeout

The blog post follows the content pack narrative arc (BA-001: problem → approach → evidence → CTA) and leads with benefit framing (BA-007: "your repo is untouchable" not "we added a sandbox command").

## Capabilities

### New Capabilities
- `blog/sandbox-isolation`: Blog post covering sandbox motivation, walkthrough, and security model

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- 1 new blog post (Markdown content file)
- No layout, style, or configuration changes
- No documentation page modifications
- Cross-references to sandbox reference docs (PR #79, if merged) and getting-started pages

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

All commands and behaviors in the walkthrough will be verified against `uf sandbox --help` output and the shipped binary at implementation time. The Current Limitations section honestly states constraints (single container, Podman-only). The security model table matches the upstream implementation. No features are fabricated or overstated.

### II. Minimal Footprint

**Assessment**: PASS

Single Markdown blog post file. No custom layouts, CSS, JavaScript, or dependencies. Uses the existing Doks blog infrastructure (categories, tags, contributors frontmatter).

### III. Visitor Clarity

**Assessment**: PASS

Follows the BA-001 narrative arc (problem → approach → evidence → CTA) which is optimized for visitor comprehension. Leads with the "why should I care" (your repo is at risk) before the "how" (sandbox commands). The walkthrough gives visitors a concrete mental model of the round-trip workflow.
