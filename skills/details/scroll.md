# Scroll Details

Scroll behavior, anchoring, overscroll feedback, navigation shortcuts, scrollbars, and edge fades for long or nested content.

**Skip when:** Content fits within a viewport without scrolling, or the surface is a short static page — anchored highlights, overscroll tuning, and scroll-landmark buttons add machinery nobody will exercise. Don't intercept native scroll/swipe on simple pages where the browser default already behaves correctly.

## Rules

### 1. Anchor list scrolling in three phases
In a long list with arrow-key navigation, first move only the highlight; once it reaches ~70% down the viewport, lock it and scroll the list beneath it; release the lock at the top and bottom so the user lands on the final item. A fixed visual anchor keeps the eye still while the data moves. Keep any input field anchored too — center the whole panel and its position will drift as the list height changes. Belongs in every CMDK-style menu.

### 2. Prevent accidental swipe-back
Set `overscroll-behavior-x: none` on any horizontally scrollable UI. Without it, a horizontal swipe past the edge triggers the browser's built-in back gesture and destroys page state.
```css
.carousel {
  overflow-x: auto;
  overscroll-behavior-x: none;
}
```

### 3. Style the scrollbar to match the surface
A default OS scrollbar reads as chrome bolted onto your UI. A thin, low-contrast scrollbar recedes until needed and keeps the design coherent.
```css
.scroller {
  scrollbar-width: thin;
  scrollbar-color: gray transparent;
}
```

### 4. Don't let fade edges cover the scrollbar
A fade-out mask on a scroll container must not overlap the scrollbar track. Inset the mask or apply it to the content layer only — obscuring the scrollbar removes a load-bearing navigation affordance.
```css
.list {
  /* mask the content, leave a gutter for the scrollbar */
  padding-right: 16px;
  mask: linear-gradient(to bottom, transparent, black 24px);
}
```

### 5. Keep the active view's entry visible
When a detail view opens from a list or sidebar, keep its originating row visible and highlighted. The persistent link between entry point and content prevents spatial disorientation when the user navigates back.

### 6. Enable overscroll in nested scrollers
Let inner scrollable containers rubber-band at their boundaries so an edge reads as "end of this region," not a dead surface — now supported across Chrome, WebKit, and Firefox. Use `overscroll-behavior: contain` to give the inner scroller its own bounce without leaking scroll to the parent.
```css
.panel {
  overflow-y: auto;
  overscroll-behavior: contain;
}
```

### 7. Blur the scroll edge instead of fading it
When content scrolls beneath a sticky header, a flat white-to-transparent gradient washes out anything colored — photos, gradients, saturated UI bleed through looking faded, not soft. Stack several `backdrop-filter: blur()` layers, each masked to a horizontal band with blur strength increasing toward the edge, so detail dissolves out of focus while color stays intact and header text stays readable.
```css
.edge-blur {
  position: absolute;
  inset: 0 0 auto 0;
  height: 80px;
  pointer-events: none;
}
.edge-blur > div {
  position: absolute;
  inset: 0;
  -webkit-backdrop-filter: blur(16px);
  backdrop-filter: blur(16px);
  -webkit-mask: linear-gradient(to bottom, black 25%, transparent 50%);
  mask: linear-gradient(to bottom, black 25%, transparent 50%);
}
```

### 8. Provide a scroll landmark back to the top
Long pages need a discoverable, persistent way back to the top that also remembers where the user was. Platform shortcuts (iOS status-bar tap, Home key) are device-specific, undiscoverable, and can't return to the original position.
```jsx
<button onClick={() => window.scrollTo({ top: 0, behavior: 'smooth' })}>
  ↑ Top
</button>
```

### 9. Highlight the ToC by visibility, not last anchor
Activate table-of-contents items based on which sections are currently in the viewport, not just the last anchor crossed. Multiple items can be active at once, turning the ToC into a true minimap; transition the highlight bar smoothly rather than letting it stagger between items.
```js
const io = new IntersectionObserver((entries) => {
  for (const e of entries) {
    tocLink(e.target.id).classList.toggle('active', e.isIntersecting);
  }
}, { rootMargin: '0px 0px -60% 0px' });
sections.forEach((s) => io.observe(s));
```
