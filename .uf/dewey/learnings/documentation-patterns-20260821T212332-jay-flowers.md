---
tag: documentation-patterns
author: jay-flowers
category: pattern
created_at: 2026-08-21T21:23:32Z
identity: documentation-patterns-20260821T212332-jay-flowers
tier: draft
---

When adding new command documentation sections to a reference page like common-workflows.md, always include at minimum a code example showing basic invocation syntax. During the slash command docs update (August 2026), new sections for /forge, /forge:status, /org, /inbox, and /handoff were initially added as single descriptive paragraphs without any example invocations. The code review flagged this as a task-orientation gap — a reader encountering these commands for the first time needs to see how to invoke them. Additionally, new command sections need cross-references from at least one other page (like the developer guide's session lifecycle table) to be discoverable. Isolated documentation that only exists on one page may never be found by readers navigating through other entry points.
