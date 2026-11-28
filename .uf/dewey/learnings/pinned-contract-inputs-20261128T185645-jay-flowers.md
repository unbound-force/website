---
tag: pinned-contract-inputs
author: jay-flowers
category: gotcha
created_at: 2026-11-28T18:56:45Z
identity: pinned-contract-inputs-20261128T185645-jay-flowers
tier: draft
as_of: "website@221a8184df0b84469dc7e19470807de23d674f2e"
---

Spelling out a pinned reusable workflow's default inputs at the call site turns an implicit upstream-owned policy into an explicit, diff-reviewable contract — but the "these are the defaults" comment is version-relative: anchor it to the pinned version ("defaults as of vN") and re-diff the inputs against the new version's defaults on every pin bump, or the comment silently misleads after the next upgrade.
