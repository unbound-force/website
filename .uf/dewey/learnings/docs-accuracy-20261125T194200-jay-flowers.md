---
tag: docs-accuracy
author: jay-flowers
category: pattern
created_at: 2026-11-25T19:42:00Z
identity: docs-accuracy-20261125T194200-jay-flowers
tier: draft
as_of: "website@e481e6bbbe0d05dcd3a609769e355f32b66e0329"
evidence:
  - "dewey:content/docs/zero-dot-force/_index.md"
  - "dewey:content/docs/projects/gaze.md"
  - "session:ses_debd8fd35ffePvgKKbnDI78wnr"
  - "session:ses_debce1aceffevMGcFdQGwEQK3t"
---

Install commands published on the Unbound Force website are supply-chain surface: a static site build (`npm run build` / hugo) validates internal links but never registry/package names. Before approving a docs PR that publishes install commands, live-verify the exact registry distribution name (npm view, PyPI JSON API, gh api repos/&lt;owner&gt;/&lt;repo&gt;/readme) — repo name, distribution name, and installed binary can all legitimately differ (zero-dot-force/snake-eyes repo → PyPI `snake-eyes-analyzer` → `snake-eyes` binary), and sibling-repo CI examples may use either form (gaze#375 documented `uv tool install snake-eyes` while the correct distribution name is `snake-eyes-analyzer`). In PR #453 every claim checked out live (npm reading-stone 0.2.2, PyPI snake-eyes-analyzer 0.1.0 requiring Python >=3.11, all four zero-dot-force repos public), which is what made approval possible. Divisor children with denied bash/webfetch cannot run these checks — the parent session must.
