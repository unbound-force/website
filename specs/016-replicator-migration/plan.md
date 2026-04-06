# Implementation Plan: Replicator Migration

**Branch**: `016-replicator-migration` | **Date**: 2026-04-06 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `specs/016-replicator-migration/spec.md`

## Summary

Replace all Swarm plugin product references with Replicator across the Unbound Force website. Replicator (Go binary, 53 MCP tools, zero runtime dependencies) has replaced the Node.js Swarm plugin. This involves updating 10 content/layout files, 3 internal agent/command files, 1 config file, and creating 1 new project page. All MCP tool names and generic "swarm" noun usage remain unchanged.

## Technical Context

**Language/Version**: Hugo (Go-based SSG) via @thulite npm packages  
**Primary Dependencies**: thulite ^2.6.3, @thulite/doks-core ^1.8.3  
**Storage**: N/A (static Markdown files)  
**Testing**: `npm run build` (zero-error build is the test gate)  
**Target Platform**: GitHub Pages (static site)  
**Project Type**: Static documentation website  
**Performance Goals**: N/A  
**Constraints**: Zero build errors, all links valid  
**Scale/Scope**: 14 files modified, 1 file created, 19 functional requirements

## Constitution Check

_GATE: Must pass before implementation._

### I. Content Accuracy — PASS

All Replicator content is sourced from the upstream `replicator/README.md` (53 MCP tools, 190+ tests, 15MB binary, <50ms startup, Go 1.25+, MIT license). No fabricated features. The migration removes factually incorrect references to a deprecated product (Swarm plugin / swarmtools.ai) and replaces them with the current tool (Replicator). This directly serves Content Accuracy.

### II. Minimal Footprint — PASS

Changes are content-only (Markdown text replacements) plus one new Markdown page and one HTML card addition. No new dependencies, no new custom CSS, no new JavaScript. The homepage grid change (`col-lg-6` → `col-lg-4`) uses existing Bootstrap classes. All changes use Doks built-in features.

### III. Visitor Clarity — PASS

The migration improves clarity by replacing references to a tool that no longer exists (Swarm plugin) with the tool developers will actually install (Replicator). The new project page follows the same structure as existing Gaze and Dewey pages. The homepage 3-column grid maintains visual consistency.

## File Change Matrix

| File                                               | FRs                                                    | Change Type                                                               |
| -------------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------------------- |
| `content/docs/getting-started/quick-start.md`      | FR-001, FR-002, FR-003, FR-005, FR-008, FR-009         | Edit: Stack table, setup list, /swarm removal                             |
| `content/docs/getting-started/developer.md`        | FR-001, FR-003, FR-004, FR-006, FR-007, FR-008, FR-009 | Edit: Section rename, setup list, opencode.json docs, /swarm removal      |
| `content/docs/getting-started/common-workflows.md` | FR-001, FR-003, FR-008, FR-009                         | Edit: Swarm references, /swarm removal, setup list                        |
| `content/docs/getting-started/_index.md`           | FR-001, FR-002, FR-005                                 | Edit: Stack table                                                         |
| `content/docs/getting-started/knowledge.md`        | FR-001, FR-004                                         | Edit: "swarm init" reference                                              |
| `content/docs/getting-started/artifacts.md`        | FR-001                                                 | Edit: "Swarm Orchestration" → keep (role name, not product) — verify only |
| `content/blog/unleash-in-practice.md`              | FR-001, FR-008                                         | Edit: Swarm product references, /swarm link                               |
| `content/docs/projects/replicator.md`              | FR-010, FR-011                                         | **NEW**: Replicator project page                                          |
| `content/docs/projects/_index.md`                  | FR-013                                                 | Edit: Add Replicator to project list                                      |
| `layouts/home.html`                                | FR-012                                                 | Edit: Add Replicator card, change grid to 3-column                        |
| `.opencode/command/unleash.md`                     | FR-014                                                 | Edit: "Swarm plugin" → "Replicator" in fallback messages                  |
| `.opencode/agents/cobalt-crush-dev.md`             | FR-015                                                 | Edit: "Swarm" product refs → "Replicator" in coordination section         |
| `.opencode/skill/speckit-workflow/SKILL.md`        | FR-016                                                 | Edit: "Swarm coordinator" → "Replicator"                                  |
| `.dewey/sources.yaml`                              | FR-017                                                 | Edit: Remove web-swarm entry                                              |

## Project Structure

### Documentation (this feature)

```text
specs/016-replicator-migration/
├── spec.md              # Feature specification
├── plan.md              # This file
├── research.md          # Upstream content research
└── tasks.md             # Task breakdown (next step)
```

### Source Code (repository root)

```text
content/
├── docs/
│   ├── getting-started/
│   │   ├── _index.md          # Stack table update
│   │   ├── quick-start.md     # Stack table, setup list, /swarm removal
│   │   ├── developer.md       # Section rename, opencode.json, /swarm removal
│   │   ├── common-workflows.md # Swarm refs, /swarm removal, setup list
│   │   ├── knowledge.md       # swarm init reference
│   │   └── artifacts.md       # Verify only (role name stays)
│   └── projects/
│       ├── _index.md          # Add Replicator listing
│       └── replicator.md      # NEW project page
├── blog/
│   └── unleash-in-practice.md # Swarm product refs
layouts/
└── home.html                  # Replicator card, 3-column grid
.opencode/
├── command/unleash.md         # Swarm plugin → Replicator
├── agents/cobalt-crush-dev.md # Swarm → Replicator in coordination
└── skill/speckit-workflow/SKILL.md # Swarm coordinator → Replicator
.dewey/
└── sources.yaml               # Remove web-swarm entry
```

## Complexity Tracking

No constitution violations. All changes are content updates within the existing site structure.
