---
tag: content-pipeline
author: jay-flowers
category: pattern
created_at: 2026-11-25T19:42:11Z
identity: content-pipeline-20261125T194211-jay-flowers
tier: draft
as_of: "website@e481e6bbbe0d05dcd3a609769e355f32b66e0329"
evidence:
  - "dewey:content/docs/zero-dot-force/_index.md"
  - "session:ses_debd87c44ffeG7hndcQMq6gsqz"
---

When a website PR adds a new page describing a tool that already has open docs-sync issues (e.g., unbound-force/website#450 for vibe-check's ripwire pin), check whether the new page enters those issues' blast radius: a new page inherits the sync obligations of its subject even when it quotes no stale specifics. Flag the scope update (add the new page to the issue's affected-pages list) rather than filing a duplicate issue. Related: a new page that gives install commands should state non-obvious prerequisites (e.g., ripwire must be on PATH for vibe-check Python/TypeScript analysis) or explicitly defer to the repo README as canonical.
