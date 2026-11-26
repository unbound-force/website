---
tag: docs-accuracy
author: jay-flowers
category: pattern
created_at: 2026-11-26T23:41:31Z
identity: docs-accuracy-20261126T234131-jay-flowers
tier: draft
as_of: "website@93fd03137f002169b7669b2088812b7a401080f7"
evidence:
  - "https://github.com/unbound-force/website/pull/461"
  - "package.json"
  - "go.mod"
  - ".github/workflows/"
---

For docs-only website PRs, every factual claim (commands, versions, CI triggers, path descriptions) is mechanically checkable against package.json, go.mod, .github/workflows/, and the content tree — verify those before judging prose quality.
