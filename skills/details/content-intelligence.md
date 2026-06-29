# Content Intelligence Details

Smart defaults, context-aware behavior, and automatic detection that anticipate what the user means before they spell it out.

**Skip when:** The inference is wrong often enough that users stop trusting it, or when "smart" behavior overrides an explicit choice the user already made. Any silent transformation (auto-embed, auto-convert) must stay reversible and never fire on ambiguous input.

## Rules

### 1. Name files from their capture context
Auto-name screenshots and exports from the visible app, page title, or content at capture time — don't try to be clever with timestamps and metadata prefixes, just name the thing what it is. A file named `Stripe-dashboard.png` is findable months later; `Screenshot 2026-06-29.png` is a graveyard entry.
```js
const name = `${detectVisibleTitle() ?? activeApp}.png`;
```

### 2. Auto-segment video chapters
Detect real content changes in video and create chapter markers automatically. Users jump to the relevant section instead of scrubbing blindly. Trigger on scene cuts, slide changes, or speaker shifts — not fixed intervals.

### 3. Snap crop to component edges
When crop handles drag within a few pixels of a UI boundary, snap to that edge. It eliminates pixel-peeping and produces clean, professional cuts instantly.
```js
if (Math.abs(handleX - edgeX) < SNAP_THRESHOLD) handleX = edgeX;
```

### 4. Pre-flight user content for missing pieces
Scan user content for omissions that will cause real harm — a missing unsubscribe link before sending a broadcast, "I've attached" with no attachment, a merge with unresolved conflicts — and block or warn before the irreversible action. This is poka-yoke: move the checklist off the user and onto the system.
```js
if (action === 'send' && !/unsubscribe/i.test(emailHtml)) {
  block('No unsubscribe link found — add one before sending.');
}
```

### 5. Make detected data actionable
Detect addresses, phone numbers, and links in document and message viewers and make them tappable. Removes the copy-paste-switch-app cycle for common real-world data like an address buried in a PDF.
```html
<a href="tel:+14155550123">(415) 555-0123</a>
<a href="https://maps.apple.com/?q=...">1 Infinite Loop</a>
```

### 6. Use human-readable IDs
For user-visible identifiers, generate readable project-scoped IDs like `DTD-123` instead of UUIDs or raw auto-increments — they're easy to remember, say aloud, and reference in conversation. For IDs users never see, like a database index, random strings are fine.
```js
const id = `${project.prefix}-${project.nextSequence++}`; // "DTD-123"
```

### 7. Offer inline unit conversion
Detect units — temperature, distance, weight, currency — in text and offer inline conversion to the user's preferred system. Kills mental math and app-switching at the point of reading.
```js
text.replace(/(\d+)°F/g, (m, f) => `${m} (${Math.round((f - 32) * 5 / 9)}°C)`);
```

### 8. Respect whitespace on cut and paste
When cutting or pasting words in space-delimited languages, fix up the surrounding spaces so the user never lands on doubled or missing whitespace. A 1984-era Mac detail that still trips up most editors. Skip for languages without word spaces (Chinese, Japanese, Thai).
```js
// deleting a word: collapse the leftover double space
text = text.replace(/  +/g, ' ').replace(/ ([.,!?])/g, '$1');
```

### 9. Tap an amount in a photo to convert it
When a viewer shows monetary amounts in an image, let users tap any value for an instant local-currency conversion in a contextual popover — no copy, no calculator, no exchange-rate search. The same "tap to transform" pattern extends to measurements, time zones, and dates.

### 10. Honor paste context
Read the surface the paste lands on. A URL dropped onto a canvas or visual board becomes a rich embed; the same URL in a text field or code editor stays literal text. Only transform when context makes intent unambiguous and the change is cheaply reversible.
```js
if (target.type === 'canvas' && isURL(pasted)) insertEmbed(pasted);
else insertText(pasted);
```

### 11. Search by intent, not keyword match
Resolve what the user means, not just what they typed: "change my password" must surface account-security settings even when those exact words never appear, and "John" should mean something different in mail than in contacts. Match on meaning, synonyms, and the current context.

### 12. Interpret time against the human day model
When a user says "wake me at 9am tomorrow" at 1am, "tomorrow" means after their next sleep — eight hours out, not 32. The human day flips at sleep, not midnight. When the interpretation is genuinely ambiguous, ask which day rather than guessing wrong.

### 13. Give folders semantic icons by name
Assign recognizable icons by naming convention — "Documents", "Developer", "Downloads" — so users scan instead of read. macOS does this automatically the moment a folder is named.
```js
const ICONS = { Documents: '📄', Developer: '⌨️', Downloads: '⬇️' };
const icon = ICONS[folder.name] ?? '📁';
```

### 14. Live-sync the title to the field being edited
When the field is the primary identifier, reflect edits in the page or tab title in real time, before save — it tightens the loop between "what I'm typing" and "what this becomes," and doubles the browser tab as a live preview. Don't do this for non-identifier fields or values where mid-typing updates would disorient (a banking balance updating per keystroke).
```js
input.addEventListener('input', e => { document.title = e.target.value; });
```

### 15. Tap the active tab again to reset it
Re-tapping the tab you're already on should reset state: scroll to top, refresh data, or pop back to the section root. The first tap navigates, the second gives a predictable, muscle-memory home base when you're lost deep in a feed.
```js
function onTabPress(tab) {
  if (tab === activeTab) tab.scrollToTop({ refresh: true });
  else navigate(tab);
}
```

### 16. Tailor CTA copy to the visitor's time
Phrase the call to action against the current moment — "Start Building Tonight," "Try Better Reading This Weekend," "Send a Gift by Christmas." The same button feels written for this visitor, right now.
```js
const cta = isEvening(now) ? 'Start Building Tonight' : 'Start Building Today';
```
