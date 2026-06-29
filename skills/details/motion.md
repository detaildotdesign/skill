# Motion & Animation Details

Transitions, hover states, micro-animations, and layout choreography that make state changes legible and spatially coherent.

**Skip when:** The control is used many times per session — motion that delights once becomes friction on the hundredth use, so favor instant feedback there. Always gate non-essential motion behind `prefers-reduced-motion: reduce` and ship a static fallback.

## Rules

### 1. Animate the action button to preview its result
Tie an action button's motion to its function so the transition foreshadows the outcome (e.g., a plus rotating into a close icon, an add control morphing into a confirmation). Motion that maps to the result reads as feedback, not flourish.

### 2. Transition icon color through intermediate hues
When an icon's color changes in a picker, interpolate from the old color to the new one instead of snapping every part at once. The eye can track a moving value; a hard cut loses it. Watch it at half speed and the smoothness is obvious.

### 3. Animate a signature stroke-by-stroke
Reveal a signature or handwritten mark by drawing its path over time (animate `stroke-dashoffset` from the full path length to zero) rather than fading the whole thing in. Tracing the stroke order mimics the act of signing, which reads as alive where a static reveal reads as an image.

### 4. Make state-based icons mirror what they control
Animate a toggle icon to trace the motion of the thing it operates — a sidebar icon that opens and closes exactly like the panel, or a paperclip that opens on hover to signal "click to unpin." Shared timing makes the control feel physically connected to its surface, turning the icon into a tiny preview of the action.

### 5. Use ASCII loaders in text-only interfaces
Cycle character-based spinner frames in terminal or CLI contexts where no graphical loader exists. It signals progress and adds personality. Libraries like `cli-spinners` ship ready-made frame sets.

### 6. Bleed blurred edges to the container boundary
For a fade-out at the edge of scrollable content, use a blurred overlay that extends fully to the container edge rather than stopping short and leaving a clipped line. A soft falloff reads as depth; if the blur doesn't reach the edge you get a visible seam, so add bleed.

### 7. Cross-fade content through blur, not a hard swap
When an icon morphs into another or an image first paints, ease the change through a brief blur instead of a hard cut. A blur-up softens the moment a real image replaces its placeholder and smooths an icon swap, so the transition reads as one element changing rather than two elements flickering.

### 8. Pin a chat minimap for long transcripts
In long conversations, pin a subtle minimap to the viewport marking the region in view so users don't lose their place. The indicator glides to the new anchor as you scroll (ease in over ~240ms so it feels calm), section labels brighten on hover to signal it's interactive, and clicking plays a brief focus ring in the transcript to confirm the jump landed.

### 9. Close modals back to their origin, tracking scroll
When dismissing a modal that expanded from a grid item or trigger, throw it back to that element's current rect — and if the page scrolled mid-transition, smoothly retrack to the new position like a magnet. Returning to origin preserves the spatial link between trigger and surface; Apple Music demonstrates it well.

### 10. Lock the cursor while a surface morphs
Pin the cursor style for the duration of a morph so the form elements appearing underneath don't flicker the pointer between shapes. A stable cursor sustains the read of one continuous motion — enable interactive elements only after the transition completes.

### 11. Make animations interruptible
Let a close or cancel fire mid-open instead of queuing behind the animation. Run transitions on interruptible properties so a new target retargets from the current value; blocking input until an animation ends reads as lag, while immediate response feels durable and snappy.

### 12. Choreograph layout shifts instead of jumping
When an element expands (album art, media controls), slide its neighbors to their new positions rather than letting them jump. On macOS Tahoe's music player the controls slide down to make room, maintaining the spatial relationship so the user can follow the change. Layout shifts should be choreographed, not instant.

### 13. Reserve the bold width to prevent font-weight shift
Give each label an invisible `::after` carrying the same text at the heavier weight (via a `data-text` attribute, set to `height: 0; visibility: hidden`), so toggling weight on hover doesn't reflow neighbors. The element is already as wide as its boldest state, killing the cumulative layout shift.

### 14. Reduce edge blur while scrolling fast
Drop heavy backdrop blur during fast scroll and restore it once motion settles. Heavy blur in motion destroys perceived smoothness and spikes GPU work; lowering it dynamically keeps the interface crisp and faster, then eases the full blur back in after a short idle.

### 15. Reduce animation for frequently used features
Cut or shorten motion on tools invoked repeatedly (a launcher, a frequent command) — when a user opens with a clear goal, they want no friction, not delight. Reserve full motion for first-run or rare moments where the spectacle still lands.

### 16. Shake the surface to mark a dead end
When an action can't proceed (wrong input, locked control), give a short horizontal shake as feedback for the dead end. The shake is a physical "no" — legible without a message, and it stops where the user's intent stops.

### 17. Slide the highlight block between items
Move a selection highlight between items (nav, tabs, code blocks, segmented control) with a sliding transition rather than an instant jump. The slide ties the old and new positions into one continuous object the eye can follow.

### 18. Stagger reveals to set event order
When several elements enter at once, offset each by a small delay so they resolve in reading order. Simultaneous motion splits attention; a stagger hands the eye a path and a clear order of events.

### 19. Animate the transition between two open tooltips
When the pointer moves between adjacent targets in a group, transition the existing tooltip's position and content instead of hiding and re-showing it. Reusing one element kills the off/on flicker and makes a row of controls feel like one connected surface.
