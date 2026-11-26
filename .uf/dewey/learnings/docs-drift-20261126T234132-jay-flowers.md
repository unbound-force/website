---
tag: docs-drift
author: jay-flowers
category: pattern
created_at: 2026-11-26T23:41:32Z
identity: docs-drift-20261126T234132-jay-flowers
tier: draft
as_of: "website@93fd03137f002169b7669b2088812b7a401080f7"
evidence:
  - "https://github.com/unbound-force/website/pull/461"
  - "package.json"
  - "go.mod"
---

When a README restates version prerequisites, cite the machine-readable sources inline (package.json engines, go.mod) so future drift is checkable rather than silent.
