# Browser & URL Details

Rules for favicons, theme colors, URL handling, meta tags, and PWA behavior.

## Rules

### Dynamic theme-color

Set and dynamically update `<meta name="theme-color">` to match the current page background. This merges the browser chrome with your app, creating an immersive, native-like experience.

### Themed favicon for light/dark mode

Provide separate favicon SVGs for light and dark mode using the `media` attribute in your icon metadata. A favicon that clashes with the OS theme looks out of place.

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

### Monochrome PWA icon for Android

Include a monochrome icon with `"purpose": "monochrome"` in your web manifest for Material You theming on Android. Your PWA icon blends seamlessly into the user's personalized home screen.

### Immersive pinned tab icon

Design pinned-tab favicons whose borders or colors match the tab chrome for a seamless look. A well-integrated pinned tab icon elevates perceived quality.

### Serialize state in URL

Serialize UI state (filters, search queries, sort order, view mode) into URL parameters. Enables sharing, bookmarking, back-button navigation, and session continuity — all for free.

### Drop the www prefix

Stop using the `www.` subdomain — serve your site from the apex domain. The www prefix is a legacy convention; dropping it creates cleaner URLs.

### Use PNG/JPEG for OG images

Use PNG or JPEG for Open Graph images, not WebP. Facebook, Twitter/X, LinkedIn, and other platforms have inconsistent WebP support for OG image previews.

### Dynamic favicon for status

Update the favicon in real-time to reflect page status (CI pass/fail, notification count, PR state). The favicon becomes a persistent, zero-cost status indicator visible across all tabs.

### Portal-proof components

Ensure your components work correctly when embedded in iframes, portals, or third-party contexts. Embedded contexts have different sizing, event handling, and style inheritance rules.

### Clickable email with copy fallback

Make email addresses clickable with `mailto:` links, and offer a copy-to-clipboard alternative via an icon. Give users the choice between their mail client and manual paste.

### Rich previews for internal URLs

Detect internal links and render them as rich preview cards with title, description, and thumbnail. A glanceable card is faster to scan than a raw URL.

### Rich previews for external URLs

Fetch Open Graph metadata for external links and display them as rich preview cards. Elevates the reading experience and gives users confidence about where a link leads.

### Prevent swipe-back on horizontal scroll

Apply `overscroll-behavior-x: none` to horizontally scrollable containers to prevent the browser's swipe-back navigation from triggering accidentally.

### Announce link exit

For external links, either append an Open in new Tab icon or use a tooltip to describe the action (For example, "Open https://detail.design in new tab").
