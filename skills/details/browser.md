# Browser & Platform Details

How an app integrates with browser chrome, OS theming, URLs, link previews, and PWA surfaces.

**Skip when:** The app is a short-lived internal tool, an embed, or a single-screen utility — themed favicons, dynamic theme-color, and rich link previews add maintenance cost with no payoff. Don't serialize state into the URL when it's private, large, or meaningless to a recipient.

## Rules

### 1. Sync theme-color with the page background

The address bar shouldn't be a white strip. Match `<meta name="theme-color">` to the current background so the browser chrome merges with the app for a native-like, immersive feel. Update it dynamically on scroll, theme, or route changes — not just at load.

```js
document
	.querySelector('meta[name="theme-color"]')
	.setAttribute("content", getComputedStyle(document.body).backgroundColor);
```

### 2. Ship light/dark favicon variants

Serve separate favicon SVGs gated by `prefers-color-scheme` so the icon never clashes with the OS tab bar in either mode.

```html
<link
	rel="icon"
	href="/favicon-light.svg"
	media="(prefers-color-scheme: light)"
/>
<link
	rel="icon"
	href="/favicon-dark.svg"
	media="(prefers-color-scheme: dark)"
/>
```

### 3. Reflect live status in the favicon

On pages with a clear state — a GitHub PR, a CI workflow, an export job — repaint the favicon to mirror it (pending → pass/fail, draft → published, unread count). It becomes an at-a-glance indicator readable across a wall of tabs without switching to any of them. Deep respect for a cluttered workspace.

### 4. Adapt media to theme mode

Almost anything on the page can respond to light/dark mode — not just colors. Swap photos, illustrations, or video sources so imagery stays legible and on-brand in both modes instead of glowing or washing out against the opposite background.

```html
<picture>
	<source srcset="/hero-dark.avif" media="(prefers-color-scheme: dark)" />
	<img src="/hero-light.avif" alt="" />
</picture>
```

### 5. Serialize UI state into the URL

Encode filters, search query, sort, view mode, selection, and even scroll position as URL params. The view becomes shareable, bookmarkable, and survives back-button and refresh — a saved URL is a snapshot of a configuration, not just a destination. For one-way data it also avoids a database round-trip entirely.

```js
const params = new URLSearchParams({ q: query, sort, view });
history.replaceState(null, "", `?${params}`);
```

### 6. Serve from the apex domain

Drop the `www.` subdomain and serve from the bare apex. Modern DNS, CDNs, and HTTP/2/3 handle apex domains fine; the prefix is legacy noise, and cleaner URLs are easier to type, share, and remember.

### 7. Use PNG or JPEG for Open Graph images

Render OG images as PNG or JPEG, never WebP — Facebook, X, LinkedIn, and iMessage have inconsistent or no WebP preview support, so it silently fails to render. Keep key content (text, logos) inside a ~1000×500px center safe zone to survive cropping across platforms.
