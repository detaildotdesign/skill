# Button & Control Details

Hit areas, cursor semantics, click guards, and keyboard shortcuts for buttons and interactive controls.

**Skip when:** The element isn't actually interactive (static labels, decorative chips) — hover cursors and shortcuts there mislead. Single-key shortcuts and contextual cursors only earn their keep in dense, frequently-used tools; on a marketing page or settings form they're noise.

## Rules

### 1. Make the hit area larger than the visual element
Per Fitts's Law, acquiring a target is faster the bigger it is — so let the button look small and refined while feeling generous and forgiving. Pad the hit area beyond the glyph rather than bloating the glyph itself. Rule of thumb: extend the hit area for any visual smaller than 44px on mobile, 24px on desktop.
```css
.icon-button {
  width: 24px;
  height: 24px;
  padding: 10px; /* hit area grows, glyph stays 24px */
}
```

### 2. Reserve the pointer cursor for navigation
Default to the arrow cursor everywhere, then opt into `pointer` only where a click takes the user to a new page. The cursor then reliably predicts whether a click navigates or just acts in place.
```css
a { cursor: pointer; }       /* goes somewhere */
button { cursor: default; }  /* acts in place */
```

### 3. Use a contextual cursor over special targets
Swap to a context-specific cursor when hovering an element with a distinct interaction model — e.g. Linear shows a help cursor over an author/people section so the cursor itself signals "more info here, not a link."
```css
.author-section:hover { cursor: help; }
.user-avatar:hover { cursor: url("/cursors/avatar.svg") 4 4, pointer; }
```

### 4. Guard against duplicate clicks
Disable the trigger the instant it fires so a rapid double-click or impatient repeat-tap can't submit a second order, message, or API call. Re-enable only after the request resolves.
```js
button.addEventListener("click", async () => {
  button.disabled = true;
  try { await submit(); } finally { button.disabled = false; }
});
```

### 5. Shake a disabled button when it's clicked
A disabled button that simply ignores clicks feels broken. A quick horizontal shake acknowledges the attempt and reads as "not yet" — telling the user the control exists but isn't ready.
```css
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-4px); }
  75% { transform: translateX(4px); }
}
.btn.is-disabled.clicked { animation: shake 0.2s ease; }
```

### 6. Offer single-key shortcuts in power-user tools
In non-compositional, high-frequency views (a dashboard, not a doc editor), bind bare keys without Cmd/Ctrl/Alt — press `F` and the find menu opens instantly. Removing the modifier shaves a micro-decision off every action and rewards mastery. Skip the shortcut when focus is in a text field, where the keypress should compose text.
```js
document.addEventListener("keydown", (e) => {
  if (e.target.matches("input, textarea, [contenteditable]")) return;
  if (e.key === "f") openFind();
});
```
