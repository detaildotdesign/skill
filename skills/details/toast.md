# Feedback & Status Details

Ambient feedback, status indicators, loading affordances, confirmations, and dismissible surfaces that report what happened without stealing focus.

**Skip when:** The signal is critical and blocking (payment failure, data loss) — use a real dialog or inline error, not a toast that auto-dismisses. Don't pile ambient indicators onto a flow that already gives clear synchronous feedback; redundant motion reads as noise.

## Rules

### 1. Confirm subscriptions with double opt-in

Send a confirmation email and require the user to click before activating a newsletter signup. It protects people from being subscribed by someone else's typo and protects your sender reputation by keeping the list to addresses that actually exist and want in.

```js
async function subscribe(email) {
	const token = crypto.randomUUID();
	await store.pending(email, token);
	await sendEmail(email, `Confirm: https://app/confirm?t=${token}`);
	// activate only when the link is clicked
}
```

### 2. Indicate lifecycle state in one ambient indicator

Show process state (loading, active, complete) with a small persistent indicator — a pulsing dot or ring that morphs through the stages rather than swapping between separate icons. Continuous ambient feedback removes the uncertainty that makes users re-trigger an action mid-flight, and packing every state into one shape keeps the eye in place.

```css
.status-dot {
	width: 8px;
	height: 8px;
	border-radius: 50%;
	background: var(--accent);
	transition: all 0.3s ease;
}
.status-dot.active {
	animation: pulse 1.5s ease-in-out infinite;
}
.status-dot.done {
	background: var(--success);
}
@keyframes pulse {
	50% {
		opacity: 0.4;
		transform: scale(0.85);
	}
}
```

### 3. Make loading explain its own cause

Use restrained motion for spatial context, not decoration — a load bar that visibly fills to trigger a view switch tells the user what is about to happen and gives the interaction a tactile feel. Self-documenting motion teaches causality without copy or a generic spinner.

```css
.commit-bar {
	width: 0;
	height: 3px;
	background: var(--accent);
	transition: width 0.6s linear;
}
.commit-bar.filling {
	width: 100%;
} /* fill completes -> view switches */
```

### 4. Turn completion into presence, not absence

When a background action finishes (updates, syncs, migrations), don't just remove the item from its pending list — surface it in a "Recently updated" section right below. Apple moves finished App Store updates there instead of letting them vanish; the loop closes visually, reassuring the user it actually shipped and giving quick access to what just changed.

### 5. Write a thoughtful "What's New" message

Even a changelog few people read deserves real copy. Skip the bare "Bug fixes and improvements" — a warm, specific line acknowledges the quiet work and makes the release feel cared-for.

```md
We fixed the small things you'd never notice, so you never have to.
Nothing flashy this time. Just a smoother, faster, lighter app.
```

### 6. Collapse instead of close

For dismissible panels like "What's New," offer "Collapse" rather than "Close" so the content minimizes instead of vanishing. People are hiding the popup, not destroying it — keep the return path one click away (Linear and Vercel both do this).

### 7. Delay the promo close button

Gate the close button on a promotional banner or "What's New" popup behind a short delay so the headline gets a beat of exposure before it can be dismissed. It forces a split-second glance — less hostile than omitting the close affordance, and it still respects the user's exit.

### 8. Validate content before the destructive send

Scan user-generated content for predictable mistakes (missing unsubscribe link, broken URL, empty required field) and surface a warning before the irreversible action fires. Catching the error pre-send is cheaper than an apology email after.

```js
function preflight(email) {
	const warnings = [];
	if (!email.body.includes("unsubscribe"))
		warnings.push("No unsubscribe link");
	if (/href="(?!https?:)/.test(email.body))
		warnings.push("Broken or relative URL");
	return warnings; // block send while non-empty
}
```

### 9. Shake on disabled click

A click on a disabled control must acknowledge the input. Silent no-ops read as a broken UI; a brief shake says "heard you, not yet" without an intrusive alert.

```css
@keyframes shake {
	0%,
	100% {
		transform: translateX(0);
	}
	25% {
		transform: translateX(-4px);
	}
	75% {
		transform: translateX(4px);
	}
}
.btn[aria-disabled="true"].clicked {
	animation: shake 0.25s ease;
}
```
