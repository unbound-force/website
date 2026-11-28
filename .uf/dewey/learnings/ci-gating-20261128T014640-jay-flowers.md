---
tag: ci-gating
author: jay-flowers
category: gotcha
created_at: 2026-11-28T01:46:40Z
identity: ci-gating-20261128T014640-jay-flowers
tier: draft
as_of: "website@d75b8adb45ab6752ea77a15ae57c01097df43707"
---

Before gating a required build on a scope classifier's skip classes, prove the skip classes disjoint from the build's ACTUAL input surface by enumerating its mounts/config — directory-name intuition ("dot-dirs are ignored by the SSG") can silently fail when module mounts remap paths, and it is the disjointness proof, not the classifier, that makes skip-equals-safe true. Validated on website PR #467: config/_default/module.toml mounts only content/, data/, layouts/, i18n/, archetypes/, assets/, static/ and node_modules theme paths — nothing from .uf/ or .opencode/.
