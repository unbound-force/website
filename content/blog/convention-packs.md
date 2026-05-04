---
title: "Convention Packs: Reusable Harness Templates That Travel Between Projects"
description: "Most teams encode AI agent coding standards as prose in a single file. Convention packs are versioned, numbered, and portable. Here is why that matters."
lead: "If you maintain AI agent standards as prose in a single file, convention packs show what structured, portable standards look like."
slug: "convention-packs"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 43
toc: true
categories: ["Engineering"]
tags: ["agents", "quality", "documentation", "architecture"]
contributors: ["Unbound Force"]
---

## The Prose Standards Problem

Every project that uses AI coding agents needs coding standards. The agent needs to know how to format code, name variables, handle errors, structure tests, and manage dependencies. Without explicit standards, the agent guesses — and its guesses are inconsistent across sessions, files, and projects.

Most teams solve this by writing prose in their AGENTS.md or system prompt: "Use camelCase for variables. Write tests for all public functions. Handle errors with wrapped returns." This works for a single project. Then you start a second project. You copy the AGENTS.md, adapt it, and move on.

Six months later, you have five projects with five versions of the same standards. The Go project requires wrapped errors. The TypeScript project forgot to mention it. The content standards evolved in one repo but not the others. Updates drift. Nobody knows which version is canonical.

## What Convention Packs Look Like

Convention packs replace prose standards with structured, numbered, severity-classified rules. Each pack is a Markdown file with YAML frontmatter, deployed to projects via `uf init`.

Here is what a rule looks like in the Go convention pack:

```markdown
- **CS-001** [MUST] Format all Go source files with `gofmt`.
  No manual formatting overrides.
- **CS-006** [MUST] Wrap errors with `fmt.Errorf("context: %w", err)`
  to preserve the error chain.
- **CS-010** [SHOULD] Keep functions focused on a single responsibility.
  Extract helper functions when a function exceeds ~50 lines.
```

Each rule has three components:

1. **A stable identifier** (CS-001, AP-003, SC-002). The prefix indicates the category: CS for Coding Style, AP for Architectural Patterns, SC for Security Checks, TC for Testing Conventions, DR for Documentation Requirements. The number is sequential within the category and never reused.

2. **A severity level** (MUST, SHOULD, MAY per RFC 2119). MUST rules are non-negotiable — violations are automatic failures. SHOULD rules are expected but can be overridden with justification. MAY rules are suggestions.

3. **The rule itself**. Concrete, actionable, unambiguous. Not "write good tests" but "test observable side effects, not implementation details."

Unbound Force ships with packs for different domains:

| Pack | Domain | Example Rules |
|------|--------|---------------|
| `default.md` | All projects | Commit message format, PR conventions, error handling patterns |
| `go.md` | Go projects | `gofmt`, import organization, `Options`/`Result` pattern, `embed.FS` for assets |
| `content.md` | Content/docs | Frontmatter requirements, heading structure, link conventions |
| `typescript.md` | TypeScript projects | Formatting, type safety, module patterns |
| `severity.md` | Quality gates | CRAP score thresholds, coverage requirements, auto-fix policies |

## The Extension Pattern

Convention packs solve the portability problem — the same `go.md` deploys to every Go project. But projects have different needs. A web API project has different architectural patterns than a CLI tool.

The `-custom.md` pattern handles this. For every pack `<name>.md`, a project can create `<name>-custom.md` with project-specific additions or overrides:

```
.opencode/uf/packs/
  go.md              # Standard Go rules (shipped by uf init)
  go-custom.md       # Project-specific Go rules (you write this)
  content.md         # Standard content rules
  content-custom.md  # Project-specific content rules
```

The custom file can add new rules, tighten existing ones, or document project-specific exceptions. The base pack is never modified directly — it gets updated when you run `uf init` again. Your custom rules persist across updates because they live in a separate file.

This is the same pattern as configuration layering in tools like ESLint or Prettier: a shared base config that extends cleanly with project-specific overrides. The difference is that convention packs are designed for AI agent consumption — structured rules with stable identifiers that agents can reference in their reasoning.

## Why Structure Matters for Agents

Prose instructions work for humans because humans interpret context. An experienced developer reads "handle errors properly" and applies their judgment about what "properly" means in each situation.

AI agents interpret instructions differently. They follow what they are told, but the specificity of the instruction determines the consistency of the output. "Handle errors properly" produces different results on different invocations. "Wrap errors with `fmt.Errorf('context: %w', err)` to preserve the error chain" (rule CS-006, MUST) produces the same result every time.

The structure of convention packs — numbered rules, explicit severity, concrete examples — gives agents three things that prose does not:

**Deterministic reference.** When a review agent flags a violation, it can cite "CS-006 [MUST]" rather than "the part about error handling." The stable identifier makes the reference unambiguous and auditable.

**Severity-based enforcement.** Agents can distinguish between non-negotiable requirements (MUST) and best practices (SHOULD). This prevents the common failure mode of treating every standard as equally important, which leads to either paralysis (everything must be perfect) or nihilism (nothing matters because everything is optional).

**Portable consistency.** The same `go.md` produces the same coding standards across every project. When you update a rule in the canonical pack, every project that runs `uf init` gets the update. No copy-paste, no drift, no "which version are we on."

## Packs as Harness Templates

ThoughtWorks' agent taxonomy includes the concept of "harness templates" — reusable patterns that can be deployed across projects to establish consistent agent behavior (as described in Yanli Liu, "Harness Engineering," *AI Advances*, Apr 2026). Convention packs are a direct implementation of this concept.

The key insight is that coding standards are a property of the organization, not the project. An organization that values error chain preservation values it in every Go project, not just the one where someone wrote it down. Convention packs make organizational standards portable, versioned, and consistently deployed — the same way a CI workflow template ensures every project runs the same quality checks.

If you are maintaining AI agent coding standards as prose in a single AGENTS.md file, convention packs show what the structured alternative looks like. The agent gets explicit rules. The review agent gets stable identifiers to cite. The organization gets portable consistency. And when you start your next project, the standards travel with you.
