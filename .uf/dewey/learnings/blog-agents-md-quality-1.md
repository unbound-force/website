---
tag: blog-agents-md-quality
category: reference
created_at: 2026-05-03T18:47:59Z
identity: blog-agents-md-quality-1
tier: draft
---

When writing blog posts about /agent-brief and AGENTS.md quality for the Unbound Force website, the 5-tier scoring rubric has a downgrade rule that is easy to miss: if Tier 1C sections are applicable (project has a constitution or spec framework) but missing, the score is downgraded by one level. This means an otherwise Strong AGENTS.md becomes Adequate. The upstream source at .opencode/command/agent-brief.md lines 273-279 defines the rubric, and lines 251-267 define the Tier 1C quality checks. The 12 structural checks include: 5 Tier 1 section presence, 2 Tier 1C conditional checks, build code block presence, directory tree accuracy, constitution reference, spec framework reference, cross-framework bridge, and branch protection instruction.
