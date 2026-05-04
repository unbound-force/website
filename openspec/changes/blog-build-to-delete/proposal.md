## Why

No existing content addresses the question of harness decay — which parts of an AI agent system survive model upgrades and which become dead weight. This is the most provocative and honest post in the harness engineering series, directly acknowledging that some components of Unbound Force may become unnecessary as models improve.

GitHub Issue: #92

## What Changes

- Add a new blog post at `content/blog/build-to-delete.md` covering harness decay, the decay-resistant vs decay-prone distinction, and practical pruning methodology

## Capabilities

### New Capabilities
- `blog-build-to-delete`: Blog post on harness decay and the discipline of pruning agent system components

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- New file: `content/blog/build-to-delete.md`
- No changes to existing pages

## Constitution Alignment

Assessed against the **Unbound Force Website** constitution (.specify/memory/constitution.md).

### I. Content Accuracy

**Assessment**: PASS

The harness inventory classification (decay-resistant vs decay-prone) is sourced from the analysis document and verified against actual upstream components. Anthropic model progression data attributed to Liu article.

### II. Minimal Footprint

**Assessment**: PASS

Pure Markdown blog post.

### III. Visitor Clarity

**Assessment**: PASS

The post provides a practical framework (decay classification + pruning methodology) that visitors can apply to their own agent systems.
