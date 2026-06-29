# Form Details

Crafting form inputs, keyboards, labels, and data-entry interactions so typing, focusing, and submitting feel effortless.

**Skip when:** The "form" is a single search box or one-shot field — auto-conversion, char counters, and morph animations add latency and surprise where a plain input wins. Don't intercept keystrokes on inputs that aren't text-composition heavy.

## Rules

### 1. Link every label to its input
Wire `<label for="id">` to the input's `id` so clicking the label focuses the control. HTML does this natively — don't break it. It enlarges the hit target and is required for screen-reader association.
```html
<label for="email">Email</label>
<input id="email" type="email" />
```

### 2. Make every interaction keyboard-reachable
Every control must be focusable and operable without a pointer: Tab reaches it, Enter/Space activates it, Esc dismisses. Verify by unplugging the mouse and completing the whole form. Custom widgets need `tabindex` and key handlers.
```jsx
<div role="button" tabIndex={0}
  onClick={submit}
  onKeyDown={(e) => (e.key === "Enter" || e.key === " ") && submit()}
/>
```

### 3. Respect CJK composition state
Track `compositionstart`/`compositionend` and ignore Enter or single-letter shortcuts while composing. IME methods (Pinyin, Romaji) pre-fill letters and use Enter to confirm a pending candidate — acting on those keydowns submits or filters mid-word for CJK users.
```js
let composing = false;
input.addEventListener("compositionstart", () => (composing = true));
input.addEventListener("compositionend", () => (composing = false));
input.addEventListener("keydown", (e) => {
  if (e.key === "Enter" && !composing) submit();
});
```

### 4. Make Enter context-aware
Enter should submit in single-line intent and insert a newline inside a code block or multi-line context — Discord sends a message normally but wraps when the caret sits in a markdown code fence. Branch on caret context (and modifier keys) so the same key matches what the user is doing.
```js
function onKeyDown(e) {
  if (e.key === "Enter" && !e.shiftKey && !inCodeBlock()) {
    e.preventDefault();
    send();
  }
}
```

### 5. Pre-fill fields with example content
Seed empty fields with realistic, editable sample values — not just gray placeholder text — because people love to copy. Concrete examples teach the expected format and lower the activation cost of starting.

### 6. Offer paste-from-clipboard near paste-heavy fields
Add a one-tap paste button beside fields that usually receive pasted data — URLs, OTP codes, addresses. It removes the tap-hold-select-paste sequence, which is especially painful on touch.
```jsx
<button onClick={async () => setValue(await navigator.clipboard.readText())}>
  Paste
</button>
```

### 7. Morph the trigger button into the input
When a button reveals an input, animate the button morphing into the field in place rather than opening a dialog. Keeping the control anchored preserves spatial context and avoids modal disorientation.
```css
.field { transition: width 0.2s ease, border-radius 0.2s ease; }
```

### 8. Morph length-limited fields into a live counter
Render remaining capacity as a glanceable live indicator that updates on every keystroke — e.g. a poll item's checkbox transforming into a depleting word-count ring. Continuous feedback prevents the surprise of hitting the cap mid-thought.
```js
input.addEventListener("input", () => {
  ring.style.setProperty("--pct", input.value.length / max);
});
```

### 9. Accept natural-language dates
Parse phrases like "next Friday" or "in 3 days" alongside the calendar picker. People express dates in relative terms; resolving them to a concrete date is faster than navigating a grid.
```js
import * as chrono from "chrono-node";
const date = chrono.parseDate("next Friday"); // → Date
```

### 10. Auto-convert common character sequences
Replace typed sequences with their intended glyphs as the user types — `->` becomes `→`, `--` becomes `—` (Notion and Linear both do this). Convert only unambiguous patterns so the editor feels smart without fighting the user.
```js
input.addEventListener("input", () => {
  input.value = input.value.replace(/->/g, "→");
});
```

### 11. Tolerate overflow so people can finish their thought
Don't hard-truncate or block typing at a visual boundary — let the value scroll or wrap past the field's width and let people complete the input before you validate or trim. Cutting them off mid-thought loses content and trust.
```css
.input { white-space: nowrap; overflow-x: auto; }
```

### 12. Combine magic-link and password into one login field
Let a single field accept either an email (for a magic link) or a password, branching on what was entered instead of forcing the user to pick an auth method up front. One field reads as less work than a choice between two flows.
```js
const isEmail = /^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(value);
isEmail ? sendMagicLink(value) : signInWithPassword(value);
```
