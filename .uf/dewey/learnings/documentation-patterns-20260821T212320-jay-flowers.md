---
tag: documentation-patterns
author: jay-flowers
category: pattern
created_at: 2026-08-21T21:23:20Z
identity: documentation-patterns-20260821T212320-jay-flowers
tier: draft
---

When updating command references that describe CI behavior (like "CI hard gate"), the change must propagate to ALL pages that mention that behavior, not just the primary documentation page. In the slash command docs update (August 2026), the common-workflows.md page was correctly updated from "hard gate" to "soft gate with causality analysis" per issue #221, but the quality-gates.md page and unleash-in-practice.md blog post still described a "hard gate." The code review caught this inconsistency. The pattern: when a behavioral change affects terminology used across multiple pages, grep for the old terminology across the entire content directory, not just the files listed in the task. A verification sweep for old terminology should be standard practice after any behavioral documentation update.
