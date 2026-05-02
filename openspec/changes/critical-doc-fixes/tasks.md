## 1. Fix `/finale` merge description (#53)

- [ ] 1.1 Update `content/docs/getting-started/common-workflows.md`: Remove "Merge PR" step from the `/finale` workflow table, renumber remaining steps, update guardrails to say "Never merges the PR", update description to say "/finale wraps up the branch and creates a PR for review"
- [ ] 1.2 Update `content/docs/getting-started/quick-start.md`: Change `/finale # ship it` comment to `/finale # commit, push, create PR`
- [ ] 1.3 Update `content/docs/getting-started/developer.md`: Change "run `/finale` to commit, push, create a PR, and merge" to "run `/finale` to commit, push, and create a PR"

## 2. Fix `/unleash` branch support (#54)

- [ ] 2.1 Update `content/docs/getting-started/common-workflows.md`: Replace text stating `/unleash` rejects `opsx/*` branches with text stating it supports both `NNN-*` and `opsx/*` branches, including OpenSpec task detection behavior

## 3. Fix `uf init` file count and breakdown (#55)

- [ ] 3.1 Update `content/docs/getting-started/developer.md`: Change "approximately 50 files" to "approximately 35 files"
- [ ] 3.2 Update the breakdown table in `developer.md`: Set Commands to 8 (review-council, review-pr, constitution-check, cobalt-crush, unleash, uf-init, agent-brief, finale), set Agents to 12 (9 Divisor personas including 3 content, Cobalt-Crush, Constitution Check, Mx F Coach)

## 4. Fix `mxf` install instruction (#56)

- [ ] 4.1 Update `content/docs/getting-started/product-manager.md`: Replace the stale `brew install unbound-force/tap/unbound-force` instruction with `brew install unbound-force/tap/mxf` and add `uf setup` as an alternative

## 5. Verification

- [ ] 5.1 Run `npm run build` and confirm no build errors
- [ ] 5.2 Run `npm run dev` and visually verify all four corrected pages render correctly
- [ ] 5.3 Verify no other pages reference the stale `/finale` merge behavior or `/unleash` opsx rejection
