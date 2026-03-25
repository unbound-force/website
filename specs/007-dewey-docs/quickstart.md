# Quickstart: Dewey Documentation

**Date**: 2026-03-25
**Branch**: `007-dewey-docs`

## Overview

After this change, the Unbound Force website has
comprehensive Dewey documentation: a getting-started
guide, a tool reference page, and updated role guides
that show how each hero uses Dewey for context.

## New Pages

### knowledge.md (Getting Started)

Covers:

1. What is Dewey? (1 paragraph)
2. Installation (Homebrew + go install fallback)
3. Initialization (`dewey init`)
4. Source configuration (YAML examples for all 3 types)
5. Indexing (`dewey index`)
6. Verification (`dewey status`)
7. 3-tier degradation explanation
8. OpenCode integration (opencode.json config)

### dewey.md (Team/Tools)

Covers:

1. What Dewey does (semantic knowledge layer, MCP
   server with persistent index)
2. How it relates to graphthulhu (hard fork, superset)
3. Query capabilities (structured + semantic)
4. Content sources (disk, GitHub, web)
5. Embedding model (Granite, enterprise provenance)
6. How heroes use it (brief per-hero overview)

## Updated Pages

### common-workflows.md

Add a "Knowledge Context" subsection to the hero
lifecycle description explaining that Dewey provides
semantic context to all heroes during their stages.

### developer.md

Add "Knowledge Retrieval with Dewey" subsection
under the Swarm section explaining how Cobalt-Crush
uses Dewey for toolstack docs and patterns.

### product-owner.md

Add "Knowledge Retrieval with Dewey" subsection
explaining how Muti-Mind uses Dewey for cross-repo
backlog patterns.

### product-manager.md

Add "Knowledge Retrieval with Dewey" subsection
explaining how Mx F uses Dewey for velocity trends
and retrospective data.

### tester.md

Add "Knowledge Retrieval with Dewey" subsection
explaining how Gaze uses Dewey for quality patterns
and test baselines.

## Verification

```bash
npm run build    # Must succeed with zero errors
npm run dev      # Visual verification of all pages
```

Check:

- New pages appear in sidebar navigation
- Dark mode renders correctly
- No broken internal links
- Content is self-contained (Dewey guide readable
  without prior hero knowledge)
