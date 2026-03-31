# Hard Launch — Brand Exploration Through Code

Session date: 2026-03-31
Previous: [[4. Research - Editorial Design & Photography]], [[3. Research - Photography First Direction]], [[2. Session - Art Direction Concepts]]

---

## What Happened

### Phase 1: Layout Hypotheses in Paper

We created 10 design hypotheses for the landing page in Paper (the design tool), each grounded in editorial design research:

| # | Name | Status |
|---|------|--------|
| H1 | The Gentlewoman Cover (asymmetric split, one dominant portrait) | **Kept** |
| H2 | The Vanity Fair Spread (layered depth ensemble) | **Kept** |
| H3 | The Monocle Wall (uniform grid) | Removed |
| H4 | The Bento Editorial (mixed-size bento grid) | Removed |
| H5 | The Bloomberg Bold (typography as structure) | **Kept** |
| H6 | The Kinfolk Breath (minimal, white space, 3 portraits) | **Kept** |
| H7 | The Staggered Column (cascading vertical columns) | **Kept** |
| H8 | Full-Bleed Hero + Grid (portrait fills viewport, sidebar grid) | **Kept** |
| H9 | The Succession Ensemble (collage of fragments) | **Kept** |
| H10 | The MasterClass Grid (full-viewport face tiles) | Removed |

### The Pivot: Brand System Discovery

After reviewing the Paper explorations, João shared Maven's brand book (Sep 2025 edition). This revealed three generative brand elements that our designs completely missed:

1. **The Architectural Grid** — thin-line grid with quarter-circle arcs and diagonals (not just 90-degree lines)
2. **Image Vector Dither (Stippling)** — portraits rendered as vertical lines of varying width (connects to the pillar logo)
3. **Pattern Lines** — horizontal stripes in brand colors evoking speed and energy

These elements are **already in production** in Maven's marketing assets but haven't been applied to the web or the hard launch landing page.

### The New Approach

Instead of refining layouts in Paper with placeholder rectangles, we're pivoting to:

1. **Build a brand exploration toolkit in code** (HTML/Canvas/JS)
   - Architectural grid generator (configurable arcs, diagonals, density zones)
   - Dither/stippling effect that works on real instructor photos
   - Pattern line generator in brand colors
   - Composition engine that layers all three

2. **Apply brand patterns to the 7 remaining hypotheses**
   - Each layout now gets real brand texture, not just colors

3. **Return to Paper (or stay in code) for refined explorations**
   - With genuine brand patterns, the designs will be fundamentally different

This approach was inspired by designers like Ayush Soni who use AI and code to create brand tools — treating code as a brand exploration medium, not just a production tool.

---

## Why This Pivot Matters

The original Paper explorations used Maven's **colors and typography** but none of the brand's **graphic system**. That's like designing with a brand's font but ignoring their illustration style — it looks related but doesn't feel right.

The three brand elements (grid, dither, pattern lines) are **generative by nature**. They're defined by parameters and rules, not static images. Code is the natural medium to explore them. Paper can't express the parametric quality of these tools — you'd be drawing static approximations of dynamic patterns.

The generative approach also means:
- We can explore **variations infinitely** (change parameters, get new on-brand results)
- The brand exploration toolkit becomes **reusable** for future Maven projects
- The final prototype can **animate** these elements (grid reveals, dither transitions, line animations)
- We can apply the dither effect to **real instructor photos** and see how they look immediately

---

## Technical Research Completed

Full technical research documented in:
- [[5. Research - Generative Brand Pattern Techniques]] — algorithms, parameter ranges, implementation details for Canvas 2D
- [[Maven/Brand/1. Brand System]] — comprehensive brand system documentation

Key technical decisions:
- **Canvas 2D** for all rendering (consistent with previous prototypes in the repo)
- **Pure HTML/CSS/JS** — no build step, single HTML files
- **Seeded PRNG** for reproducible patterns across sessions
- **Multi-canvas compositing** for layered brand elements
- **SVG export** capability for production-quality dither outputs

---

## Next Steps

1. Build the brand exploration toolkit (grid, dither, pattern lines)
2. Test with real instructor photos from `hard-launch-concepts/images/`
3. Apply brand patterns to the 7 remaining layout hypotheses
4. Refine the strongest 2-3 directions into full prototypes
5. Deploy to GitHub Pages for team review

---

## Repos

- Art direction concepts (previous): https://github.com/acorda-joao/hard-launch-concepts
- Brand toolkit (new): TBD — will create when ready to deploy
