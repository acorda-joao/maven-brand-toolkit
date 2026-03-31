# Deep Research: Generative Brand Pattern Techniques

Research date: 2026-03-31
Context: Brand exploration toolkit for Maven Hard Launch — three generative techniques + composition strategy
Previous: [[4. Research - Editorial Design & Photography]]

---

## Table of Contents

1. [Architectural Grid with Arcs and Diagonals](#1-architectural-grid-with-arcs-and-diagonals)
2. [Vertical Line Dither/Stippling Effect](#2-vertical-line-ditherstippling-effect)
3. [Horizontal Pattern Lines](#3-horizontal-pattern-lines)
4. [Compositing All Three Layers](#4-compositing-all-three-layers)
5. [Generative Brand System References](#5-generative-brand-system-references)

---

## Foundation: Canvas 2D Setup for All Techniques

Before diving into each technique, every one of them shares the same high-DPI canvas foundation. This is non-negotiable for quality.

### High-DPI / Retina Canvas Setup

```javascript
function createHiDPICanvas(width, height, canvas) {
  const dpr = window.devicePixelRatio || 1;

  // Set display size via CSS
  canvas.style.width = width + 'px';
  canvas.style.height = height + 'px';

  // Set actual canvas buffer size, scaled by DPR
  canvas.width = width * dpr;
  canvas.height = height * dpr;

  const ctx = canvas.getContext('2d');

  // Scale all drawing operations by DPR
  // This means all coordinates stay in CSS pixels
  ctx.scale(dpr, dpr);

  return ctx;
}
```

**Why this matters:** On a 2x Retina display, a 1px line drawn without DPR scaling occupies 1 physical pixel out of 2, creating a blurry, anti-aliased mess. With DPR scaling, that same 1px logical line occupies 2 physical pixels — crisp and intentional.

**Dynamic DPR changes:** If the user drags the window between displays with different DPRs, use `window.matchMedia()` to detect changes and re-render:

```javascript
const mqString = `(resolution: ${window.devicePixelRatio}dppx)`;
const mql = window.matchMedia(mqString);
mql.addEventListener('change', () => {
  // Re-create canvas with new DPR, re-render
});
```

### Sub-pixel Positioning for Thin Lines

For architectural/blueprint aesthetics with 1px lines:

```javascript
// For ODD lineWidth values (1px, 3px), offset by 0.5 to align with pixel grid
ctx.lineWidth = 1;
// Instead of drawing at integer coords:
// ctx.moveTo(100, 100) — line straddles two pixel rows, gets anti-aliased
// Draw at half-pixel offset:
// ctx.moveTo(100.5, 100.5) — line falls exactly on one pixel row

function snapToPixel(coord, lineWidth) {
  if (lineWidth % 2 === 1) {
    return Math.round(coord) + 0.5;
  }
  return Math.round(coord);
}
```

---

## 1. Architectural Grid with Arcs and Diagonals

### Concept

A thin-line grid where some cells contain quarter-circle arcs (corner to corner) and some contain diagonal lines (corner to corner). Most cells are empty. The pattern evokes architectural drawings, blueprints, and the Le Corbusier Modulor — a proportional grid system based on human measurements and the golden ratio.

The grid serves as a metaphor for growth (stacking, building) and learning (modules as building blocks). It should feel like precise draftsmanship, not random generative noise.

### The 10 PRINT Heritage

This pattern descends from the classic 10 PRINT algorithm (Commodore 64, 1982):
```basic
10 PRINT CHR$(205.5+RND(1)); : GOTO 10
```
That one-liner randomly draws `\` or `/` diagonals in a grid, creating maze-like patterns. Our version is more sophisticated: instead of binary randomness, we use weighted probabilities across multiple tile types, creating controlled variation that feels designed rather than random.

### Tile Type Definitions

Each cell in the grid can be one of these types:

```javascript
const TILE_TYPES = {
  EMPTY: 'empty',           // Just the grid lines, no fill
  ARC_TL: 'arc_tl',         // Quarter-circle from top-left corner
  ARC_TR: 'arc_tr',         // Quarter-circle from top-right corner
  ARC_BL: 'arc_bl',         // Quarter-circle from bottom-left corner
  ARC_BR: 'arc_br',         // Quarter-circle from bottom-right corner
  DIAG_TLBR: 'diag_tlbr',   // Diagonal: top-left to bottom-right (\)
  DIAG_TRBL: 'diag_trbl',   // Diagonal: top-right to bottom-left (/)
  CROSS: 'cross',           // Both diagonals (X)
  DOUBLE_ARC: 'double_arc', // Two opposing quarter-circles (creates an S-curve)
};
```

### Drawing Each Tile Type

```javascript
function drawTile(ctx, x, y, size, type, config) {
  const { lineColor, lineWidth } = config;

  ctx.strokeStyle = lineColor;
  ctx.lineWidth = lineWidth;
  ctx.lineCap = 'round';  // Smooth line ends, critical for blueprint feel
  ctx.lineJoin = 'round';

  switch (type) {
    case 'arc_tl':
      // Quarter circle: center at top-left corner, radius = cell size
      // Sweeps from right edge (0 rad) to bottom edge (PI/2 rad)
      ctx.beginPath();
      ctx.arc(x, y, size, 0, Math.PI / 2);
      ctx.stroke();
      break;

    case 'arc_tr':
      // Center at top-right corner
      // Sweeps from bottom edge (PI/2) to left edge (PI)
      ctx.beginPath();
      ctx.arc(x + size, y, size, Math.PI / 2, Math.PI);
      ctx.stroke();
      break;

    case 'arc_bl':
      // Center at bottom-left corner
      // Sweeps from top edge (3*PI/2) to right edge (2*PI)
      ctx.beginPath();
      ctx.arc(x, y + size, size, -Math.PI / 2, 0);
      ctx.stroke();
      break;

    case 'arc_br':
      // Center at bottom-right corner
      // Sweeps from left edge (PI) to top edge (3*PI/2)
      ctx.beginPath();
      ctx.arc(x + size, y + size, size, Math.PI, 3 * Math.PI / 2);
      ctx.stroke();
      break;

    case 'diag_tlbr':
      ctx.beginPath();
      ctx.moveTo(x, y);
      ctx.lineTo(x + size, y + size);
      ctx.stroke();
      break;

    case 'diag_trbl':
      ctx.beginPath();
      ctx.moveTo(x + size, y);
      ctx.lineTo(x, y + size);
      ctx.stroke();
      break;

    case 'cross':
      ctx.beginPath();
      ctx.moveTo(x, y);
      ctx.lineTo(x + size, y + size);
      ctx.moveTo(x + size, y);
      ctx.lineTo(x, y + size);
      ctx.stroke();
      break;

    case 'double_arc':
      // Two opposing arcs — creates S-curve / Vesica Piscis feel
      ctx.beginPath();
      ctx.arc(x, y, size, 0, Math.PI / 2);
      ctx.stroke();
      ctx.beginPath();
      ctx.arc(x + size, y + size, size, Math.PI, 3 * Math.PI / 2);
      ctx.stroke();
      break;

    case 'empty':
    default:
      // Nothing drawn inside the cell
      break;
  }
}
```

### Anti-Aliased Arc Rendering

Canvas arcs are natively anti-aliased — the browser handles sub-pixel smoothing automatically. The quality concerns are:

1. **DPR scaling** (covered above) — the single biggest quality lever
2. **lineCap: 'round'** — prevents jagged line endings where arcs meet grid lines
3. **Consistent lineWidth** — arcs and grid lines must use the same weight, or the visual system breaks
4. **No integer-rounding of arc centers** — arc centers should be at exact grid intersections (integer coordinates). The arc curve itself will be anti-aliased naturally.

For extra smoothness on standard displays, you can render at 2x and scale down:

```javascript
// Supersampling approach (for non-retina displays)
const SUPERSAMPLE = 2;
canvas.width = width * SUPERSAMPLE;
canvas.height = height * SUPERSAMPLE;
canvas.style.width = width + 'px';
canvas.style.height = height + 'px';
ctx.scale(SUPERSAMPLE, SUPERSAMPLE);
```

### The Grid Generation Algorithm

```javascript
function generateGrid(config) {
  const {
    cols,              // Number of columns
    rows,              // Number of rows
    cellSize,          // Cell size in CSS pixels (e.g., 40-80)
    emptyWeight,       // Probability of empty cell (0.5-0.8 typical)
    arcWeight,         // Probability of arc (0.1-0.3)
    diagonalWeight,    // Probability of diagonal (0.05-0.15)
    crossWeight,       // Probability of cross (0.01-0.05)
    doubleArcWeight,   // Probability of double arc (0.02-0.08)
    seed,              // Optional: deterministic seed for reproducibility
    densityMap,        // Optional: 2D array of density multipliers per cell
  } = config;

  const grid = [];
  const rng = seed ? seededRandom(seed) : Math.random;

  for (let row = 0; row < rows; row++) {
    grid[row] = [];
    for (let col = 0; col < cols; col++) {
      // Apply density multiplier if provided
      const density = densityMap ? densityMap[row][col] : 1.0;

      // Adjust weights by density
      // density > 1 = more likely to have content
      // density < 1 = more likely to be empty
      const adjustedEmpty = emptyWeight / density;
      const adjustedArc = arcWeight * density;
      const adjustedDiag = diagonalWeight * density;
      const adjustedCross = crossWeight * density;
      const adjustedDouble = doubleArcWeight * density;

      const total = adjustedEmpty + adjustedArc + adjustedDiag
                     + adjustedCross + adjustedDouble;

      const r = rng() * total;
      let cumulative = 0;

      if ((cumulative += adjustedEmpty) > r) {
        grid[row][col] = 'empty';
      } else if ((cumulative += adjustedArc) > r) {
        // Randomly choose which corner the arc comes from
        const corners = ['arc_tl', 'arc_tr', 'arc_bl', 'arc_br'];
        grid[row][col] = corners[Math.floor(rng() * 4)];
      } else if ((cumulative += adjustedDiag) > r) {
        grid[row][col] = rng() > 0.5 ? 'diag_tlbr' : 'diag_trbl';
      } else if ((cumulative += adjustedCross) > r) {
        grid[row][col] = 'cross';
      } else {
        grid[row][col] = 'double_arc';
      }
    }
  }

  return grid;
}
```

### Density Maps: Active vs. Quiet Zones

The density map is the key to making it feel designed rather than random. Instead of uniform randomness, you create zones of activity:

```javascript
function createDensityMap(cols, rows, zones) {
  // Initialize all cells to base density
  const map = Array.from({ length: rows }, () =>
    Array(cols).fill(0.3) // Low base density = mostly empty
  );

  // Apply zones of higher density
  for (const zone of zones) {
    const { centerCol, centerRow, radius, intensity } = zone;
    for (let r = 0; r < rows; r++) {
      for (let c = 0; c < cols; c++) {
        const dist = Math.hypot(c - centerCol, r - centerRow);
        if (dist < radius) {
          // Smooth falloff from center
          const falloff = 1 - (dist / radius);
          const eased = falloff * falloff; // Quadratic ease
          map[r][c] += intensity * eased;
        }
      }
    }
  }

  return map;
}

// Example: Dense cluster in top-right, sparse everywhere else
const densityMap = createDensityMap(20, 15, [
  { centerCol: 15, centerRow: 3, radius: 6, intensity: 2.5 },
  { centerCol: 5, centerRow: 10, radius: 4, intensity: 1.5 },
]);
```

### Making It Feel Architectural (Not Mathematical)

Blueprint/architectural quality comes from these specific choices:

1. **Color palette:**
   - Grid lines: very low contrast against background. On dark bg (`#080c28`), use `rgba(255, 255, 255, 0.06)` for grid, `rgba(255, 255, 255, 0.15)` for arcs/diagonals
   - On light bg (`#f5f5f0`), use `rgba(0, 0, 0, 0.08)` for grid, `rgba(0, 0, 0, 0.20)` for arcs/diagonals
   - Blueprint blue: grid in `rgba(30, 81, 202, 0.1)`, arcs in `rgba(30, 81, 202, 0.25)` (using Maven lapis-500)

2. **Line weight hierarchy:**
   - Grid lines: 0.5px (sub-pixel, gets anti-aliased into a faint line)
   - Arc/diagonal strokes: 1px (crisp, reads as the "drawing" on top of the grid "paper")
   - Optional emphasis arcs: 1.5px (for focal elements)

3. **Modulor-inspired proportional cells:**
   Instead of uniform cell sizes, use the Fibonacci/golden-ratio sequence for cell dimensions:
   ```javascript
   // Modulor-inspired cell sizes (in pixels)
   const MODULOR_SIZES = [20, 32, 52, 84, 136]; // Approximate Fibonacci
   // Use a base module (e.g., 40px) and subdivide some cells
   // or use larger cells in certain zones
   ```

4. **Architectural conventions:**
   - Dimension lines: tiny tick marks at grid intersections (like measurement annotations)
   - Registration marks: small crosses at the canvas corners
   - Margin area: leave the outermost 1-2 rows/cols empty (like a drawing's border)

### Configurable Parameters Summary

| Parameter | Range | Default | Effect |
|-----------|-------|---------|--------|
| `cellSize` | 20-120px | 48px | Smaller = denser, more textile-like. Larger = more architectural |
| `lineWidth` (grid) | 0.25-1px | 0.5px | Sub-pixel creates ghost grid. 1px is crisp blueprint. |
| `lineWidth` (content) | 0.5-2px | 1px | The arcs/diagonals should be 1.5-2x the grid weight |
| `emptyWeight` | 0.4-0.9 | 0.65 | Higher = more breathing room, quieter grid |
| `arcWeight` | 0.05-0.4 | 0.20 | Arcs are the signature element — keep prominent |
| `diagonalWeight` | 0.02-0.2 | 0.10 | Diagonals add energy/movement |
| `gridColor` | — | `rgba(255,255,255,0.06)` | Nearly invisible structural layer |
| `contentColor` | — | `rgba(255,255,255,0.15)` | Visible but subtle drawing layer |
| `densityZones` | Array | `[]` | Zones of visual activity |

### Seeded Random for Reproducibility

For brand consistency, use a deterministic PRNG so the same seed always produces the same pattern:

```javascript
// Mulberry32 — fast, good distribution, 32-bit seed
function seededRandom(seed) {
  let s = seed | 0;
  return function() {
    s = (s + 0x6D2B79F5) | 0;
    let t = Math.imul(s ^ (s >>> 15), 1 | s);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}
```

### Full Render Pipeline

```javascript
function renderArchitecturalGrid(ctx, config) {
  const { cols, rows, cellSize, gridLineWidth, contentLineWidth,
          gridColor, contentColor, bgColor } = config;

  const width = cols * cellSize;
  const height = rows * cellSize;

  // 1. Fill background
  ctx.fillStyle = bgColor;
  ctx.fillRect(0, 0, width, height);

  // 2. Draw grid lines (structural layer)
  ctx.strokeStyle = gridColor;
  ctx.lineWidth = gridLineWidth;
  ctx.beginPath();
  for (let i = 0; i <= cols; i++) {
    const x = snapToPixel(i * cellSize, gridLineWidth);
    ctx.moveTo(x, 0);
    ctx.lineTo(x, height);
  }
  for (let j = 0; j <= rows; j++) {
    const y = snapToPixel(j * cellSize, gridLineWidth);
    ctx.moveTo(0, y);
    ctx.lineTo(width, y);
  }
  ctx.stroke();

  // 3. Generate tile assignments
  const grid = generateGrid(config);

  // 4. Draw tile content (arcs, diagonals)
  for (let row = 0; row < rows; row++) {
    for (let col = 0; col < cols; col++) {
      const type = grid[row][col];
      if (type !== 'empty') {
        drawTile(ctx, col * cellSize, row * cellSize, cellSize, type, {
          lineColor: contentColor,
          lineWidth: contentLineWidth,
        });
      }
    }
  }
}
```

---

## 2. Vertical Line Dither/Stippling Effect

### Concept

Convert a photograph into vertical lines of varying width, where line thickness encodes image brightness. This is a form of **amplitude-modulated (AM) line halftoning** — the frequency (spacing) stays constant while the amplitude (width) varies.

The effect on tooooools.app specifically uses **vertical lines** (not dots, not diagonal screen angles). Thicker lines = darker image areas. Thinner lines = lighter areas. The result is a portrait that's recognizable but feels computed, vector-like, machine-rendered.

### The Algorithm (Step by Step)

#### Step 1: Load Image and Extract Brightness Data

```javascript
async function loadImageBrightness(imageSrc, targetWidth, targetHeight) {
  return new Promise((resolve) => {
    const img = new Image();
    img.crossOrigin = 'anonymous';
    img.onload = () => {
      // Create offscreen canvas to sample the image
      const offscreen = document.createElement('canvas');
      offscreen.width = targetWidth;
      offscreen.height = targetHeight;
      const octx = offscreen.getContext('2d');

      // Draw image scaled to target dimensions
      octx.drawImage(img, 0, 0, targetWidth, targetHeight);

      // Extract pixel data
      const imageData = octx.getImageData(0, 0, targetWidth, targetHeight);
      const pixels = imageData.data; // [R, G, B, A, R, G, B, A, ...]

      // Convert to 2D brightness array (0 = black, 1 = white)
      const brightness = [];
      for (let y = 0; y < targetHeight; y++) {
        brightness[y] = [];
        for (let x = 0; x < targetWidth; x++) {
          const i = (y * targetWidth + x) * 4;
          const r = pixels[i];
          const g = pixels[i + 1];
          const b = pixels[i + 2];
          // Perceptual luminance (ITU-R BT.709)
          brightness[y][x] = (0.2126 * r + 0.7152 * g + 0.0722 * b) / 255;
        }
      }

      resolve(brightness);
    };
    img.src = imageSrc;
  });
}
```

**Important:** Use perceptual luminance weights (0.2126R, 0.7152G, 0.0722B), not simple average. Green contributes most to perceived brightness. Simple averaging produces incorrect tonal mapping.

#### Step 2: Column Sampling

For each vertical line in the output, sample a column of pixels from the brightness data. The sampling resolution determines the line count:

```javascript
function sampleColumns(brightness, lineSpacing, imageWidth, imageHeight) {
  const columns = [];
  const numLines = Math.floor(imageWidth / lineSpacing);

  for (let i = 0; i < numLines; i++) {
    const x = Math.round(i * lineSpacing + lineSpacing / 2);
    const column = [];

    for (let y = 0; y < imageHeight; y++) {
      // Sample brightness at this position
      // For smoother results, average a horizontal strip
      let sum = 0;
      let count = 0;
      const halfSpacing = Math.floor(lineSpacing / 2);
      for (let dx = -halfSpacing; dx <= halfSpacing; dx++) {
        const sx = Math.max(0, Math.min(imageWidth - 1, x + dx));
        sum += brightness[y][sx];
        count++;
      }
      column.push(sum / count);
    }

    columns.push(column);
  }

  return columns;
}
```

#### Step 3: Brightness-to-Width Mapping

The critical mapping function. Darker pixels = wider lines, lighter pixels = thinner lines:

```javascript
function brightnessToWidth(brightness, minWidth, maxWidth, gamma) {
  // brightness: 0 (black) to 1 (white)
  // Invert: dark pixels get wider lines
  const inverted = 1 - brightness;

  // Apply gamma correction for perceptual linearity
  // gamma < 1 = more contrast in shadows (good for portraits)
  // gamma > 1 = more contrast in highlights
  const corrected = Math.pow(inverted, gamma);

  // Map to width range
  return minWidth + corrected * (maxWidth - minWidth);
}
```

**Gamma parameter is critical.** Without gamma correction, mid-tones get compressed. A gamma of 0.7-0.9 works well for portraits — it opens up shadow detail and prevents faces from becoming solid black blobs.

#### Step 4: Render Vertical Lines

Two rendering approaches:

**Approach A: Variable-width rectangles (simpler, faster)**

```javascript
function renderVerticalLines(ctx, columns, config) {
  const { lineSpacing, outputHeight, lineColor, minWidth, maxWidth,
          gamma, verticalResolution } = config;

  ctx.fillStyle = lineColor;

  for (let i = 0; i < columns.length; i++) {
    const centerX = i * lineSpacing + lineSpacing / 2;
    const column = columns[i];

    // Draw line as a series of small rectangles with varying width
    const segmentHeight = outputHeight / column.length;

    for (let j = 0; j < column.length; j++) {
      const width = brightnessToWidth(column[j], minWidth, maxWidth, gamma);

      if (width > 0.1) { // Skip near-zero widths
        const y = j * segmentHeight;
        ctx.fillRect(
          centerX - width / 2,
          y,
          width,
          segmentHeight + 0.5 // Slight overlap prevents gaps
        );
      }
    }
  }
}
```

**Approach B: Smooth path with bezier curves (higher quality, tooooools.app style)**

This approach draws each line as a filled path where the left and right edges are smooth curves:

```javascript
function renderSmoothVerticalLines(ctx, columns, config) {
  const { lineSpacing, outputHeight, lineColor, minWidth, maxWidth,
          gamma, smoothing } = config;

  ctx.fillStyle = lineColor;

  for (let i = 0; i < columns.length; i++) {
    const centerX = i * lineSpacing + lineSpacing / 2;
    const column = columns[i];
    const segmentHeight = outputHeight / column.length;

    // Calculate widths for each vertical position
    const widths = column.map(b => brightnessToWidth(b, minWidth, maxWidth, gamma));

    // Optional: smooth the width array to prevent jitter
    const smoothedWidths = smoothArray(widths, smoothing);

    // Build path: go down the right edge, then back up the left edge
    ctx.beginPath();

    // Right edge (top to bottom)
    ctx.moveTo(centerX + smoothedWidths[0] / 2, 0);
    for (let j = 1; j < smoothedWidths.length; j++) {
      const y = j * segmentHeight;
      const hw = smoothedWidths[j] / 2;
      ctx.lineTo(centerX + hw, y);
    }

    // Left edge (bottom to top)
    for (let j = smoothedWidths.length - 1; j >= 0; j--) {
      const y = j * segmentHeight;
      const hw = smoothedWidths[j] / 2;
      ctx.lineTo(centerX - hw, y);
    }

    ctx.closePath();
    ctx.fill();
  }
}

// Gaussian-like smoothing of width values
function smoothArray(arr, passes) {
  let result = [...arr];
  for (let p = 0; p < passes; p++) {
    const smoothed = [...result];
    for (let i = 1; i < result.length - 1; i++) {
      smoothed[i] = result[i - 1] * 0.25 + result[i] * 0.5 + result[i + 1] * 0.25;
    }
    result = smoothed;
  }
  return result;
}
```

#### Step 5: Full Pipeline

```javascript
async function createDitherEffect(imageSrc, canvas, config) {
  const {
    lineSpacing,    // px between line centers (3-8px typical)
    minWidth,       // thinnest line width (0.3-1px)
    maxWidth,       // thickest line width (2-6px, up to lineSpacing)
    gamma,          // brightness curve (0.6-1.2)
    smoothing,      // width smoothing passes (2-5)
    lineColor,      // brand color for the lines
    bgColor,        // background color
    outputWidth,    // final canvas width
    outputHeight,   // final canvas height
  } = config;

  const ctx = createHiDPICanvas(outputWidth, outputHeight, canvas);

  // Calculate sampling resolution
  const sampleWidth = Math.floor(outputWidth / lineSpacing) * lineSpacing;
  const sampleHeight = outputHeight;

  // Load and sample brightness
  const brightness = await loadImageBrightness(imageSrc, sampleWidth, sampleHeight);
  const columns = sampleColumns(brightness, lineSpacing, sampleWidth, sampleHeight);

  // Render
  ctx.fillStyle = bgColor;
  ctx.fillRect(0, 0, outputWidth, outputHeight);

  renderSmoothVerticalLines(ctx, columns, {
    lineSpacing, outputHeight, lineColor, minWidth, maxWidth, gamma,
    smoothing: smoothing || 3,
  });
}
```

### Optimal Parameters for Recognizable Portraits

These ranges were determined by analyzing the tooooools.app output characteristics:

| Parameter | Range | Recommended | Notes |
|-----------|-------|-------------|-------|
| `lineSpacing` | 2-12px | 4-6px | Below 3px: too dense, reads as texture. Above 8px: faces become abstract |
| `minWidth` | 0-1.5px | 0.5px | The whitest areas. 0 = lines disappear completely (good). 0.5 = faint lines persist (more textile) |
| `maxWidth` | 2-8px | 3-5px | Should be 60-90% of lineSpacing. If maxWidth >= lineSpacing, darkest areas become solid |
| `gamma` | 0.4-1.5 | 0.75 | Lower = more contrast, darker overall. Higher = brighter, more washed. 0.75 is the sweet spot for portraits |
| `smoothing` | 0-8 | 3 | 0 = each pixel row has independent width (noisy). 3 = gentle smoothing. 8 = very smooth, loses detail |
| `verticalRes` | — | Match image height | Sample every pixel row for maximum fidelity |

### Canvas vs SVG: The Trade-off

**Canvas advantages:**
- Real-time rendering (can animate, respond to hover, etc.)
- Handles large images without DOM overhead
- Better for the exploration toolkit (tweak params, see results instantly)
- Performance: renders in one paint call

**SVG advantages:**
- Infinite resolution (vector)
- Can be exported and used in print
- Each line is an addressable DOM element (could animate individually)
- Better for final production assets

**Recommendation:** Build the exploration toolkit in Canvas for interactivity and real-time feedback. Add an "Export SVG" button that converts the computed line paths to SVG `<path>` elements for production use. The conversion is straightforward — each line's path data is the same coordinates used in the canvas version.

### Color Mapping

To render in brand colors while preserving luminance:

```javascript
// The lines ARE the color. Background is the "paper."
// Dark areas = more line coverage = more color visible
// Light areas = less line coverage = more background visible

const COLOR_SCHEMES = {
  bluePrint: { lineColor: '#1e51ca', bgColor: '#080c28' },  // Maven blue on dark
  redEnergy: { lineColor: '#A9202A', bgColor: '#faf5f0' },  // Leadership burgundy on cream
  greenGrowth: { lineColor: '#009e3d', bgColor: '#f0faf5' }, // Green on light
  limeVibrant: { lineColor: '#CDFF92', bgColor: '#080c28' }, // Lime on dark
  purpleAI: { lineColor: '#460952', bgColor: '#faf5f5' },    // AI purple on light
};
```

### Dual-Layer Effect (Dither Behind Cut-Out Portrait)

For the specific effect of a dithered pattern behind a background-removed portrait:

```javascript
async function renderDualLayer(ctx, originalPhoto, cutoutPhoto, config) {
  // 1. Create dither effect from the original photo (with background)
  const ditherCanvas = document.createElement('canvas');
  await createDitherEffect(originalPhoto, ditherCanvas, config);

  // 2. Draw the dither layer
  ctx.drawImage(ditherCanvas, 0, 0);

  // 3. Draw the cut-out portrait on top
  const cutout = new Image();
  cutout.src = cutoutPhoto;
  await new Promise(r => cutout.onload = r);

  // Position the cutout (centered, or custom placement)
  const scale = config.outputHeight / cutout.naturalHeight;
  const w = cutout.naturalWidth * scale;
  const h = cutout.naturalHeight * scale;
  const x = (config.outputWidth - w) / 2;

  ctx.drawImage(cutout, x, 0, w, h);
}
```

---

## 3. Horizontal Pattern Lines

### Concept

Horizontal stripes of varying widths and colors, creating texture and energy. These evoke broadcast signals, barcode aesthetics, and speed/progress. Used as accent elements — background textures, dividers, energy strips.

### Generation Algorithm

```javascript
function generatePatternLines(config) {
  const {
    totalHeight,       // Total height of the pattern area
    palette,           // Array of color strings from brand
    minLineHeight,     // Minimum stripe thickness (1-2px)
    maxLineHeight,     // Maximum stripe thickness (4-12px)
    gapProbability,    // Chance of a gap between stripes (0-0.4)
    minGapHeight,      // Minimum gap size (1-4px)
    maxGapHeight,      // Maximum gap size (2-16px)
    colorDistribution, // 'uniform' | 'weighted' | 'gradient'
    energyLevel,       // 0-1: controls density and thickness
    seed,
  } = config;

  const rng = seed ? seededRandom(seed) : Math.random;
  const lines = [];
  let y = 0;

  while (y < totalHeight) {
    // Determine if this is a stripe or a gap
    if (rng() < gapProbability) {
      // Gap (transparent/background)
      const gapH = minGapHeight + rng() * (maxGapHeight - minGapHeight);
      y += gapH;
      continue;
    }

    // Generate a stripe
    const energyMultiplier = 0.3 + energyLevel * 0.7;
    const lineH = minLineHeight + rng() * (maxLineHeight - minLineHeight) * energyMultiplier;

    // Pick color
    let color;
    if (colorDistribution === 'weighted') {
      // First color in palette is most likely, decreasing probability
      const weights = palette.map((_, i) => 1 / (i + 1));
      color = weightedChoice(palette, weights, rng);
    } else if (colorDistribution === 'gradient') {
      // Colors shift based on vertical position
      const progress = y / totalHeight;
      const colorIndex = Math.floor(progress * palette.length) % palette.length;
      color = palette[colorIndex];
    } else {
      // Uniform random
      color = palette[Math.floor(rng() * palette.length)];
    }

    lines.push({
      y: Math.round(y),
      height: Math.max(1, Math.round(lineH)),
      color,
    });

    y += lineH;
  }

  return lines;
}

function weightedChoice(items, weights, rng) {
  const total = weights.reduce((a, b) => a + b, 0);
  let r = rng() * total;
  for (let i = 0; i < items.length; i++) {
    r -= weights[i];
    if (r <= 0) return items[i];
  }
  return items[items.length - 1];
}
```

### Rendering

```javascript
function renderPatternLines(ctx, lines, config) {
  const { width, opacity } = config;

  ctx.globalAlpha = opacity || 1;

  for (const line of lines) {
    ctx.fillStyle = line.color;
    ctx.fillRect(0, line.y, width, line.height);
  }

  ctx.globalAlpha = 1;
}
```

### Visual Energy and Speed Perception

The relationship between stripe characteristics and perceived energy:

| Property | Low Energy (calm, ambient) | High Energy (speed, progress) |
|----------|---------------------------|-------------------------------|
| Line height range | 1-3px (uniform, subtle) | 1-12px (high variation) |
| Gap frequency | 30-40% gaps | 5-10% gaps (dense) |
| Gap size | 8-16px (lots of breathing) | 1-4px (compressed) |
| Color count | 1-2 colors | 4-6 colors |
| Color contrast | Low (analogous colors) | High (complementary, brand primaries) |
| Overall opacity | 0.15-0.3 | 0.6-1.0 |

### Color Harmony Strategies

```javascript
// Strategy 1: Brand primary with tints/shades
const HARMONY_TINTED = [
  '#1e51ca',           // lapis-500 (primary)
  'rgba(30, 81, 202, 0.6)',  // 60% opacity
  'rgba(30, 81, 202, 0.3)',  // 30% opacity
  '#2465e8',           // lapis-400
  '#b2d3fa',           // lapis-100
];

// Strategy 2: Category color palette
const HARMONY_CATEGORY = [
  '#1e51ca',  // Blue (primary)
  '#460952',  // AI purple
  '#009e3d',  // Green
  '#B33600',  // Product orange
  '#A9202A',  // Leadership burgundy
];

// Strategy 3: Duotone (two colors + their mid-blend)
const HARMONY_DUOTONE = [
  '#1e51ca',  // Blue
  '#CDFF92',  // Lime
  '#97B85E',  // Midpoint blend (approximate)
];

// Strategy 4: Signal/broadcast (high contrast, bold)
const HARMONY_BROADCAST = [
  '#1e51ca',  // Blue
  '#ffffff',  // White
  '#080c28',  // Dark
  '#CDFF92',  // Lime (accent flash)
];
```

### Stripe Pattern Variations

```javascript
// Barcode style: mostly thin, occasional thick
const BARCODE = {
  minLineHeight: 1,
  maxLineHeight: 8,
  gapProbability: 0.15,
  minGapHeight: 1,
  maxGapHeight: 3,
  energyLevel: 0.8,
};

// Broadcast signal: regular rhythm, varying colors
const BROADCAST = {
  minLineHeight: 2,
  maxLineHeight: 4,
  gapProbability: 0.05,
  minGapHeight: 1,
  maxGapHeight: 2,
  energyLevel: 0.9,
};

// Ambient texture: subtle, lots of space
const AMBIENT = {
  minLineHeight: 1,
  maxLineHeight: 3,
  gapProbability: 0.4,
  minGapHeight: 4,
  maxGapHeight: 16,
  energyLevel: 0.3,
};

// Speed/progress: dense, aggressive
const SPEED = {
  minLineHeight: 1,
  maxLineHeight: 12,
  gapProbability: 0.08,
  minGapHeight: 1,
  maxGapHeight: 3,
  energyLevel: 1.0,
};
```

### Width Variations (Not Full-Width)

For accent strips that don't span the full canvas:

```javascript
function renderAccentStrips(ctx, lines, config) {
  const { canvasWidth, stripWidth, alignment, offset } = config;

  let x;
  if (alignment === 'left') x = offset || 0;
  else if (alignment === 'right') x = canvasWidth - stripWidth - (offset || 0);
  else x = (canvasWidth - stripWidth) / 2;

  for (const line of lines) {
    ctx.fillStyle = line.color;
    ctx.fillRect(x, line.y, stripWidth, line.height);
  }
}
```

---

## 4. Compositing All Three Layers

### Layer Architecture

From bottom to top:

```
Layer 0: Background color (solid)
Layer 1: Architectural grid (structural, lowest opacity)
Layer 2: Dither pattern (focal area, medium-high opacity)
Layer 3: Pattern lines (accent, positioned at edges/borders)
Layer 4: Photography (cut-out portraits, full opacity)
Layer 5: Typography and UI (full opacity)
```

### Multi-Canvas Compositing

For maximum control, render each layer on its own canvas and composite them:

```javascript
function composeLayers(outputCanvas, layers) {
  const ctx = outputCanvas.getContext('2d');
  const { width, height } = outputCanvas;

  for (const layer of layers) {
    ctx.globalAlpha = layer.opacity || 1;
    ctx.globalCompositeOperation = layer.blendMode || 'source-over';

    if (layer.mask) {
      // Apply mask (e.g., show dither only within certain grid cells)
      applyMask(ctx, layer.canvas, layer.mask, width, height);
    } else {
      ctx.drawImage(layer.canvas, 0, 0, width, height);
    }
  }

  ctx.globalAlpha = 1;
  ctx.globalCompositeOperation = 'source-over';
}
```

### Masking: Dither Within Grid Cells

To show the dither pattern only within specific grid cells (creating a focal composition):

```javascript
function applyMask(ctx, sourceCanvas, maskCells, cellSize) {
  // Save current state
  ctx.save();

  // Create clipping path from mask cells
  ctx.beginPath();
  for (const cell of maskCells) {
    ctx.rect(cell.col * cellSize, cell.row * cellSize, cellSize, cellSize);
  }
  ctx.clip();

  // Draw the source (dither) through the clip mask
  ctx.drawImage(sourceCanvas, 0, 0);

  // Restore (removes clip)
  ctx.restore();
}
```

### Blend Modes for Layering

Key `globalCompositeOperation` values for this system:

| Blend Mode | Use Case |
|------------|----------|
| `source-over` | Default. Layer on top. Good for photography layer. |
| `multiply` | Grid over background: darkens without washing out. Good for grid on light bg. |
| `screen` | Grid over dark background: lightens. Good for white/light grid on dark bg. |
| `overlay` | Dither over background: increases contrast. Creates punchy effect. |
| `soft-light` | Subtle texture overlay. Good for ambient pattern lines. |

### Composition Strategies

**Strategy 1: Grid as Canvas, Dither as Subject**
- Full-viewport architectural grid at very low opacity
- A cluster of 4-6 grid cells in the center have the dither portrait
- Pattern lines run along the left edge as a vertical accent strip
- The instructor's cut-out photo overlaps the dither, slightly offset

**Strategy 2: Horizontal Zones**
- Top zone: Pattern lines (energy/speed)
- Middle zone: Dither portrait, full width
- Bottom zone: Architectural grid fading out
- Cut-out portrait centered in the middle zone

**Strategy 3: Asymmetric Split**
- Left 40%: Dense architectural grid with arcs
- Right 60%: Dither portrait
- Pattern lines as a thin vertical divider between zones
- Typography floats in the grid zone

### Performance Considerations

```javascript
// Render once, cache the result
const gridLayer = document.createElement('canvas');
const ditherLayer = document.createElement('canvas');
const linesLayer = document.createElement('canvas');

// Only re-render layers that change
// Grid: re-render on resize or config change
// Dither: re-render on image change or config change
// Lines: re-render on config change

// For animation (e.g., grid cells slowly filling in):
// Use requestAnimationFrame, but only redraw the changing layer
function animate(timestamp) {
  // Clear output
  outputCtx.clearRect(0, 0, width, height);

  // Draw cached layers
  outputCtx.drawImage(gridLayer, 0, 0);
  outputCtx.drawImage(ditherLayer, 0, 0);
  outputCtx.drawImage(linesLayer, 0, 0);

  // Draw animated elements on top
  drawAnimatedElements(outputCtx, timestamp);

  requestAnimationFrame(animate);
}
```

---

## 5. Generative Brand System References

### Case Studies: How Others Did It

#### Sagmeister & Walsh — Casa da Musica (2007)
**Approach:** The building's physical silhouette, viewed from 17 angles, creates 17 faceted shapes. A Processing app uses these 17 facets as color-sampling regions over any input image. The sampled colors fill the facets, producing infinite logo variations that are all recognizably "Casa da Musica."

**Technical takeaway:** Their generator maps a fixed geometric structure (the building shape) to variable input (any image's color palette). The structure is the brand; the color is the context. **For Maven: the grid system is the structure. Brand colors, density patterns, and content fill are the variables.**

#### Sagmeister & Walsh — Fugue (2015)
**Approach:** A custom web app allows Fugue's team to upload SVG line drawings and transform them into the brand's calligraphic visual style. The algorithm applies the brand's stroke characteristics (weight, curvature, tapering) to any input path.

**Technical takeaway:** The brand provides a transformation function, not a static asset. Input goes in, branded output comes out. **For Maven: the dither function IS the brand treatment. Any portrait goes in, Maven-branded dither comes out.**

#### Sagmeister & Walsh — Jewish Museum NYC
**Approach:** An identity system built on "sacred geometry" — all patterns, icons, and layouts derive from a single geometric grid. A Processing app turns any photo or webcam input into an illustration in the museum's geometric style.

**Technical takeaway:** Everything derives from one grid. The grid IS the identity. Different applications (poster, signage, web, merch) all share the same underlying proportional system. **Directly relevant to the architectural grid concept.**

#### Pentagram — MIT Media Lab (2011, updated 2014)
**Approach:** A 7x7 grid generates over 40,000 unique logo variations. Each of the 23 research groups gets a unique glyph derived from the same grid algorithm. The system uses 3 spotlights with different colors that "project" through grid openings, creating overlapping color shapes.

**Technical takeaway:** Algorithmic variation from fixed constraints. The 7x7 grid is the constraint; the specific cells activated and spotlight positions are the variables. The output space is enormous (40,000+) but every variation is unmistakably MIT Media Lab. **For Maven: the architectural grid parameters (which cells get arcs, which get diagonals, density zones) create enormous variation space while maintaining brand coherence.**

#### Spotify Wrapped (2023)
**Approach:** "No grid, no rules" — but that's misleading. They actually use a layered composition system with specific visual elements (bold shapes, photography, typography, data visualizations) that can be mixed and matched. The system allows dialing up/down individual elements to control energy.

**Technical takeaway:** The power isn't in one element but in the composition grammar — how elements layer, what takes precedence, how density creates different moods. **For Maven: the three techniques (grid, dither, lines) need a composition grammar that defines how they combine at different energy levels.**

#### Components.ai (Adam Morse / Brent Jackson)
**Approach:** A tool for exploring generative design systems by cycling through parametric variations of typography, color, spacing, and layouts. Parameters are exposed as sliders.

**Technical takeaway:** The exploration interface matters as much as the output. Exposing parameters as controls lets designers discover unexpected combinations. **For Maven: the brand toolkit should expose all parameters with real-time preview — this is how the team discovers the specific configurations that work.**

### The Maven Brand System Architecture

Based on all of this, the Maven generative brand system has:

**1. Fixed structure (the brand):**
- Architectural grid proportions and line weights
- Vertical line dither algorithm (AM halftone)
- Horizontal pattern line generation
- Brand color palette
- Typography (Bureau Serif + Bureau Sans)

**2. Variable inputs (the content):**
- Which instructor's photo gets dithered
- Which category colors are active
- Grid density zones (where to be active vs. quiet)
- Energy level (calm/ambient vs. bold/intense)
- Composition layout (asymmetric, centered, zoned)

**3. Composition grammar (how they combine):**
- Grid is always the base layer
- Dither is always the focal element
- Pattern lines are always accent/energy
- Photography is always on top
- Typography has its own space (never fighting with texture)

**4. Output space:**
- Each instructor x each category x each energy level x each layout = hundreds of unique, on-brand compositions
- All generated from the same algorithm, all recognizably Maven

---

## Implementation Roadmap

### Phase 1: Individual Techniques (build each in isolation)
1. Architectural grid renderer with configurable params
2. Vertical line dither with image input
3. Horizontal pattern line generator
4. Build a parameter control panel (sliders, dropdowns) for real-time tuning

### Phase 2: Composition Engine
5. Multi-canvas layer compositor
6. Masking system (dither within grid cells)
7. Blend mode controls
8. Layout presets (the 3 composition strategies)

### Phase 3: Brand Toolkit
9. Instructor photo library integration
10. Category color preset system
11. SVG export for production assets
12. Save/load configurations (so the team can share good combos)
13. Batch generation (produce assets for all instructors automatically)

### File Structure

```
brand-toolkit/
├── index.html              # Single-page app
├── styles.css              # Minimal layout styles
├── js/
│   ├── canvas-utils.js     # HiDPI setup, snapToPixel, seededRandom
│   ├── grid.js             # Architectural grid generator + renderer
│   ├── dither.js           # Vertical line dither processor
│   ├── lines.js            # Horizontal pattern line generator
│   ├── compositor.js       # Layer compositing engine
│   ├── controls.js         # Parameter UI (sliders, presets)
│   ├── export.js           # SVG/PNG export
│   └── app.js              # Main orchestrator
└── images/                 # Instructor photos
```

---

## Sources

### Canvas 2D Techniques
- [MDN: Canvas arc() method](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/arc)
- [MDN: Pixel manipulation with canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas)
- [MDN: globalCompositeOperation](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation)
- [MDN: Optimizing canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)
- [web.dev: High DPI Canvas](https://web.dev/articles/canvas-hidipi)
- [Canvas HiDPI Retina Fix (GitHub Gist)](https://gist.github.com/callumlocke/cc258a193839691f60dd)
- [Quarter Circle Arc in Canvas](https://joshuatz.com/posts/2021/canvas-filled-quarter-partial-circles-with-arc/)
- [Improving Anti-Aliasing for HTML5 Canvas Arcs](https://copyprogramming.com/howto/how-can-i-make-my-html5-canvas-arc-less-pixellated-or-more-anti-aliased)

### Halftone & Dithering
- [Wikipedia: Halftone](https://en.wikipedia.org/wiki/Halftone)
- [Wikipedia: Stochastic screening](https://en.wikipedia.org/wiki/Stochastic_screening)
- [Processing.py Halftones Tutorial](https://tabreturn.github.io/code/processing/python/2019/02/09/processing.py_in_ten_lessons-6.3-_halftones.html)
- [halftone-js (GitHub)](https://github.com/M-A-19/halftone-js/)
- [tooooools.app Stippling Effect](https://www.tooooools.app/effects/stipping)
- [Halftone Printing in JavaScript](http://anderoonies.github.io/projects/halftone/)
- [Creating Halftone Dot Art with JavaScript (Medium)](https://medium.com/@banyapon/creating-halftone-dot-art-with-javascript-a-step-by-step-guide-to-grayscale-and-accent-highlights-1e57e946e05e)
- [Postscript Halftones (Grymoire)](https://www.grymoire.com/Postscript/Halftones.html)

### Generative Art & Patterns
- [10PRINT Generative Art Variations](https://stepanzh.github.io/10PRINT/)
- [Generative Art with JavaScript and Canvas (DEV)](https://dev.to/shayo_victor_c02f1777210e/generative-art-with-javascript-and-canvas-a-beginners-playground-2cb8)
- [Canvas, JavaScript, and Generative Art (wheatblog)](https://wheatblog.com/2025/01/canvas-javascript-and-generative-art/)
- [Eloquent JavaScript: Drawing on Canvas](https://eloquentjavascript.net/17_canvas.html)

### Architectural Grid & Modulor
- [Le Modulor: Le Corbusier's Human-Scale Blueprint](https://pooltableportfolio.com/blogs/magazine/le-modulor-le-corbusier-s-human-scale-blueprint)
- [The Modular: An Analysis into Generative Architecture](https://www.generativeart.com/on/cic/papers2005/30.MahnazShah.htm)
- [Le Corbusier's Modulor Parametric Approach (ResearchGate)](https://www.researchgate.net/publication/362678996_Le_Corbusier's_Modulor_and_'le_jeu_des_panneaux'_A_Parametric_Approach)

### Generative Brand Systems
- [Pentagram: MIT Media Lab Identity](https://www.pentagram.com/work/mit-media-lab)
- [Dezeen: Pentagram rebrands MIT Media Lab](https://www.dezeen.com/2014/10/29/pentagram-mit-media-lab-rebrand-visual-identity/)
- [Sagmeister & Walsh: Jewish Museum Identity](https://andwalsh.com/work/all/jewish-museum-identity/)
- [Sagmeister: Casa da Musica (MoMA)](https://www.moma.org/collection/works/145254)
- [Sagmeister & Walsh: Fugue Identity (Fast Company)](https://www.fastcompany.com/3043536/sagmeister-walshs-new-identity-for-a-software-brand-makes-security-tech-sexy)
- [Spotify Wrapped 2023 Design (It's Nice That)](https://www.itsnicethat.com/features/spotify-wrapped-campaign-identity-2023-graphic-design-301123)
- [Spotify Design: Reimagining Design Systems](https://spotify.design/article/reimagining-design-systems-at-spotify)
- [Components.ai: Generative Design Systems](https://components.ai/)
- [Dynamic Branding Examples (Tom Foster)](https://tomfosterctp.wordpress.com/2016/04/13/examples-of-dynamic-branding/)
