---
tag: ci-parity-gate
author: jay-flowers
category: gotcha
created_at: 2026-11-06T17:37:50Z
identity: ci-parity-gate-20261106T173750-jay-flowers
tier: draft
---

SRE council re-verification gotcha (website PR #315, docs/unleash-tier-routing): a REQUEST CHANGES finding claiming "the real build has never run against this branch's content" was based on a STALE session-database snapshot taken before `npm ci` completed. Root causes and countermeasures: (1) Always corroborate session-DB reads about build/install state with fresh on-disk checks — read `node_modules/<pkg>/package.json` directly and grep `public/` for the changed content to confirm a themed build rendered it. (2) Use tool-call timestamps in the session DB to establish causal ordering: a degenerate themeless Hugo run (89 pages, "found no layout file" warnings, Homebrew hugo) at 17:01 PRECEDED `npm ci`; themed builds (224 pages, exit 0) followed at 17:02/17:05/17:31 — reading only the earlier snapshot inverts the conclusion. (3) Glob tools exclude `node_modules` by default — "No files found" for node_modules patterns is a false negative, not evidence of absence; use direct file reads. (4) Hugo version skew (local 0.161.1 vs CI-pinned 0.155.1 via SHA-pinned peaceiris/actions-hugo) is acceptable for prose-only diffs when both versions are verified building the content. (5) Per the shared severity pack, MEDIUM findings do not block merge — a lone MEDIUM should not produce a blocking REQUEST CHANGES verdict.
