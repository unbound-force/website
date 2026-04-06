## 1. Fix Stale References (Issues #21, #22)

- [x] 1.1 Remove "Bun" from development tools list in content/docs/getting-started/quick-start.md
- [x] 1.2 Remove "Bun" from development tools list in content/docs/getting-started/developer.md
- [x] 1.3 Remove "Bun" and ", `.hive/` initialization" from development tools list in content/docs/getting-started/common-workflows.md
- [x] 1.4 Replace "Hivemind semantic memory" with "Dewey semantic memory" in the /unleash pipeline table in content/docs/getting-started/common-workflows.md
- [x] 1.5 Replace 3 "Hivemind" references with "Dewey" in content/blog/unleash-in-practice.md (lines ~103, ~105, ~151) — also remove the GitHub link wrapper around the Hivemind mention
- [x] 1.6 Replace 2 "Hivemind" references with "Dewey" in content/docs/team/the-divisor.md (lines ~104, ~106)

## 2. Update Divisor Persona Count (Issue #22)

- [x] 2.1 Update "council of five dynamically discovered personas -- five canonical roles" to reflect 8 personas (5 review + 3 content) in content/docs/team/the-divisor.md
- [x] 2.2 Update "five personas" references in the Cobalt-Crush and Collective Approval sections of content/docs/team/the-divisor.md
- [x] 2.3 Update "five sub-personas" to "eight sub-personas" in content/docs/team/\_index.md
- [x] 2.4 Update "five sub-personas" in content/docs/contributing/\_index.md

## 3. Add Content Agents (Issue #20)

- [x] 3.1 Add a "Content Creation Personas" section to content/docs/team/the-divisor.md after the existing review personas, documenting The Scribe (tech docs, temperature 0.1), The Herald (blog/announcements, 0.4), and The Envoy (PR/comms, 0.5)
- [x] 3.2 Update the Divisor summary in content/docs/team/\_index.md to mention content creation capability alongside review

## 4. Update Convention Packs and Contributing (Issue #20)

- [x] 4.1 Add `content.md` (tool-owned, language-agnostic writing standards) and `content-custom.md` (user-owned stub) rows to the convention packs table in content/docs/getting-started/developer.md
- [x] 4.2 Add Replicator to the issue reporting list and links section in content/docs/contributing/\_index.md (if not already present)

## 5. Fix Session Ritual (Issue #20)

- [x] 5.1 Update the Session Ritual section in content/docs/getting-started/developer.md: replace `hive_ready()`/`hive_sync()` references with current workflow patterns, reference `/finale` as end-of-session

## 6. Verification

- [x] 6.1 Run `npm run build` and verify zero errors
- [x] 6.2 Grep for "Hivemind" in content/ — zero results expected
- [x] 6.3 Grep for "Bun" in setup tool lists — zero results expected
- [x] 6.4 Grep for "five personas" / "five sub-personas" / "five canonical" — zero results expected
- [x] 6.5 Verify constitution alignment: Observable Quality PASS (all factual claims now accurate)
