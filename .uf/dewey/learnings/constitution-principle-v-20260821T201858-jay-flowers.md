---
tag: constitution-principle-v
author: jay-flowers
category: gotcha
created_at: 2026-08-21T20:18:58Z
identity: constitution-principle-v-20260821T201858-jay-flowers
tier: draft
---

When documenting Constitution Principle V (Security by Default) on the Unbound Force website, the content MUST be sourced from the upstream .specify/memory/constitution.md, not from GitHub issue descriptions or spec summaries. Principle V covers four MUST rules: (1) dependencies verified by content hash SHA256, (2) all external inputs validated and sanitized, (3) components operate with minimum permissions, and (4) external dependencies must be justified. Compound severity escalation does NOT belong under Principle V — it is defined in the severity.md convention pack, which is a Layer 2 governance artifact, not a Layer 1 constitutional principle. This distinction matters because placing convention pack mechanics inside a constitutional principle misrepresents the governance hierarchy. The spec review council caught this as a CRITICAL finding and it required correction before implementation could proceed.
