# V5 — Affordance & life round

index.html only. Standing rules: no emojis; SVG only; facts only; no --nite-driven paint props in CSS transitions; `[hidden]{display:none}` where needed. Don't change fonts or the overall design language.

## E1. Button-style affordances for clickable things
Clickable should LOOK clickable without breaking the editorial style. Introduce ONE consistent "chip button" idiom and use it everywhere an outbound action exists:
- Style: mono uppercase 11–12px, letter-spacing .12em, 1px solid tangerine border, pill radius (999px), padding ~0.55em 1.1em, transparent bg; hover = tangerine bg + cream text + slight translateY(-1px); focus-visible ring. Include a small inline SVG arrow (↗ style, drawn as SVG path) after the label.
- Apply to: FocusDown panel CTA ("ON THE APP STORE"), Throwda CTA ("THROWDA.COM"), NOW strip's "COMMIT LOG", contact icon row (keep icons, wrap each in the chip style OR keep icons plain and add one chip "EMAIL ME" — designer's judgment, stay tasteful), Lab's final "…and a dozen more" card link, résumé page's PRINT button already looks like this — align it to the same idiom.
- The panels stay fully clickable (stretched link) — the chip is the visible affordance inside them.

## E2. Vault affordance v2 — make clickability UNMISSABLE
Current pulsing rings aren't landing. Three changes:
1. **Chip-labels for story nodes**: clickable (major) node labels render as small tangerine-BORDERED pills (keep dark fill for readability, add 1px tangerine stroke at 65% alpha + a tiny "+" glyph appended to the label text, e.g. "ADYEN +"). Non-clickable labels keep the plain dark pill, no border, no plus. Rings stay.
2. **Corner hint badge**: persistent small overlay pinned inside the canvas top-right (HTML element over the canvas, not canvas-drawn): chip-button style, text "CLICK THE ORANGE CHIPS" with a slow 2s pulse. On first node-click it fades out permanently (this page load).
3. **First-view demo**: when the graph first enters the viewport (once per page load), after 900ms auto-open the GODFREY story panel. The user closing it is the lesson: everything ringed opens. Skip under reduced-motion (open nothing). Ensure it can't fire while the user is already dragging/clicking.

## E3. Bird migration
Replace the current timid flocks. Goal: an actual migration moment across the whole hero, then quiet.
- 14–18 birds total in 2–3 V-formation skeins (offset chevrons, leader ahead, followers staggered behind in a V), sizes varied (far skein small/faint, near skein larger).
- Paths cross the FULL hero diagonally (e.g. enter lower-left ~55% height, exit upper-right ~12%), not hugging the top edge. Two skeins L→R, one R→L at a different altitude/timing.
- Wing flap: 2-frame CSS animation on each bird (alternate chevron open/closed via transform scaleY or an SVG path swap), staggered flap phases.
- Timing: one big migration sweep begins ~2s after load (the moment), takes ~18–25s to cross; afterwards skeins recur on long staggered cycles (~45–70s) with gaps. Day-only (`(1−nite)` opacity), pause/remove under reduced-motion.
- Subtle per-bird vertical bob so the V breathes. Keep them silhouette-simple (espresso strokes), consistent with the gouache world — no detailed bird art.

## E4. Verify
Chip buttons render on both themes (forced --nite 0/1) with AA contrast; Vault chip-labels visibly distinct from plain labels via canvas pixel sampling; hint badge present, fades after synthetic node click; auto-open fires once (and not under reduced-motion); migration paths cover the hero (sample bird positions over time via getBoundingClientRect on a few birds), no overflow, zero console errors, 375/1440.
