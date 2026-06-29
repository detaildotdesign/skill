# Easter Egg & Delight Details

Hidden surprises, playful touches, and personality woven into otherwise routine surfaces.

**Skip when:** The surprise adds maintenance cost or distracts from the core task with no payoff. Never put delight into high-stakes or serious flows — payments, account deletion, medical, legal, or error recovery where the user needs clarity, not whimsy.

## Rules

### 1. Embed a real detail in your app icon
Hide a genuine, verifiable detail in the app icon. Apple's Mail icon carries a real address ("Apple Park, California 95014"). It rewards curious users and signals that someone cared about the small stuff.

### 2. Use iconic text in document-app icons
Set the placeholder text in a text-editor or document icon to something meaningful rather than lorem ipsum. macOS TextEdit's icon once held the full "Here's to the crazy ones" passage addressed to a real person — the icon becomes a cultural artifact and a talking point instead of filler.

### 3. Hide a surprise in the footer
Footers are the least-visited region of a page, so an interactive Easter egg there rewards only the users who explore furthest — exactly the audience worth delighting. It takes a footer from 99 to 100.

### 4. Carry the active state into the footer
When the same navigation appears in both the header and the footer, light up the active link in the footer too. Most sites forget the footer nav; honoring it reinforces spatial awareness and shows the system is coherent everywhere, not just up top.

### 5. Credit the team with a credits roll
End the landing page with a movie-style "cast" section naming the people who built the product. It humanizes the product, gives the team visible credit, and takes the landing from 99 to 100.

### 6. Turn error and 404 pages into a moment of delight
A missing page or error is a dead end by default. Treat it as a hidden room of your site rather than a void: add warmth and light interactivity so the few who land there leave less frustrated, not more. Few people get there, but it's nice to reduce their sadness when they do — the same care you'd give the corners of your house guests never see.

### 7. Hide an egg in developer tools
Tuck a small visual surprise into your inspect or dev tooling — Vercel's inspect tool has shipped a "golden thumb" since day one. It delights the power users who spend the most time inside your product.

### 8. Expose a "Meet the Developer" via a hidden gesture
Reach a "Meet the Developer" view through a non-obvious gesture or keystroke. The discovery itself creates a personal connection between the makers and the users who find it.

### 9. Give your AI features a living character
Let users customize or personify the AI (Notion's editable AI character), and have the assistant narrate its work with varied, human verbs the way Claude Code does. It isn't just for fun — a product that feels alive feels less like a tool and more like a collaborator.

### 10. Cycle the cursor through brand colors
In a branded input, let the caret blink through your brand palette instead of a single flat color — Google's AI search cursor cycles blue, red, yellow, green. It animates an otherwise static element and quietly signals "this is a special mode."

### 11. Randomize the emoji on hover
Cycle the emoji icon on each hover of an emoji button. It is near-zero cost and adds playful unpredictability to an otherwise routine interaction.
```js
const emojis = ["😀", "🎉", "✨", "🐙", "🍕", "🚀"];
button.addEventListener("mouseenter", () => {
  button.textContent = emojis[Math.floor(Math.random() * emojis.length)];
});
```

### 12. Turn activity data into a visual narrative
Plot the user's activity — visited workspaces, projects, locations — on an interactive map or timeline (Conductor maps visited workspaces). Usage data becomes a story users enjoy exploring rather than a dry log.

### 13. Make text in images live and formattable
Use system live-text recognition so users can select, copy, and even apply formatting like highlights to text inside images. It blurs the line between a static image and interactive content.

### 14. Make invite codes memorable
Format invite codes as color names with their hex values instead of random strings (Paper uses `#ffffff-pure-white`). A mundane token becomes something memorable and delightful to share.
```js
const palette = { "tomato": "#FF6347", "azure": "#007FFF", "amber": "#FFBF00" };
const name = pickRandomKey(palette);
const code = `${name}-${palette[name].slice(1)}`; // "azure-007FFF"
```

### 15. Slip meaning into a system date
Where you set a fallback or sentinel timestamp, pick one with a story. When you cancel a Safari download, the file's creation date becomes January 24, 1984 — the day the original Macintosh was unveiled. The curious user who notices gets a small wink.

### 16. Make the "not available on mobile" screen playful
A desktop-only experience needs a mobile fallback anyway, so make it land with personality instead of a dry warning. symbl.space sends mobile visitors to Amazon to buy a bigger monitor — the redirect turns a limitation into a joke.
