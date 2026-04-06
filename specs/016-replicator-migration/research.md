# Research: Replicator Migration

**Branch**: `016-replicator-migration` | **Date**: 2026-04-06

## Replicator Upstream Content

Source: `/Users/jflowers/Projects/github/unbound-force/replicator/README.md`

### Key Facts

- **Description**: Multi-agent coordination for AI coding agents
- **Language**: Go (1.25+)
- **License**: MIT
- **Binary**: Single Go binary, zero runtime dependencies
- **Stats**: 53 MCP tools, 190+ tests, 15MB binary, <50ms startup
- **Install**: `brew install unbound-force/tap/replicator` or `go install github.com/unbound-force/replicator/cmd/replicator@latest`
- **Heritage**: Go rewrite of cyborg-swarm, originally forked from swarm-tools by Joel Hooks

### MCP Tool Categories (53 total)

| Category   | Tools | Purpose                                                                 |
| ---------- | ----- | ----------------------------------------------------------------------- |
| Hive       | 11    | Work item tracking: create, query, update, close, epics, sessions, sync |
| Swarm Mail | 10    | Agent messaging: send, inbox, ack, file reservations                    |
| Swarm      | 24    | Orchestration: decompose, spawn, worktrees, progress, review, insights  |
| Memory     | 8     | Dewey proxy: store/find learnings, deprecated tool stubs                |

### CLI Commands (9)

- `replicator serve` — Start MCP server (AI agents connect via stdio)
- `replicator init` — Per-repo setup (creates .hive/ directory)
- `replicator setup` — Per-machine setup (creates ~/.config/swarm-tools/ + SQLite DB)
- `replicator doctor` — Check environment health
- `replicator cells` — List work items
- `replicator stats` — Activity summary
- `replicator query` — Run preset analytics queries
- `replicator docs` — Generate tool reference docs
- `replicator version` — Version info

### Architecture

- MCP Server (stdio JSON-RPC) → Tool Registry (53 tools) → Domain Logic (hive, swarm, mail) → SQLite (WAL mode)
- Dewey proxy via HTTP
- Git worktrees via os/exec

## Current State of Files to Edit

### Files with "Swarm plugin" product references (FR-001, FR-003)

1. **quick-start.md** — Line 24: "Swarm plugin (multi-agent coordination)" in uf setup list; Line 68: Stack table row with `[Swarm](https://www.swarmtools.ai/)` and "An OpenCode plugin that enables multi-agent parallelism"; Line 70: "coordinate with Swarm"
2. **developer.md** — Line 4: "Swarm" in lead; Line 23: "Swarm plugin" in setup list; Line 33: "Swarm plugin health"; Line 92-93: "Working with Swarm" section heading; Line 268: "Swarm plugin entry" in opencode.json docs
3. **common-workflows.md** — Line 31: "Swarm workers"; Line 85: "Swarm workers" link; Line 124: "parallel task execution without a spec" with `/swarm`; Line 394: "Install the Swarm plugin for parallel execution"; Line 416-417: "Swarm plugin" and "Swarm configuration" in setup list; Line 435: "Swarm plugin entries"
4. **\_index.md** — Line 25: Stack table with `[Swarm](https://www.swarmtools.ai/)`; Line 27: "coordinate with Swarm"
5. **knowledge.md** — Line 36: "swarm init" reference
6. **artifacts.md** — Line 45: "Swarm Orchestration" producer (this is a role name, not product — KEEP)
7. **unleash-in-practice.md** — Line 85: "Swarm workers" link; Line 89: "Swarm worktrees"; Line 148-149: "Swarm" in tool table

### Files with `/swarm` command references (FR-008, FR-009)

1. **quick-start.md** — Lines 56-58: `/swarm "your task description"` block
2. **developer.md** — Line 59: `/swarm "task description"` reference; Lines 96-100: "Using /swarm" section
3. **common-workflows.md** — Line 124: `/swarm "implement spec NNN"`; Lines 449-451: `/swarm "your first task description"`

### Files with swarmtools.ai URLs (FR-002)

1. **quick-start.md** — Line 68: `[Swarm](https://www.swarmtools.ai/)`
2. **\_index.md** — Line 25: `[Swarm](https://www.swarmtools.ai/)`

### Files with opencode-swarm-plugin (FR-003, FR-006)

1. **developer.md** — Line 268: `opencode-swarm-plugin`

### Internal files (FR-014, FR-015, FR-016)

1. **unleash.md** — Lines 43-44, 392-395, 516-517: "Swarm plugin" references
2. **cobalt-crush-dev.md** — Lines 148-177: "Swarm Coordination" section header, "Swarm worker" references
3. **SKILL.md** — Line 2: "Swarm coordinator" in description; Line 15: "Swarm coordinator"

### Dewey sources (FR-017)

1. **.dewey/sources.yaml** — Lines 97-104: `web-swarm` entry with swarmtools.ai URL
