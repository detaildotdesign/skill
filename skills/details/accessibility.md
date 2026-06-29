# Accessibility Details

Focus management, keyboard operability, screen reader semantics, find-in-page, robust links, and inclusive input — the details that make an interface usable by keyboard, assistive tech, and motor-impaired users.

**Skip when:** Rarely skippable — these are baseline obligations. But don't reach for ARIA where native HTML already conveys the role and state: a `<button>` already announces itself, `<details>`/`<summary>` already handles disclosure, and adding `role="button"` to a `<div>` is a regression, not an upgrade.

## Rules

### 1. Keep a visible focus ring on every focusable element
The focus ring is the only landmark keyboard users have to know which element Enter will activate. `outline: none` with no replacement strands them. Leave the default as a floor; you can still customize color, offset, and radius for a focus ring that fits the design without trading away affordance.
```css
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
  border-radius: 4px;
}
```

### 2. Reduce animation for frequent features
Tools invoked dozens of times a day should prioritize speed over spectacle — delight fades with repetition, and motion can trigger vestibular discomfort. Honor `prefers-reduced-motion` and trim transitions on high-frequency actions.
```css
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}
```

### 3. Describe what a link does, not just "here"
A screen reader can read links out of context, in isolation from surrounding prose, so "Click here" tells the user nothing. State the destination and any side effect — opens a new tab, downloads a file — so the link makes sense on its own.
```html
<a href="/report.pdf" download>
  Annual report <span class="sr-only">(PDF, opens download)</span>
</a>
<a href="https://example.com" target="_blank" rel="noopener">
  Docs <span class="sr-only">(opens in new tab)</span>
</a>
```

### 4. Make email addresses both clickable and copyable
A displayed email address has two jobs: launch the user's mail client, and let them grab the raw address. Wire the text as a `mailto:` link and pair it with a copy-to-clipboard icon, so neither workflow forces the other.
```html
<span class="email">
  <a href="mailto:hi@example.com">hi@example.com</a>
  <button aria-label="Copy email address"
          onclick="navigator.clipboard.writeText('hi@example.com')">⧉</button>
</span>
```

### 5. Make collapsed content findable with Cmd+F
Sections hidden with `display: none` or plain `hidden` are invisible to browser find-in-page, forcing users to expand everything to search. `hidden="until-found"` keeps content visually collapsed but searchable — on a match the browser reveals the section, fires `beforematch` (hook it for animation/state), scrolls in, and highlights. Degrades gracefully where unsupported.
```html
<details>
  <summary>Section Title</summary>
  <div hidden="until-found">
    Content that can be found with Cmd+F
  </div>
</details>
```

### 6. Bind global shortcuts to the right window
Keyboard shortcuts attached to `window` silently break when the component is rendered in an iframe, pop-out window, or React portal — the listener lives on the parent window, not the one the UI is in. Resolve the owning window from a mounted node so the shortcut works in any context.
```jsx
useEffect(() => {
  const win = ref.current?.ownerDocument.defaultView || window;
  const onKey = (e) => {
    if (e.metaKey && e.key === "d") { e.preventDefault(); toggleTheme(); }
  };
  win.addEventListener("keydown", onKey);
  return () => win.removeEventListener("keydown", onKey);
}, []);
```

### 7. Minimum 44px touch targets
Hit areas must be at least 44px on mobile and 24px on desktop (WCAG 2.5.5/2.5.8). Extend the target beyond the visible glyph with padding or a pseudo-element rather than enlarging the icon itself.
```css
.icon-btn { position: relative; }
.icon-btn::after {
  content: "";
  position: absolute;
  inset: -12px; /* pushes a 20px icon to a 44px target */
}
```

### 8. Preserve meaningful parts in truncation
Assistive tech reads the rendered string, so truncation must keep the load-bearing segments. For file paths, clip the middle and keep the start and filename rather than cutting off the end.
```css
.path { white-space: nowrap; overflow: hidden; text-overflow: ellipsis; direction: rtl; text-align: left; }
```

### 9. Offer single-key shortcuts as alternatives
In specialized tools, offer single-key shortcuts alongside modifier combos. Chording is physically costly for users with motor limitations; a bare key cuts the effort. Scope them so they don't fire while typing in inputs.
```js
document.addEventListener("keydown", (e) => {
  if (e.target.matches("input, textarea, [contenteditable]")) return;
  if (e.key === "/") openSearch();
});
```

### 10. Accept natural language as an accessible input path
Calendar pickers and complex widgets are hard to operate with a screen reader or keyboard alone. Accept plain text ("next friday", "in 3 days") as a parallel input path so the feature stays reachable without the widget.
```html
<input type="text" placeholder="e.g. next friday" aria-label="Due date — type a date or phrase">
```
