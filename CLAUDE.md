# Maven Brand Toolkit

## What This Is

A generative brand exploration toolkit for Maven's Hard Launch campaign ("Learn from Humans"). Built as pure HTML/CSS/JS with Canvas 2D rendering.

## Project Context

Maven is launching a "Learn from Humans" campaign — a major brand moment showcasing their instructor community. This toolkit explores Maven's brand system through code, creating generative patterns that can be used in the landing page design.

### The Three Brand Elements

1. **Architectural Grid** — thin-line grid with quarter-circle arcs, diagonals, and circles spanning 1-3 cells. Inspired by architectural drawings. Metaphor: growth (stacking/building) and learning (building blocks).

2. **Dither/Stippling** — instructor photos rendered as vertical lines of varying width. Connects to Maven's pillar logo (also vertical lines). Creates a "digital renders human" moment.

3. **Pattern Lines** — horizontal stripes in brand colors evoking speed and progress.

## File Structure

```
maven-brand-toolkit/
├── index.html          # The toolkit — 4 tabs: Grid, Dither, Pattern Lines, Composition
├── images/             # 23 instructor headshots (JPG/PNG)
├── docs/
│   ├── brand-system.md                 # Maven brand system documentation
│   ├── editorial-design-research.md    # Editorial design principles for photography layouts
│   ├── generative-patterns-research.md # Technical research on implementing the patterns
│   ├── photography-first-research.md   # Photography-first direction research
│   └── session-brand-exploration.md    # Session notes on the approach
└── CLAUDE.md           # This file
```

## How to Run

```bash
# From the repo root:
python -m http.server 3939
# Then open http://localhost:3939/
```

Or any local HTTP server — the file:// protocol won't load images due to CORS.

## Maven Brand System Quick Reference

### Colors
- Hero dark: `#080c28` (lapis-900)
- Grid lines: `#101b57` (lapis-800)
- Primary blue: `#1e51ca` (lapis-500)
- CTA on dark: `#7cbeff` (brand-light)
- Lime accent: `#cdff92` (brand-highlight)
- Text on dark: `#f7fbff` (lapis-25, not pure white)

### Category Colors
- AI: `#460952` (deep purple)
- Product: `#B33600` (burnt orange)
- Engineering: `#00746B` (teal)
- Design: `#772CBB` (purple)
- Marketing: `#943170` (mauve)
- Leadership: `#A9202A` (burgundy)
- Founders: `#077202` (forest green)

### Typography
- Headlines: STK Bureau Serif (Light 300, Book 400, hero at 200)
- Body/UI: STK Bureau Sans (Book 400, Medium 500, SemiBold 600)
- Letter-spacing on headlines: approximately -4% of font-size
- Web stand-ins: Source Serif 4 + Inter

### Grid Patterns (Only These)
- Quarter-circle arcs: spanning 1x1, 2x2, or 3x3 cells
- Diagonal lines: spanning 1x1, 2x2, or 3x3 cells
- Full circles: spanning 2x2 or 3x3 cells
- NO crosses, NO double-arcs, NO leaf shapes

## Design Direction

7 layout hypotheses are being explored (from 10, 3 were eliminated):
- H1: The Gentlewoman Cover (asymmetric split, one dominant portrait)
- H2: The Vanity Fair Spread (layered depth ensemble)
- H5: The Bloomberg Bold (typography as structure)
- H6: The Kinfolk Breath (minimal, white space)
- H7: The Staggered Column (cascading vertical columns)
- H8: Full-Bleed Hero + Grid (portrait fills viewport)
- H9: The Succession Ensemble (collage of fragments)

## Next Steps

1. Refine grid generator to perfectly match brand book reference
2. Test dither effect with real instructor photos
3. Apply brand patterns to the 7 layout hypotheses
4. Build full prototype compositions
5. Deploy to GitHub Pages for team review

## Related Repos
- Art direction concepts (previous): https://github.com/acorda-joao/hard-launch-concepts
- Original report: https://github.com/rqcai200/experts-report
- Expert pages: https://github.com/acorda-joao/maven-expert-pages
