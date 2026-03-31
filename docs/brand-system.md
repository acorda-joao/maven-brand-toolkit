# Maven Brand System

Documented: 2026-03-31
Source: Maven Brand Guidelines (Sep 2025) + marketing assets analysis

---

## Core Brand Elements

Maven's visual identity is built on three generative/parametric elements that layer together. They're not static assets — they're *systems* that can produce infinite on-brand variations.

### 1. The Architectural Grid

**What it is:** A thin-line grid inspired by architectural drawings. Not a simple matrix of 90-degree lines — its character comes from the selective use of diagonals and quarter-circle arcs that break up uniformity.

**Key characteristics:**
- Thin lines (~1px) in a single color, lighter than the background
- Grid cells are square, consistent size
- Some cells contain **quarter-circle arcs** (drawn from one corner, radius = cell size)
- Some cells contain **diagonal lines** (corner to corner)
- Most cells are **empty** (just the grid lines)
- The distribution of arcs and diagonals is intentional — creating areas of visual activity and quiet
- On dark backgrounds: lines in `#101b57` (lapis-800) or lighter
- On light backgrounds: lines in `#080c28` (lapis-900) at low opacity
- On colored backgrounds (lime, purple, green): lines in the same hue, darker

**Metaphor:**
- Growth → stacking and building, incremental personal development
- Learning → each module/block represents courses as building blocks of knowledge

**Usage in marketing assets:**
- Full-bleed backgrounds behind headlines and content
- Combined with category colors (each category gets its own colored version)
- The grid cells serve as structural containers for content placement
- The pillar logo mark sits within and relates to the grid

### 2. Image Vector Dither (Stippling)

**What it is:** A vector dithering technique that renders images (portraits, statues, pillars, objects) as dense vertical lines of varying width. Created using tooooools.app/effects/stipping.

**Key characteristics:**
- **Vertical lines only** — not dots, not halftone circles
- Lines are evenly spaced but vary in **width** based on the source image brightness
- Darker areas = thicker lines (more ink/color), lighter areas = thinner lines (more background showing)
- The result is recognizable portraits that feel "computed" or "rendered through a system"
- Applied on brand-colored backgrounds:
  - Blue (lapis) background + blue/white lines
  - Red background + red/white lines
  - Green/lime background + green/dark lines
  - Light/cream background + dark lines

**Usage:**
- Standalone graphic images of pillars, statues, technology, people
- **Behind background-removed portrait photos** to frame and add dynamic texture
- Within grid cells as focal elements
- The vertical line pattern connects to Maven's pillar logo mark (which is also vertical lines)

**The pillar connection:** Maven's logo is a stylized pillar made of vertical lines. The dither effect extends this motif — the entire brand visual language becomes "vertical lines at varying densities." This is a powerful systemic connection.

### 3. Pattern Lines

**What it is:** Horizontal stripes of varying widths and colors, used as texture and energy elements.

**Key characteristics:**
- Horizontal lines ranging from 1px to ~8px in thickness
- Multiple colors from the brand palette mixed together (blues, purples, magentas, oranges)
- The pattern evokes **speed, progress, and energy**
- Can be dense (broadcast/barcode feel) or sparse (subtle accent)

**Usage:**
- Background textures on marketing cards
- Divider strips between content sections
- Accent elements at the edges of compositions
- Behind text content for added visual richness
- Already live on maven.com as a subtle strip below category navigation

---

## Typography

**STK Bureau Serif** — Headlines, editorial display
- Weights: Light (300), Book (400)
- The hero headline weight is actually **200** (extra-light) — unusually thin, creates editorial confidence
- Negative letter-spacing: approximately **-4% of font-size** (72px → -2.88px, 56px → -2.24px, etc.)
- This tight tracking + thin weight = the magazine-quality feel that defines Maven

**STK Bureau Sans** — Body, UI, labels
- Weights: Book (400), Medium (500), SemiBold (600)
- Category labels use **positive** letter-spacing (+0.56px) — the only element with wide tracking

---

## Color System

### Primary (Lapis / Navy)
| Token | Hex | Usage |
|-------|-----|-------|
| lapis-900 | `#080c28` | Hero backgrounds, page canvas, the signature dark |
| lapis-800 | `#101b57` | Grid lines, subtle patterns |
| lapis-500 | `#1e51ca` | Primary blue |
| lapis-400 | `#2465e8` | Interactive blue |
| lapis-100 | `#b2d3fa` | Photo backgrounds (expert pages), section labels on dark |
| lapis-25 | `#f7fbff` | Primary text on dark (not pure white — blue-tinted) |

### Accent
| Token | Hex | Usage |
|-------|-----|-------|
| brand-light | `#7cbeff` | CTA button on dark backgrounds |
| brand-highlight | `#cdff92` | Neon lime green highlight accent (sparingly) |
| primary green | `#009e3d` | Primary CTA button |

### Category Colors
| Category | Hex | Notes |
|----------|-----|-------|
| AI | `#460952` / `#931faa` | Deep purple |
| Product | `#B33600` | Burnt orange |
| Engineering | `#00746B` | Teal |
| Design | `#772CBB` | Purple |
| Marketing | `#943170` | Mauve |
| Leadership | `#A9202A` | Burgundy |
| Founders | `#077202` | Forest green |

### Key Insight
Maven's dark mode isn't black — it's `#080c28`, a very deep navy. Their "white" isn't `#ffffff` but `#f7fbff`, a barely-there blue-tinted white. This blue undertone runs through everything.

---

## Brand Personality

**Editorial + Institutional + Human**

The tension that defines Maven: premium/institutional (like NYT or a university) crossed with accessible/approachable (real people, conversational tone, "Learn from humans"). The design system resolves this by making the *container* premium (dark backgrounds, editorial type, architectural grid) while making the *content* human (real photos, real names, real credentials).

---

## How Elements Combine

Looking at the marketing assets, the layering order is:

1. **Background color** (category color or lapis-900)
2. **Architectural grid** (thin lines, same-hue, low contrast)
3. **Dither pattern** (in a focal area or behind a portrait)
4. **Pattern lines** (as accent — often at edges or as dividers)
5. **Photography** (cut-out portraits placed on top)
6. **Typography** (Bureau Serif headlines, Bureau Sans body)
7. **UI elements** (CTAs, badges, logo)

The grid provides structure. The dither provides texture and the "digital-to-human" metaphor. Pattern lines add energy. Photography grounds it in real humans. Typography gives it editorial authority.

---

## References

- Brand Guidelines: Sep 2025 edition (pages 5.1–5.6 referenced)
- Stippling tool: https://www.tooooools.app/effects/stipping
- Marketing assets: instructor promotion cards, Lightning Lessons hero, social cards
- Live implementations: maven.com, web-git-teach-page-v1.mavy.dev/teach
