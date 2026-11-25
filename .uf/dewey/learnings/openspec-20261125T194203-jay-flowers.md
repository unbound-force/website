---
tag: openspec
author: jay-flowers
category: gotcha
created_at: 2026-11-25T19:42:03Z
identity: openspec-20261125T194203-jay-flowers
tier: draft
as_of: "website@e481e6bbbe0d05dcd3a609769e355f32b66e0329"
evidence:
  - "dewey:openspec/changes/zero-dot-force-juniors/proposal.md"
  - "dewey:.specify/memory/constitution.md"
  - "session:ses_debd8bdc0ffeKUKyV6PVLMJOrp"
---

In unbound-force/website, .specify/memory/constitution.md currently holds the generic uf scaffold (I. Autonomous Collaboration, II. Composability First, III. Observable Quality, IV. Testability, V. Security by Default), while ~30 OpenSpec proposals and prior review precedent (Dewey learning cli-reference-and-positioning-1) assess against website principles (Content Accuracy, Minimal Footprint, Visitor Clarity) that exist in no file; openspec/config.yaml and the proposal template describe a third set. When reviewing website spec artifacts, treat the three-principle citation as a stale repo-wide convention (root cause predates any single PR): flag it as a MEDIUM metadata inconsistency on the new lines, not a constitution violation by the change under review. The correct fix is a dedicated governance change restoring the website constitution and reconciling file + config + template.
