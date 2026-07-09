# V3 — Painted World integration (Workstream C)

Integrate the hand-painted gouache assets in `assets/` into index.html. They share one style and the exact site palette. The paintings BECOME the atmosphere; the CSS-drawn approximations they replace must be removed. NO emojis. Keep everything else intact (--nite machinery, Vault graph, reveals, count-ups, cursor, marquee).

Assets (WebP, already optimized):
- `assets/sky-day.webp` — cream sky, painted clouds, painted sun in the UPPER RIGHT (2748×1532 source ratio)
- `assets/sky-dusk.webp` — plum→amber sunset, painted sun half-sunk low center-right
- `assets/sky-night.webp` — indigo night, painted milky way + stars, painted moon UPPER RIGHT
- `assets/horizon-day.webp` — Dallas skyline silhouette strip along bottom edge, cream sky above
- `assets/horizon-night.webp` — Lagos night skyline strip along bottom edge (glowing windows, church cross), indigo above
- `assets/vignette-focusdown.webp` — paper plane orbiting a globe, centered motif on dark indigo (1:1)
- `assets/vignette-throwda.webp` — lit Lagos church + palm at night, centered motif on dark indigo (1:1)
- `assets/og.jpg` — social share image

## C1. Painted sky crossfade rig
Replace the flat color sky with three stacked FIXED full-viewport layers (behind everything, above the base color-mix body bg which stays as fallback/paint-under): `background-size: cover; background-position: center top; pointer-events: none;`
Opacity driven each frame in the existing computeNite handler (set as inline style or CSS vars):
- day layer: `1 - smoothstep(clamp(nite/0.5, 0, 1))` → fully gone by nite 0.5
- dusk layer: existing `--dusk` (sin(π·nite))
- night layer: `smoothstep(clamp((nite - 0.5)/0.5, 0, 1))`
REMOVE now-redundant CSS-drawn elements: `.sky-glow`, `.sunset-veil`, the CSS sun/moon celestial element (+ its crossfade & moon-parallax JS), and the CSS cloud blobs. The paintings own sun, moon, clouds, and sunset now. KEEP: the stars canvas + shooting stars (they twinkle ON TOP of the painted night — depth), the birds (they fly across the painted day sky; keep their `(1-nite)` fade), and the grain overlay.
Contrast guard: add a very subtle fixed scrim between sky layers and content — vertical gradient using the themed bg color at ~14% opacity at viewport edges, transparent in the middle — only if body text legibility needs it; check first, prefer no scrim if the paintings are calm enough (they were prompted for negative space).

## C2. Horizon skylines (narrative bookends, in content flow — NOT fixed)
- Bottom of the HERO section: full-bleed `<img src="assets/horizon-day.webp">` (Dallas), bottom-anchored via aspect-ratio crop: wrap in a div `height: clamp(120px, 20vw, 260px); overflow: hidden;` with the img `object-fit: cover; object-position: bottom;` so mostly the skyline strip shows and the cream above blends into the day sky. `alt="Painted silhouette of the Dallas skyline"`.
- In the CONTACT section, below the icon links / above the footer bar: same treatment with `assets/horizon-night.webp` (Lagos), `alt="Painted Lagos skyline at night with lit windows"`. Its indigo must blend with the night bg — if the tones differ slightly, add a soft-edge mask (`mask-image: linear-gradient(to top, black 75%, transparent)`).
- Both get a slow reveal (existing `.reveal` pattern) and subtle parallax if cheap (translateY ±10px from scroll, optional — skip under reduced-motion).

## C3. Project panel vignettes
In the FocusDown panel, REPLACE the orbiting-globe SVG motif with `<img src="assets/vignette-focusdown.webp" loading="lazy">`; add the Throwda vignette to the Throwda panel in the mirrored position (`assets/vignette-throwda.webp`). Both: max-width ~420px, `mask-image: radial-gradient(closest-side, black 55%, transparent 98%)` so the square melts seamlessly into the card (the image bg is near the night card color already). Decorative → `alt=""` + `aria-hidden="true"` wrapper fine. Hover: very subtle scale(1.03) with slow ease; none under reduced-motion.

## C4. Meta + loading
- `og:image` / `twitter:image` → absolute-safe relative `assets/og.jpg`, plus `twitter:card = summary_large_image`.
- `<link rel="preload" as="image" href="assets/sky-day.webp">` in head. After window load, warm the others (new Image() for sky-dusk, sky-night, horizons).
- Vignettes `loading="lazy" decoding="async"`.

## C5. Verify (real constraints)
No horizontal overflow 375/1440. Day layer fully hides by mid-dusk (no painted day sun ghosting into night). Only ONE sun/moon visible at any scroll position (the painted ones). Night: canvas stars twinkle over the painting. Panels: vignettes blend with no visible square seam. Everything still 60fps — layers are opacity-only compositing.
