---
title: "Getting Started with Pinkman OSS Scout"
description: "Step-by-step tutorial for using Pinkman's /scout command to discover, evaluate, audit, and report on open source projects for adoption."
lead: "Evaluate open source projects before adopting them. /scout gives you license compatibility, maintenance health, dependency analysis, and structured adoption recommendations."
date: 2026-08-23T00:00:00+00:00
draft: false
weight: 40
toc: true
---

## Prerequisites

Before using `/scout`, complete these setup steps:

1. Run `uf init` in your project root. This scaffolds the `pinkman.md` agent file alongside your other Unbound Force agents. Pinkman handles open source intelligence — license analysis, maintenance health checks, and adoption recommendations.

2. Start Dewey if you want cross-session memory. Dewey is optional but recommended. When running, Pinkman stores every evaluation as a learning, so future scouting sessions draw on past analysis instead of starting from scratch.

3. Verify Pinkman is available by checking that `.opencode/agents/pinkman.md` exists in your project. The agent activates automatically when you invoke any `/scout` command.

## Discover Mode

Discover mode is the default. Use it to find open source projects by keyword, filtered for license compatibility with your project.

### Find Projects by Keyword

1. Run the scout command with a topic:

```bash
/scout static analysis Go
```

2. Pinkman searches GitHub for projects matching your query. It filters results against your project's license (detected from `LICENSE` or `go.mod`) and returns a ranked list.

3. Review the output. Each result includes the project name, license verdict, star count, last commit date, and a one-line summary of what the project does.

### Example Output

```
🔍 Scouting: "static analysis Go"
License context: Apache-2.0 (from LICENSE)

  # │ Project                        │ License     │ Compat │ Stars │ Last Commit │ Summary
  1 │ golangci/golangci-lint          │ GPL-3.0     │ ⚠ WEAK │ 15.2k│ 2 days ago  │ Fast Go linters runner
  2 │ dominikh/go-tools               │ MIT         │ ✅ OK  │ 6.1k │ 5 days ago  │ Staticcheck and related tools
  3 │ mgechev/revive                  │ MIT         │ ✅ OK  │ 4.8k │ 1 week ago  │ Extensible Go linter framework
  4 │ securego/gosec                  │ Apache-2.0  │ ✅ OK  │ 7.6k │ 3 days ago  │ Security-focused Go source scanner

Found 4 projects. Use /scout --report <url> for a full evaluation.
```

Pinkman marks GPL-3.0 as `⚠ WEAK` compatibility when your project uses Apache-2.0. This does not mean you cannot use the project — it means you need to evaluate the integration boundary (binary vs. library linking) before adopting it.

## Understanding Results

### License Verdicts and Compatibility Tiers

Pinkman classifies license compatibility into three tiers:

- **✅ OK** — The dependency's license is fully compatible with your project's license. No restrictions on how you integrate it.
- **⚠ WEAK** — The dependency's license imposes conditions that may conflict with your project's license. Review the specific terms before adopting. Common examples: GPL libraries used in Apache-2.0 projects, or AGPL services called over the network.
- **❌ BLOCK** — The dependency's license is incompatible with your project's license. Adopting it would require relicensing your project or isolating the dependency behind a process boundary.

### Dependency Lists and Overlap Tables

When Pinkman evaluates multiple projects in the same session, it generates an overlap table. This table shows shared transitive dependencies across candidates. High overlap means adopting multiple projects adds minimal dependency weight. Low overlap means each project brings its own dependency tree, increasing your supply chain surface area.

```
Dependency Overlap (2 of 4 candidates share deps):

  Dependency           │ golangci-lint │ revive │ gosec
  golang.org/x/tools   │ ✓             │ ✓      │ ✓
  golang.org/x/mod     │ ✓             │ ✓      │ ✗
  github.com/fatih/color│ ✓            │ ✗      │ ✗
```

Use this table to minimize the number of new transitive dependencies your project takes on.

## Trend Mode

Trend mode identifies projects gaining momentum. Use it to spot emerging tools before they become mainstream.

### Identify Trending Projects

1. Run the scout command with the `--trend` flag:

```bash
/scout --trend MCP servers
```

2. Pinkman queries GitHub's trending data and cross-references it with star velocity, fork rate, and contributor growth over the past 30 days.

3. Review the quantitative indicators. Each result includes growth metrics alongside the standard project metadata.

### Example Output

```
📈 Trending: "MCP servers" (30-day window)

  # │ Project                        │ ⭐ Velocity │ 🍴 Forks/mo │ 👥 New Contributors │ License │ Summary
  1 │ modelcontextprotocol/servers    │ +2,340      │ +187         │ +42                  │ MIT     │ Reference MCP server implementations
  2 │ punkpeye/awesome-mcp-servers    │ +1,890      │ +95          │ +18                  │ CC0-1.0 │ Curated list of MCP servers
  3 │ mark3labs/mcp-go                │ +680        │ +34          │ +11                  │ MIT     │ Go SDK for building MCP servers

Star velocity = net new stars in the past 30 days.
```

High star velocity with low contributor growth may indicate hype without sustained community investment. Look for projects where contributor count grows proportionally to stars — that signals genuine adoption, not just visibility.

## Audit Mode

Audit mode checks your existing dependencies for updates, license changes, and maintenance risks. Run it regularly to catch supply chain issues before they become vulnerabilities.

### Audit Your Dependencies

1. Run the scout command with the `--audit` flag and point it at your dependency manifest:

```bash
/scout --audit go.mod
```

2. Pinkman reads your `go.mod` (or `package.json`, `Cargo.toml`, `pyproject.toml`) and checks each dependency against its upstream repository.

3. Review the findings. Pinkman flags four categories of risk: outdated versions, license changes since your pinned version, unmaintained projects, and known vulnerabilities.

### Example Output

```
🔎 Auditing: go.mod (14 direct dependencies)

  Status │ Dependency                     │ Pinned  │ Latest  │ Issue
  ⚠ OLD  │ github.com/spf13/cobra         │ v1.8.0  │ v1.9.1  │ 3 minor versions behind
  ❌ LIC │ github.com/example/lib          │ v2.1.0  │ v3.0.0  │ License changed: MIT → BSL-1.1
  ⚠ MAINT│ github.com/old/tool             │ v0.4.2  │ v0.4.2  │ No commits in 18 months
  ✅ OK  │ golang.org/x/text               │ v0.16.0 │ v0.16.0 │ Up to date

Summary: 1 license change (action required), 1 outdated, 1 unmaintained, 11 healthy.
```

The `❌ LIC` flag demands immediate attention. A license change from MIT to BSL-1.1 means the upstream project switched to a source-available license. Pin your current version and evaluate whether the new license terms are acceptable before upgrading.

## Report Mode

Report mode generates a structured adoption recommendation for a single project. Use it when you have narrowed your candidates and need a thorough evaluation before committing.

### Generate an Adoption Report

1. Run the scout command with the `--report` flag and a repository URL:

```bash
/scout --report https://github.com/example/project
```

2. Pinkman clones the repository metadata (not the full source), analyzes its README, license, dependency tree, CI configuration, release cadence, and contributor distribution.

3. Review the structured report. It follows a consistent format designed for decision-making.

### Example Report Structure

```
📋 Adoption Report: example/project
Generated: 2026-08-23

## Identity
- Name: project
- License: Apache-2.0 (✅ compatible)
- Language: Go (98.2%)
- Latest release: v2.4.1 (2026-08-10)

## Maintenance Health
- Commits (90 days): 147
- Open issues: 23 (avg response: 2.1 days)
- Open PRs: 4 (avg merge time: 3.8 days)
- Contributors (90 days): 12
- Bus factor: 3 (top 3 contributors = 72% of commits)
- CI: GitHub Actions (passing)

## Dependency Profile
- Direct dependencies: 8
- Transitive dependencies: 34
- Overlap with your project: 6 shared deps
- Known vulnerabilities: 0

## Release Cadence
- Releases (12 months): 11
- Avg time between releases: 33 days
- Follows semver: Yes

## Recommendation
- Verdict: ✅ ADOPT
- Confidence: HIGH
- Rationale: Active maintenance, compatible license, low dependency
  footprint, consistent release cadence, healthy bus factor.
- Risks: None identified.
```

The bus factor metric counts how many top contributors account for the majority of recent commits. A bus factor of 1 means a single maintainer — high risk for long-term adoption. A bus factor of 3 or higher indicates distributed ownership.

## Persistent Reports

Pinkman saves every report to disk for future reference. You do not need to re-run evaluations to recall past results.

### Finding Saved Reports

1. Navigate to the `.uf/pinkman/reports/` directory in your project root. Pinkman creates this directory automatically on the first `/scout --report` invocation.

2. Reports are saved as Markdown files named by project and date:

```
.uf/pinkman/reports/
├── example-project-2026-08-23.md
├── mark3labs-mcp-go-2026-08-15.md
└── spf13-cobra-2026-08-01.md
```

3. Open any report file to review the full evaluation. The format matches the terminal output, so you can share reports in pull requests or architecture decision records without reformatting.

4. Pinkman appends an update log to existing reports when you re-evaluate the same project. This creates a historical record of how a dependency's health changes over time.

## Dewey Integration

When Dewey is running, Pinkman stores every evaluation as a semantic learning. This transforms `/scout` from a stateless tool into a knowledge-building system.

### How Past Evaluations Inform Future Scouting

1. **Automatic recall**: When you run `/scout` for a topic you have evaluated before, Pinkman queries Dewey for prior learnings. If you evaluated MCP servers last month, a new `/scout --trend MCP servers` command surfaces those earlier findings alongside fresh data. You see what changed without re-reading old reports.

2. **Cross-project context**: Dewey indexes learnings across all your projects. If you evaluated `spf13/cobra` in Project A, that evaluation is available when you scout dependencies in Project B. Pinkman tells you: "Previously evaluated in project-a on 2026-08-01 — verdict: ADOPT (HIGH confidence)."

3. **License pattern detection**: Over time, Pinkman builds a map of license changes across your dependency graph. If a maintainer has changed licenses on previous projects, Pinkman flags this pattern when you evaluate their new projects. This early warning helps you avoid dependencies with unstable licensing.

4. **Contradiction resolution**: When a project's health metrics change between evaluations (e.g., maintenance score dropped from healthy to unmaintained), Dewey's compilation step resolves the contradiction temporally. The compiled article reflects the current state while preserving the history, so you can trace when and why a dependency's status changed.

Start Dewey with `dewey serve` before your scouting session to enable this integration. Without Dewey, `/scout` still works — you just lose cross-session memory and must rely on the local report files in `.uf/pinkman/reports/`.
