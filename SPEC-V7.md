# V7 — Birds everywhere, shooting-star iceberg, auto-open bug, NOW into hero

index.html only. Standing rules unchanged (no emojis; SVG only; no --nite paint props in transitions; .bird-wing keeps max-width:none; facts unchanged).

## G1. Birds: instant + EVERYWHERE (not just the hero)
Problems: (a) takes ~3s before any bird is visible; (b) birds only exist in the hero — user wants them across the WHOLE page during day, fading out through dusk as usual.
- Move the migration layer to a **fixed, full-viewport layer** (like the sky layers, pointer-events:none, z-index just above sky/below content) so skeins cross the screen at ANY scroll position. Opacity of the layer continues to be ×(1−nite) from the rAF handler (birds gone by night). Do NOT reduce density anywhere — keep 8 skeins/46 birds desktop, spread altitudes across 5–85vh so they populate the whole viewport, not one band.
- Instant visibility: give 2–3 skeins **negative animation-delay** (e.g. -8s, -15s, -22s into their cycles) so on first paint they are ALREADY mid-crossing on screen. First visible bird ≤ 0.5s after load, guaranteed by geometry not timing.
- Mobile keeps the ≤24 cap (hide skeins as now) but same fixed-layer everywhere behavior.

## G2. Shooting stars: the iceberg
Night sky should feel alive with meteors while staying classy:
- Base cadence: a streak every **0.8–2.5s** while night visible (from 2.5–6s).
- Doubles ~35%, triples ~12% (staggered 200–500ms, independent paths).
- **Meteor-shower bursts**: every 25–45s, a burst of 5–8 streaks over ~2.5s radiating from the same sky region (shared origin ±80px, similar angle) — reads as a real shower moment.
- Vary length/speed/brightness as now; allow a rare (8%) long slow "fireball" (1.4× length, 1.5× duration, brighter head).
- Keep on stars canvas, pause offscreen, reduced-motion = none. Perf: cap concurrent streaks at 10.

## G3. BUG: GODFREY story auto-opens repeatedly
It must fire ONCE per page load maximum, only on the FIRST time the vault enters view, and never again (unobserve/flag). Also suppress it entirely if the user has already interacted with the graph (drag/click/hover-click) before first-view fires. Find the re-trigger cause (likely the IntersectionObserver re-firing or the flag being reset by a later code path) and fix properly.

## G4. NOW strip → into the hero
The NOW band below the hero goes unnoticed. Move its content INTO the hero, directly under the sub-lede: a single compact line — tangerine mono chip-label `NOW`, then ONE item at a time cycling with a gentle 5s fade rotation through the 3 items, then the small `COMMIT LOG ↗` chip link at the end of the line. Left-aligned with the lede, understated but visible on first paint. Remove the old NOW band entirely (keep the HTML-comment editable data concept — the items live in one JS array). Reduced-motion: show first item statically, no rotation. Mobile: wraps to two lines cleanly.

## G5. Verify
- Fixed migration layer: sample a skein's getBoundingClientRect at scroll 0, mid-page (day zone), and confirm presence at 30%/60% viewport heights; confirm layer opacity 0 at forced --nite 1.
- Negative delays: at t=0 after reload, ≥1 skein bounding box intersects the viewport (assert via forced currentTime 0 → actually just reload and measure immediately).
- Star cadence: run spawn logic with synthetic clock — expect ≥18 streaks/40s median, burst fires within any 60s window; cap respected.
- Auto-open: reload → scroll to vault → panel opens once; close; scroll away and back → does NOT reopen. Simulate user click before first view → no auto-open.
- NOW line: present in hero at scroll 0 (screenshot), old band gone, rotation code guarded for reduced-motion.
- Usual: zero console errors, node --check, no overflow 375/1440.
