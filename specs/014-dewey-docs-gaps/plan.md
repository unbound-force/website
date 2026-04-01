# Implementation Plan: Dewey Documentation Gaps

**Branch**: `014-dewey-docs-gaps` | **Date**: 2026-04-01 | **Spec**: [spec.md](spec.md)

## Summary

Address Dewey documentation gaps: document `uf init` auto-generated `sources.yaml`, add toolstack web source guidance with Go/TS templates, create a blog post about AI agents and documentation access, and add a cross-reference from the developer guide.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via `@thulite` npm packages
**Testing**: `npm run build` must succeed; visual verification
**Constraints**: Markdown content + no new dependencies

## Constitution Check

### I. Content Accuracy -- PASS

All `uf init` source detection behavior sourced from PR #63 and the archived design doc (verified via Dewey). Web source examples use real, stable documentation URLs. Blog post describes current Dewey behavior without version-coupling.

### II. Minimal Footprint -- PASS

1 new blog post (Markdown), 2 pages modified (knowledge.md, developer.md). No new CSS, JS, layouts, or dependencies.

### III. Visitor Clarity -- PASS

Fills the gap between "here's the YAML syntax" and "here's what you should actually do with it." Guides users from auto-generated defaults to a recommended configuration strategy.

## File Change Matrix

| File                                        | Priority | FRs              | Change                                                                                                             |
| ------------------------------------------- | -------- | ---------------- | ------------------------------------------------------------------------------------------------------------------ |
| `content/docs/getting-started/knowledge.md` | P1       | FR-001 to FR-008 | Add "What uf init Creates" subsection before source types; add "Extending Your Sources" section after source types |
| `content/blog/dewey-knowledge-retrieval.md` | P2       | FR-009 to FR-013 | New blog post                                                                                                      |
| `content/docs/getting-started/developer.md` | P3       | FR-014           | Add cross-reference in uf init section                                                                             |

## Research Sources (verified via Dewey)

- PR #63: `github-unbound-force/unbound-force/pulls/63` -- auto-detect sibling repos feature, example generated config, ownership model
- Design doc: `disk-unbound-force/openspec/changes/archive/2026-03-28-dewey-auto-sources/design.md` -- detection logic, isDefaultSourcesConfig, extractGitHubOrg, placement in initSubTools
- Current knowledge.md: `content/docs/getting-started/knowledge` -- existing source type docs, web crawl examples
