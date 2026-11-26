---
tag: toolchain-provenance
author: jay-flowers
category: gotcha
created_at: 2026-11-26T23:41:29Z
identity: toolchain-provenance-20261126T234129-jay-flowers
tier: draft
as_of: "website@93fd03137f002169b7669b2088812b7a401080f7"
evidence:
  - "https://github.com/unbound-force/website/pull/461"
  - "package-lock.json"
  - ".github/workflows/ci.yml"
  - "AGENTS.md"
---

A README claim that a tool "comes in through npm" is falsifiable in minutes by grepping the dependency lockfile, listing node_modules/.bin, and checking whether CI installs the tool with a dedicated setup action — and a pre-installed local binary silently masks the falsehood, so validate setup documentation against CI's explicit install steps, never against a developer machine where the tools may already be present.
