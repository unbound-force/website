# Implementation Plan: Dewey vs Karpathy Blog Post

**Branch**: `019-dewey-karpathy-blog` | **Date**: 2026-04-07 | **Spec**: [spec.md](spec.md)

## Summary

Create a blog post comparing Dewey's index-then-search approach with Karpathy's compile-then-navigate "LLM Knowledge Base" approach. Single new file: `content/blog/dewey-vs-karpathy.md`.

## Technical Context

Hugo static site, Markdown blog post, Doks theme. No new dependencies.

## Constitution Check

- **I. Content Accuracy**: PASS — Dewey claims verified via Dewey semantic search (40 MCP tools, 4 source types). Karpathy claims sourced from his X post and VentureBeat article.
- **II. Minimal Footprint**: PASS — 1 new Markdown file.
- **III. Visitor Clarity**: PASS — comparison framing helps readers choose the right approach.

## File Change Matrix

| File                                | Change          |
| ----------------------------------- | --------------- |
| `content/blog/dewey-vs-karpathy.md` | NEW — blog post |
