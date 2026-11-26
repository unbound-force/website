---
tag: review-evidence
author: jay-flowers
category: gotcha
created_at: 2026-11-26T23:41:30Z
identity: review-evidence-20261126T234130-jay-flowers
tier: draft
as_of: "website@93fd03137f002169b7669b2088812b7a401080f7"
evidence:
  - "https://github.com/unbound-force/website/pull/461"
---

A PR body claiming "every path it lists exists" verifies repo-relative paths only — external URLs in a docs diff need separate confirmation (webfetch), because grep, glob, and the build cannot see them.
