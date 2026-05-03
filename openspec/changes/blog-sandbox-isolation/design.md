## Context

The website has 7 blog posts covering Gaze, Dewey, /unleash, and the Curator. No blog content covers the sandbox — the most tangible security feature in the toolchain. Issue #40 provides a detailed narrative arc, section outline, and content pack compliance notes. The sandbox reference docs (PR #79) cover the command surface; this blog post covers the "why" and the round-trip workflow.

## Goals / Non-Goals

### Goals
- Write a blog post following the BA-001 narrative arc (problem → approach → evidence → CTA)
- Include a step-by-step walkthrough of the round-trip: start → work → extract → verify
- Lead with benefit framing per BA-007 ("your repo is untouchable" not "we added a sandbox")
- Include the security model as a scannable table
- Be honest about current limitations per VB-004

### Non-Goals
- Duplicate the reference docs (commands, flags, UID mapping details) — link to them instead
- Cover CDE/Che backend (experimental, not ready for a public blog post)
- Cover persistent workspaces in depth (the walkthrough uses ephemeral sessions)
- Tutorial-style customization guide (tracked in issue #67)

## Decisions

1. **Slug and filename**: `sandbox-isolation` — short, descriptive, matches the issue title's framing. File at `content/blog/sandbox-isolation.md`.

2. **Narrative structure**: Follow the issue's outline exactly — The Problem, The Solution, Walkthrough (the bulk), Security Model, Google Cloud / Vertex AI Users, Current Limitations, What's Next. This matches BA-001 and the issue author's intent.

3. **Terminal output style**: Use fenced `text` code blocks for terminal output (same as `unleash-in-practice.md`). Do not include screenshots — the blog is Markdown-only per Minimal Footprint.

4. **Version reference**: Use "v0.12.0" per BA-004 (no "recently" or "new"). Verify the shipped version at implementation time.

5. **Cross-references**: Link to the sandbox reference page (`/docs/reference/sandbox/`) only if it exists at implementation time. Link to getting-started pages which always exist.

6. **Frontmatter pattern**: Follow `unleash-in-practice.md` frontmatter: title, description, lead, slug, date, draft, weight, toc, categories, tags, contributors.

## Content Sources

Authoritative upstream sources for walkthrough content:
- CLI help: `uf sandbox --help`, `uf sandbox start --help`, `uf sandbox extract --help`
- Sandbox implementation: `unbound-force/unbound-force/internal/sandbox/`
- Manual test script: `unbound-force/unbound-force/temp/uf-sandbox-manual-test.md` (22 test scenarios)
- Spec: `unbound-force/unbound-force/specs/032-sandbox-command/`

All commands in the walkthrough must be verified against the shipped binary.

## Risks / Trade-offs

- **Risk**: Terminal output in the walkthrough may not exactly match what users see (different platform, different project). Mitigated by keeping output examples generic and noting that output varies by platform.
- **Trade-off**: Ephemeral sessions only in the walkthrough (no persistent workspaces). Chose simplicity — the walkthrough should demonstrate the core value proposition (isolation + extraction) without the complexity of workspace lifecycle. Persistent workspaces can be a follow-up post.
- **Risk**: Google Cloud / Vertex AI section may be too niche for a general blog post. Kept short (3-4 sentences) since it's a significant use case for enterprise users.
