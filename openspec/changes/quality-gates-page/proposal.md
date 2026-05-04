## Why

The 4-layer quality feedback stack (CI → Gaze → Divisor Council → Constitution Check) is scattered across five different pages. No single page presents the complete quality pipeline from commit to merge. Visitors who want to understand how quality is enforced must piece it together from common-workflows.md, the-divisor.md, tester.md, developer.md, and constitution.md.

GitHub Issue: #94

## What Changes

- Add a new documentation page at `content/docs/getting-started/quality-gates.md` presenting the complete quality assurance pipeline

## Capabilities

### New Capabilities
- `quality-gates-page`: Documentation page covering the layered quality feedback stack, gatekeeping value protection, doer/judge separation, and the 3-iteration review cap

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- New file: `content/docs/getting-started/quality-gates.md`
- Weight positioned after architecture.md (82), suggested: weight 83
- No changes to existing pages

## Constitution Alignment

Assessed against the **Unbound Force Website** constitution (.specify/memory/constitution.md).

### I. Content Accuracy

**Assessment**: PASS

Quality gate descriptions sourced from AGENTS.md behavioral constraints, agent persona files, and the review-council command. All claims verifiable against upstream repo.

### II. Minimal Footprint

**Assessment**: PASS

Pure Markdown documentation page, no custom elements.

### III. Visitor Clarity

**Assessment**: PASS

Consolidates scattered quality pipeline information into a single discoverable page.
