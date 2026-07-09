# V8 — Real product imagery, card rearrangement, The Orbit

index.html only. Standing rules unchanged (no emojis; SVG only; no --nite paint props in transitions; .bird-wing max-width:none stays; facts only).

New assets in assets/: `shot-focusdown.webp` (real App Store phone shot with mascot, WHITE background, 720w portrait), `shot-throwda.webp` (real dashboard screenshot, cream UI, 900w landscape), and 7 painted 1:1 lab vignettes: `lab-content-machine.webp`, `lab-route-optimizer.webp`, `lab-elevator-os.webp`, `lab-language-chatbot.webp`, `lab-algo-visualizer.webp`, `lab-bfs-maze.webp`, `lab-finances.webp`.

## H1. Project panel rearrangement (both Night Shift panels)
New content order inside .panel-content:
1. Eyebrow (mono, keep style) but SLIMMED to category + punch only — remove tech names now that tags exist:
   - FocusDown: `iOS · ~8K lines native Swift · Launched Feb 2026`
   - Throwda: `Fintech · Zero stored value · Pre-launch`
2. Title (unchanged)
3. **Colored .stack-tags row moves here** (directly under the title, above the copy)
4. Copy paragraph(s), callout, stats row, CTA — all unchanged order below.

## H2. Real product imagery replaces the painted panel vignettes
- FocusDown panel visual: `shot-focusdown.webp`. It has a WHITE bg on a dark card — present it as a "polaroid/app-store card": rounded 16px corners, thin cream border (1px rgba cream 12%), soft deep shadow, very slight rotate (-2deg, straightens on panel hover along with existing lift). Max-width ~340px.
- Throwda panel visual: `shot-throwda.webp` — landscape dashboard in a minimal "browser card": same rounded/border/shadow treatment plus a slim browser top-bar drawn in CSS (3 tiny dots, hairline) above the image. Max-width ~460px. Slight rotate 2deg (mirror of FocusDown).
- Both keep loading=lazy, decoding=async, proper width/height attrs (compute from actual file dims), alt text ("FocusDown running on an iPhone, mascot in front" / "Throwda admin dashboard — collections and trends").
- Delete the old vignette-focusdown/vignette-throwda usage from panels (files stay on disk; the GitHub profile still uses copies).

## H3. The Orbit — Lab becomes a semicircular project carousel
Replace the Lab card grid with an interactive orbit (keep section id="lab", header/eyebrow/lede, and nav link text "Lab"):
- **Geometry**: a large semicircle arc (flat side down, like the sun's path) centered horizontally, ~70vh tall zone. 8 project "planets" sit ON the arc: circular thumbnails (painted lab vignettes; 64–84px diameter, active one grows to ~120px) at positions distributed along the arc. The ACTIVE project sits at the arc's apex (top center); others trail down both sides with decreasing size/opacity.
- **Detail card**: below/inside the arc center, the active project's details: name (Fraunces), one-liner (existing true copy from current Lab cards), mono tag (AI AGENTS / PYTHON / C++ etc.), and for the last slot a link chip to github.com/godfreystorm.
- **Projects (8 slots)**: Content Machine (lab-content-machine), Route Optimizer (lab-route-optimizer), Elevator OS (lab-elevator-os), Language-Teaching Chatbot (lab-language-chatbot), Algorithm Visualizer (lab-algo-visualizer), BFS Maze Solver (lab-bfs-maze), Finances Dashboard (lab-finances), and final slot "…and a dozen more" (use the existing small sun/moon footer SVG mark or a simple tangerine dot motif — NOT an image) linking to GitHub. One-liners/tags copied verbatim from the current Lab cards.
- **Interaction**: click/tap any planet → arc rotates (planets animate along the arc path, ~0.6s spring-ish ease) until it reaches the apex; detail card crossfades. Left/right arrow buttons (chip style, SVG chevrons) + ArrowLeft/ArrowRight keys + horizontal drag/swipe on the arc. Wraps around. aria: arc is a tablist or listbox pattern with proper roles/labels; detail region aria-live=polite.
- **Ambient**: slow idle — the whole arc drifts ±1deg breathing; active planet gets a soft tangerine ring glow (matches Vault language).
- **Implementation**: plain JS + CSS transforms (position planets via JS computing arc coordinates from angle; animate a single "rotation offset" value with rAF ease — do NOT use per-element CSS transitions on left/top). Pause idle drift when offscreen (IntersectionObserver). Reduced-motion: no idle drift, rotation snaps instantly; arrows/keys still work.
- **Mobile (≤640px)**: arc compresses (planets 44–56px, active 88px), drag/swipe primary, arrows below; detail card full width. No horizontal overflow.
- Remove the old .lab-grid cards (their copy moves into the orbit data array).

## H4. Verify
node --check; zero console errors; panels: new order renders, tags directly under titles, screenshots load with frames (check naturalWidth), hover straighten works; Orbit: 8 planets positioned on arc (sample coordinates — all should satisfy the arc equation within 2px), click a side planet → becomes apex (assert active index + apex position), keyboard arrows work, wrap works, drag dispatches work via synthetic pointer events, reduced-motion path audit; mobile 375: no overflow, planets ≥44px tap targets; 1440 screenshot at scroll 0 fine.
