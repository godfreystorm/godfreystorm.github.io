# V6 — More life, faster rhythm, mobile-first polish

index.html (+resume.html only where noted). Standing rules unchanged: no emojis; SVG only; no --nite-driven paint props in CSS transitions; keep the design language exactly.

## F1. Birds — immediate, triple, fast returns
- Start IMMEDIATELY: first skein enters within ~0.3s of load (drop the 2s+ delays; stagger skeins 0.3s/1.5s/3s).
- TRIPLE the population: ~45–55 birds total. Go from 3 skeins to 6–7 (V-formations of 4–9, plus 2–3 lone stragglers), spread across altitudes 8%–60% of the hero, mixed directions/depths (far = smaller/fainter/slower). Keep silhouette chevrons; vary --vx/--vy so formations don't look cloned.
- Fast returns: when a skein exits, it re-enters after only a SHORT pause (2–5s), not a long dead cycle. Restructure keyframes so crossing ≈ 80–88% of the cycle and off-screen hold ≈ 12–20%, with total cycle lengths ~20–34s per skein (staggered so the sky always has life but never feels like a swarm loop).
- Keep day-only fade and reduced-motion removal. Watch perf: these are transform/opacity-only CSS animations, fine at 55 elements.

## F2. Shooting stars — more, faster
- Spawn cadence: every 2.5–6s while night is visible (currently 7–15s), with occasional double streaks (~25% chance spawn a second 300–600ms later on a different path).
- Vary length/speed/brightness per streak; paths across the whole upper 2/3 of the sky, both diagonal directions.
- After a streak exits, next one comes relatively fast (the cadence above handles it). Keep them on the existing stars canvas; pause when night not visible; reduced-motion = none.

## F3. Heavy mobile optimization (keep ALL design + animations)
Audit and fix at 360/375/414 widths (and one small-height landscape). Keep the aesthetic identical, tune the experience:
- **Hero**: name sizes to full width without orphan letters; tagline + lede comfortable; painted sky crops well (background-position tweak per breakpoint if needed); birds still visible but fewer/smaller (cap ~24 birds on <=640px via hiding some skeins).
- **Tap targets**: every link/chip/icon ≥ 44×44 CSS px hit area (padding, not visual bloat). Nav on mobile: currently collapses to monogram + Contact — add a compact inline row of the key anchors (Day/Night/Vault/Résumé) in mono 11px if it fits without wrapping ugly; otherwise keep collapse but ADD the Résumé link to the visible set.
- **The Vault on touch**: canvas height 70vh ok; ensure drag doesn't fight page scroll (touch-action already none — verify one-finger vertical swipe OUTSIDE canvas scrolls page; inside canvas drags nodes; consider allowing pan-y when not touching a node: hit-test on touchstart, only preventDefault when starting on/near a node). Hint badge repositions inside viewport; bottom-sheet panel: ensure close button reachable, content scrollable, backdrop tap closes.
- **Panels/vignettes**: images cap sensibly (~320px) so cards don't get too tall; stat rows wrap cleanly.
- **NOW strip / marquee / pipeline / Lab grid / contact**: single-column stacks with consistent spacing scale; marquee speed slightly slower on mobile (readability).
- **Performance**: add `<meta name="theme-color">` (day cream; plus media prefers-color-scheme dark variant with night indigo); ensure font loading uses display=swap (verify the Google Fonts URL); keep preload day sky only; confirm total JS work on load is minimal; no layout thrash in the rAF handler (read scrollY once per frame).
- **Custom cursor**: stays pointer:fine-only (no artifacts on touch); magnetic effects off on touch.
- **iOS quirks**: 100vh sections — use svh (`height: 100svh` with vh fallback) for hero and vault so the address bar doesn't cause jumps; `-webkit-tap-highlight-color: transparent` globally; check position:fixed sky layers render correctly in Safari (standard usage, fine).
- resume.html: verify mobile single-column still clean with chip button; nothing else.

## F4. Verify
- Bird counts + cadence: assert ≥45 skein children on desktop DOM, ≤~24 visible on mobile; sample skein positions at forced currentTime points across the cycle to confirm short off-screen holds.
- Shooting-star cadence: instrument temporarily (counter on window) and assert ≥8 streaks spawned in 40s with night forced visible... verify via code-path reading + a shortened-interval test override, then remove overrides.
- Mobile: 360/375/414 screenshots at scroll 0, DOM/computed checks elsewhere (no overflow anywhere, tap-target sizes via getBoundingClientRect ≥44, svh applied, theme-color present).
- Zero console errors; JS syntax node --check.
