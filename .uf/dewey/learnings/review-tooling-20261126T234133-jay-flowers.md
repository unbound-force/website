---
tag: review-tooling
author: jay-flowers
category: gotcha
created_at: 2026-11-26T23:41:33Z
identity: review-tooling-20261126T234133-jay-flowers
tier: draft
as_of: "website@93fd03137f002169b7669b2088812b7a401080f7"
evidence:
  - "https://github.com/unbound-force/website/pull/461"
  - ".github/workflows/ci.yml"
---

Glob tools silently skip hidden directories (a .github/workflows/* glob returned no files over a directory containing ci.yml), so enumerate hidden directories with a direct directory read before making claims about a repo's CI triggers.
