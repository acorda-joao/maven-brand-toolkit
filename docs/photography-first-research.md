# Hard Launch — Photography-First Research

Research date: 2026-03-30
Previous session: [[2. Session - Art Direction Concepts]]

---

## The Shift

The previous art direction (ASCII/CRT, Halftone, CMYK) started from the digital/abstract side and tried to *arrive* at the human. The new direction flips it: **start from the human. Real photography, real faces, real presence.** The digital layer supports the humans, not the other way around.

This aligns with the campaign thesis — "Learn from Humans" — which the abstract direction was actually contradicting.

---

## What We Have to Work With

### 24 Instructor Headshots
All in `hard-launch-concepts/images/` — mix of JPG and PNG, studio-quality professional photos with solid backgrounds. Ready to use.

### 10-Instructor Featured Set (with topic mapping)

| Instructor | Topic | Credential |
|-----------|-------|------------|
| Shreyas Doshi | Product Sense | #1 course on Maven, Ex-Stripe & Google PM |
| Annie Duke | Decision Making | Former pro poker player, Bestselling author |
| Colin Matthews | Vibe Coding | 8,500+ students, Vibe Coding pioneer |
| Ethan Evans | Executive Influence | Ex-Amazon VP, 59,000+ students |
| Wes Kao | Executive Communication | Co-founder Maven, Executive Influence |
| Marily Nika | AI Product Management | Google/Meta AI PM Lead, PhD ML |
| Dave Kline | Management & Leadership | 20+ years leading engineering teams |
| Hamel Husain | AI Evals | #2 course on Maven, parlance-labs founder |
| Jason Liu | RAG & LLM Apps | 25,000+ signups, Applied LLM systems |
| Xinran Ma | UX Design | 17,000+ signups, AI-native design |

13 additional instructors available with topics (AI Agents, Prototyping with AI, GTM Engineering, AI Marketing, AI Analysis, AI Productivity, Career Planning, Enterprise AI, A/B Testing, ML Engineering, Design Systems, Personal Branding, AI for Engineers).

### Maven Brand System

**Colors:**
- Hero dark: `#080c28` (lapis-900) — the signature deep navy
- Grid lines: `#101b57` (lapis-800)
- Primary blue: `#1e51ca` (lapis-500)
- Accent blue: `#2465e8` (lapis-400)
- Light photo bg: `#b2d3fa` (lapis-100)
- CTA button: `rgb(124, 190, 255)` — soft medium blue
- Green accent: `#009e3d` / highlight `#CDFF92`

**Category colors (from maven.com):**
- AI: deep purple `#460952`
- Product: burnt orange `#B33600`
- Engineering: teal `#00746B`
- Design: purple `#772CBB`
- Marketing: mauve `#943170`
- Leadership: burgundy `#A9202A`
- Founders: forest green `#077202`

**Typography:**
- **STK Bureau Serif** (Light 300, Book 400) — headlines, editorial presence
- **STK Bureau Sans** (Book 400, Medium 500, SemiBold 600) — body, UI
- Font files: 8 `.woff2` in `maven-expert-pages/public/fonts/`
- Hero headline style: Bureau Serif, 72px, weight 200, line-height 80px, letter-spacing -2.88px
- The thin serif weight creates an editorial, magazine-like feel — Maven's typographic signature

**Logo:**
- SVG wordmark: pillar icon + "aven" text, 147x32 viewBox
- Both dark (`#171717`) and white (`#fff`) versions available
- Also inline in ASCII/CRT concept HTML

### Existing Patterns to Reuse
- "Learn ___ from humans" hero text with rotating topic (Source Serif/Bureau Serif, animated translateY)
- Email capture card (light and dark versions)
- Header: logo mark + wordmark + right-side label
- Auto-cycling (5s interval) + synced topic rotation
- `prefers-reduced-motion` handling

---

## Reference Board: Best-in-Class Photography-First Sites

### Tier 1 — Direct Competitors/Analogs

**MasterClass** — The gold standard for "learn from humans"
- Cinematic portraits with consistent dramatic lighting on dark backgrounds
- **Face + Name + Verb + Category** pattern: "Gordon Ramsay teaches Cooking"
- Grid of instructor tiles as both navigation and emotional hook
- Hover reveals video preview — still portrait → motion = "meeting the person"
- *Takeaway:* Consistent photography quality across all instructors is what makes the grid feel premium

**Cameo** — Face IS the product
- Entire homepage is a grid of faces organized by category
- Category filtering is the primary navigation: Athletes, Actors, Musicians...
- Card: circular/square portrait + Name + Category + Price
- *Takeaway:* Category-to-person navigation flow is exactly what "Learn from Humans" needs

### Tier 2 — Photography + Grid Excellence

**Humans of New York** — Simplicity of a face wall
- Wall-to-wall uniform portrait grid — diversity of faces creates visual richness
- **Portrait + Quote** is the proven storytelling mechanic
- No categories, no hierarchy — creates egalitarian feeling
- *Takeaway:* A simple grid of diverse faces is powerful on its own

**Apple "Shot on iPhone"** — Editorial photography at scale
- Images at varying scales: some full-bleed, some in tight grids — creates rhythm and hierarchy
- Minimal text. Photography does all the work.
- Quality through curation, not rigid templates
- *Takeaway:* Varying image sizes (full-bleed hero vs. grid) prevents monotony

**Nike** — Confrontational portraiture
- Close-up portraits, dramatic lighting, dark backgrounds
- Full-bleed single face = 70-80% of viewport with short headline
- Scroll-based reveals where new athletes emerge
- *Takeaway:* For bold/aspirational (not warm/approachable), full-bleed single portraits with bold type overlay

**Airbnb Experiences** — People-first trust design
- Photography guidelines: candid, natural, no selfies, no staged poses
- Card: **Photo-with-person + Title + Host + Rating**
- Human presence is mandated in every listing photo
- *Takeaway:* Authenticity at scale through photography guidelines, not templates

### Tier 3 — Interaction Patterns

**Codrops HoverGrid** — Grid hover reveal
- Hovering over a grid item triggers reveal animation: image expands, details appear, other items recede
- Built with GSAP + clip-path animations
- *Takeaway:* The most directly implementable interaction for "discover instructors by face"

**Lateral / Humaan** — Reactive team pages
- Photos follow mouse cursor, faces "look at" the hovered person
- Photography has personality — not corporate headshots
- *Takeaway:* Subtle cursor-reactivity creates delight without gimmick

**Spotify Wrapped** — Personalized visual identity
- "No grid, no rules" layered approach
- Every decision reverse-engineered from social sharing formats
- Bold color + large type + photography in editorial compositions
- *Takeaway:* Consider shareability — how would a "My Learning Path" card look on social?

---

## The Teach Page — What Maven Already Does Right

The teach page at `web-git-teach-page-v1.mavy.dev/teach` is the closest existing reference for what we want to build:

**Hero composition:**
- Asymmetric split: emotional promise on left + social proof (faces) on right
- Text and cards coexist in a layered composition, not rigidly separated
- Cards extend above and below the visible area, implying continuous motion

**Instructor cards:**
- Pre-rendered PNG images with colored backgrounds (brown, purple, navy, teal, burgundy, sage...)
- Each card: large photo + name + value prop ("will change the way you make decisions") + Maven URL
- The colored backgrounds per card avoid monotony and create editorial curation
- Two diagonal cascading columns — dynamic, implies "many people here, and this is alive"

**What works about it:**
- The relationship between big message (left) and human faces (right) is strong persuasion: aspiration + evidence in one glance
- Individual card colors feel curated, like magazine features
- The diagonal cascade adds energy to what could be a static grid

**What the hard launch page should do differently:**
- The teach page is for *instructors* — the hard launch is for *students*
- We need category browsing / interactive filtering (teach page is passive)
- We want the grid to be the interactive centerpiece, not just a supporting visual
- The "Learn ___ from humans" mechanic means the grid and the hero text are *connected* — changing one changes the other

---

## Grid Mechanics — Technical Options

### Layout Approaches

| Pattern | Best For | How It Works |
|---------|----------|--------------|
| **Bento Grid** (mixed sizes: 1x1, 2x2, 3x3) | Hero with featured instructor + supporting cast | CSS Grid with `grid-auto-flow: dense`, specific items `span 2/3` |
| **Uniform Square Grid** | Browse/discover all instructors | Simple CSS Grid, consistent cells |
| **Category-filtered grid** | "Show me AI instructors" | GSAP Flip or Framer Motion `layout` for smooth rearrangement |
| **Diagonal cascade** (teach page style) | Passive scrolling showcase | CSS transforms with offset positioning |

### Transition/Animation Approaches

| Pattern | Best For | Implementation |
|---------|----------|---------------|
| **Solaris flip board** | Category switch → all tiles flip to new faces | CSS 3D transforms (`rotateY(180deg)`), staggered delays per card |
| **GSAP Flip shuffle** | Grid rearranges by category | `Flip.getState()` → DOM change → `Flip.from(state)` |
| **Framer Motion layout** | React-based smooth layout transitions | `layout` prop auto-animates position/size changes |
| **Hover reveal overlay** | Show name/topic on hover | GSAP clip-path or CSS translate overlay |
| **Elastic scroll** | Parallax feel within the grid | GSAP ScrollSmoother with lag values per item |

### Recommended Technical Stack

For a **pure HTML/CSS/JS prototype** (matching existing repo approach):
- **CSS Grid** with defined areas for the bento layout
- **GSAP Flip** for category-based grid transitions (vanilla JS, no framework needed)
- **CSS 3D transforms** for the Solaris flip-card mechanic
- **Intersection Observer** for scroll-triggered reveals
- **Canvas 2D** if any photo treatment is needed (duotone, etc.)

---

## Exploration Phase (3/30 session)

### Prototypes Built (in `hard-launch-concepts/`)

**Round 1 — Three layout models (deleted):**
Built bento-grid, uniform-grid, fullscreen-grid. João rejected all three — too generic, not aligned with his vision.

**Round 2 — Grid-native concepts:**
- `grid-flip/` — The entire viewport is a structural grid of cells. Background faces scattered as 1x1 thumbnails. Featured instructor photo sliced into grid squares (5x5). Squares flip Solaris-style in a diagonal wave to reveal next instructor. **Kept as reference.**
- `grid-dissolve/` — Same grid concept, centered layout. Squares dissolve (pixelated erase) then reassemble. **Deleted.**

**Round 3 — Vertical scroll ticker (current direction):**
- `vertical-scroll/` — Two synchronized columns. Left: category queue (upcoming categories descending like a ladder into the "Learn ___ from humans" focal point). Right: instructor photos stacked vertically, scrolling down in sync. **This is the active direction.**

### The Vertical Scroll Concept (chosen direction)

**Core mechanic — the "ladder":**
- Left column: upcoming category names stack above a focal point at the bottom
- The focal point is "Learn / [Active Category] / from humans" — the active category ONLY appears here, not duplicated above
- Categories above are the queue — they descend toward the focal point
- When advancing: everything shifts DOWN one step. The bottom queued category enters the focal point. A new one appears at the top.

**Right column:**
- Instructor photos stack vertically, all SQUARE (no compression)
- The bottom/active photo is largest with name + credential beside it
- Queued photos above are smaller and faded
- Photos scroll in sync with categories

**Interaction:** Auto-advances every 4s. Mousewheel, swipe, arrow keys, up/down buttons. Pauses on interaction, resumes after 12s.

**Bottom bar:** Fixed at bottom. Tagline + email capture. Thin colored line (category color) at top.

### Layout Problems to Fix (3/31)

João flagged these issues with the current `vertical-scroll/` prototype:

1. **Not on a grid** — Photos are pushed to the far right edge. They don't follow a consistent grid system. The layout feels arbitrary, not structured.
2. **Instructor name + credential not aligned** — The info text beside the active photo isn't positioned on the grid either. It needs to snap to a column.
3. **Too much empty space** — On larger screens there's a massive gap between left content and right photos. The layout doesn't scale well.
4. **Photos changing shape** — Queued photos were getting compressed/condensed instead of staying consistently square.

### Design Reference: Maven Expert Pages

João pointed to the expert pages as a design language foundation:
- **Live:** https://maven-expert-pages.vercel.app/experts/joao-ventura
- **GitHub:** https://github.com/acorda-joao/maven-expert-pages
- The `/experts/[name]` page "looks like Maven" — dark navy bg, grid pattern, Bureau Serif typography, lapis color palette, structured layout
- Use this as a foundation for the landing page design: same typography, colors, spacing, and grid structure

### Figma Reference

João created a Figma layout for the intended composition:
- **Figma:** https://www.figma.com/design/EpA5SZaYbLPuMc0moFKYe7/hard-launch-page?node-id=47-2233
- Shows the intended grid structure with properly aligned photos and text
- The layout should follow this Figma as the source of truth for positioning

### What Needs to Happen Next

1. **Establish a proper grid system** — the entire page should be built on a consistent column grid (similar to the expert pages). Photos, text, and spacing all snap to this grid.
2. **Fix photo alignment** — photos should be in a structured vertical column, all square, consistently sized, properly spaced.
3. **Fix name/credential alignment** — instructor info should align to a grid column, not float loosely.
4. **Better use of space** — the layout should fill the viewport proportionally on all screen sizes. No massive empty gaps.
5. **Pull design language from expert pages** — typography (Bureau Serif/Sans), colors (lapis palette), grid pattern, spacing system.
6. **Match the Figma layout** — use the Figma file as the positioning reference.

---

## Design Decisions Made

- **Photo treatment:** Natural photography on dark background. Category colors used as accents (topic text, bottom bar border), not as photo backgrounds.
- **Information display:** Featured spotlight — only the active instructor shows name + credential. Queue shows only faces.
- **Category interaction:** Auto-cycling + user-driven (scroll/click to navigate). Pauses on interaction.
- **Animation direction:** Everything moves DOWNWARD. Categories descend through the queue into the focal point. Semi-random diagonal wave for grid-flip variant.
- **Layout:** Vertical scroll ticker is the chosen direction. Two columns, bottom-aligned, with the "Learn ___ from humans" focal point at bottom-left.

---

## References & Resources

### Sites to Study
- MasterClass: masterclass.com (photography quality, face-first grid)
- Cameo: cameo.com (category-to-face navigation)
- Codrops HoverGrid: tympanus.net/Development/HoverGrid/ (hover reveal mechanics)
- GSAP Flip demo: codepen.io/GreenSock/pen/VwyGyXa (grid shuffle animation)
- Codrops Elastic Grid: tympanus.net/codrops/2025/06/03/elastic-grid-scroll (scroll parallax in grids)
- Codrops GSAP Flip Grid: tympanus.net/codrops/2026/01/20/animating-responsive-grid-layout-transitions-with-gsap-flip/
- Lapa Ninja Photography: lapa.ninja/category/photography/ (510 photography landing pages)
- Lapa Ninja Bento Grid: lapa.ninja/category/bento-grid/

### Technical Resources
- GSAP Flip Plugin: gsap.com/docs/v3/Plugins/Flip/
- CSS flip card tutorial: sliderrevolution.com/resources/css-flip-cards/
- Bento grid guide: landdding.com/blog/blog-bento-grid-design-guide

### Maven Design References
- Expert pages (live): maven-expert-pages.vercel.app/experts/joao-ventura
- Expert pages (GitHub): github.com/acorda-joao/maven-expert-pages
- Expert pages design tokens: `maven-expert-pages/src/app/globals.css` (lapis palette, fonts, grid)
- Figma layout: figma.com/design/EpA5SZaYbLPuMc0moFKYe7/hard-launch-page?node-id=47-2233

### Existing Repos
- Hard launch concepts: github.com/acorda-joao/hard-launch-concepts
- Original report: github.com/rqcai200/experts-report
- Notion doc: notion.so/mavenlearning/Learn-from-Humans-campaign-concept-32c9943bd0528086a4d5c7dee5e3c39f

### Emil Design Engineering (applied to all prototypes)
- `touch-action: manipulation` on interactive elements
- `env(safe-area-inset-bottom)` on bottom bar
- `visibilitychange` listener to pause auto-cycle when tab hidden
- `prefers-reduced-motion: reduce` disables all animations
- `@media (hover: hover) and (pointer: fine)` gates all hover effects
- Only `transform` and `opacity` animated, `will-change` applied
- 44px minimum tap targets, proper form semantics, `focus-visible` outlines
