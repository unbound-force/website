# Quickstart: Homepage UX Redesign and Analytics

## What Success Looks Like

After implementation, the homepage guides a first-time visitor from "what is this?" to "let me try it" in a single scroll. The primary CTA keeps them on-site, the "How It Works" section explains the workflow before the feature cards, and Google Analytics tracks engagement for data-driven iteration.

## Verification

```bash
npm run dev    # Start dev server
```

1. **Hero**: Primary CTA says "Get Started" -> /docs/getting-started/quick-start/
2. **Hero**: Secondary CTA says "Explore on GitHub" -> github.com/unbound-force
3. **Hero**: No "View Gaze" button
4. **Hero**: Lead text mentions autonomous pipeline capability
5. **Hero**: Badge says something capability-specific (not "AI Agent Swarm")
6. **How It Works**: 3-step section visible between hero and features
7. **How It Works**: Responsive on mobile (vertical stacking)
8. **Feature cards**: /unleash card is visually larger/more prominent
9. **Feature cards**: Headings are action/benefit-oriented
10. **Blog**: Shows 2-3 posts (not just 1)
11. **Projects**: Descriptions include benefit framing
12. **CTA**: No duplicate CTA sections at bottom
13. **Analytics**: View page source, search for "G-969E28NTFN" — script present
14. **Build**: `npm run build` passes with zero errors
15. **Dark mode**: Toggle dark mode, verify all new sections render correctly
