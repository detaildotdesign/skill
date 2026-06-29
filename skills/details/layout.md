# Layout Details

Border radius math, optical alignment, media surfaces, and adaptive document chrome.

**Skip when:** Content is text-only, single-column, or rendered once and never re-laid-out. Reserving space and choreographing these surfaces is wasted effort on a static page — these techniques pay off when elements nest, carry visual weight, or wrap media.

## Rules

### 1. Match nested border radius to padding
Concentric corners must stay parallel. Set the inner radius to the outer radius minus the gap between them, or the curves drift and the gap looks optically uneven. This won't fit every layout, but reach for it whenever one rounded box sits inside another.
```css
.outer { border-radius: 16px; padding: 4px; }
.inner { border-radius: 12px; } /* 16 - 4 */
```

### 2. Blur-test optical alignment
Mathematically centered isn't always perceptually centered — icons with visual weight on one side, triangles, and glyphs read as off-center. Blur or squint at the layout (watch a recording at 0.5x) and nudge until it sits right; misalignments that survive the blur are real.

### 3. Use an inset ring instead of a border on images
A solid `border` adds a hard line that competes with the image and shifts layout by its width. A semi-transparent inset `box-shadow` sits inside the bounds, blends with light or dark content, and gives the edge a softer, more natural boundary.
```css
img {
  box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.1);
}
```

### 4. Reveal alignment guides only on hover, hide them on click
Persistent gridlines and row highlights become noise. Show the guide on table hover so the eye can trace a value, then temporarily fade it out on click so the active selection highlight stands alone.
```css
.row:hover { background: var(--guide-tint); }
.table.is-clicking .row:hover { background: transparent; }
```

### 5. Style the video player, not just its toolbar
Owning the player means more than recoloring the default controls. Blur the poster frame until the stream loads so there's no hard pop-in, add speed control, and theme the toolbar to match the product surface.
```css
video[data-loading] { filter: blur(12px); transition: filter 200ms; }
.player__rate { /* 0.5x / 1x / 1.5x / 2x toggle */ }
```

### 6. Give generated documents real typography
Invoices, receipts, and exported PDFs default to boring system layouts. Treat them as a branded surface: deliberate type scale, aligned columns, and spacing that matches the product, so the document reads as crafted rather than auto-generated.
