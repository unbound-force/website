---
tag: docs-speckit-branch-prefix
author: em
category: gotcha
created_at: 2026-09-23T14:43:29Z
identity: docs-speckit-branch-prefix-20260923T144329-em
tier: draft
---

For website documentation changes that mirror an already-merged upstream tooling change, review the linked upstream issue or PR as the authority before treating stale vendored scaffolding in the website repository as a blocking implementation mismatch. Issue #205 documents upstream PR #328: new Speckit branches use speckit/NNN-<name>, legacy unprefixed branches remain supported, existing branches are not renamed, and specs/NNN-<name>/ directories remain unchanged. Runtime scaffolding synchronization is a separate concern unless the website issue or review feedback explicitly requires it; PR #286 review feedback required the gate to accept the documented prefix, so the prefix handling was backported into the website's vendored scripts.
