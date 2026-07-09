# V4 — Readability, Vault overhaul, more life, more Godfrey (user feedback round)

All in index.html unless stated. NO emojis. Facts only from SPEC.md/SPEC-V2.md + the data below — invent nothing.

## D1. Hero "g" descender clipped (BUG)
The hero name's per-letter animation wrappers clip descenders — the "g"/"y" bottoms get cut. Fix properly: give the letter-line wrappers enough bottom padding (or switch clip windows to `overflow: visible` once the entrance finishes) so Fraunces descenders render fully at every viewport width. Verify with getBoundingClientRect vs font metrics at 375/768/1440.

## D2. Birds — more life
Currently one small flock. Make it feel alive but never cartoonish: 3 independent flocks (5–7, 3–4, and 2 birds), different altitudes (top 8%, 22%, 38% of viewport), different scales (far = smaller/slower/fainter), different crossing directions (two L→R, one R→L), staggered timing so roughly one flock is on screen at any moment during day, with occasional gaps. Slight per-bird bob. All day-only (`(1−nite)` fade), all removed under reduced-motion.

## D3. The Vault — readability + affordance overhaul (the big one)
Users say: hard to read at night, everything clustered, no clue you can click. Fix all three:

**Spatial organization by category (cluster homes).** Assign each cluster an anchor region on the canvas: GODFREY center; DAY SHIFT cluster upper-right quadrant; FOCUSDOWN lower-left; THROWDA lower-right; METHOD upper-left; meta nodes ring near center. Add a weak per-node spring toward its cluster anchor. Increase inter-cluster repulsion and lengthen springs BETWEEN clusters, shorten within, so clusters read as distinct constellations with clear space between them. Scale anchor spread with canvas size; on mobile compress vertically.

**Cluster region labels.** Behind each cluster, painted-in faint mono uppercase caption (canvas text): "DAY SHIFT", "FOCUSDOWN", "THROWDA", "METHOD" — large (28–36px), ~7% cream opacity, sitting under the nodes like a watermark. Skip for meta.

**Label legibility.** Major+hub labels: 12–13px JetBrains Mono at 95% opacity with a soft dark backdrop pill behind the text (rounded rect, rgba(10,9,18,0.78), 4–6px padding) so labels read over edges/stars/milky way. Minor labels 10–10.5px at 60% with a lighter pill (0.55 alpha); full opacity on hover. Never let labels render without their pill.

**Canvas backdrop.** Give .vault-canvas-wrap a subtle lift so the graph reads as a surface: very soft radial gradient (center rgba(244,238,228,0.05) → transparent 75%) plus the existing borders. Dim canvas stars behind the graph area is unnecessary once pills exist.

**Click affordance.** (a) Clickable (major) nodes get a slow pulsing outer ring (2s breathe, tangerine at ~35%). (b) Hover on clickable: pointer cursor + a small mono "OPEN" tag drawn near the node. (c) Legend row above/below the canvas, mono small: [pulsing-ring dot] STORIES — CLICK · [amber dot] SKILLS · [cream dot] CONTEXT · DRAG ANYTHING. (d) Change hint line to "DRAG TO PLAY — RINGED NODES OPEN STORIES". (e) First-view nudge: 1.5s after the graph first enters the viewport, briefly pulse ALL clickable rings brighter once (skipped under reduced-motion).

## D4. "NOW" strip (what I'm working on)
Slim full-bleed band directly under the hero horizon strip (above the marquee), themed borders: mono eyebrow `NOW` in tangerine, then 3 items in a row (stack on mobile), each mono small: 
1. "Shipping FocusDown growth experiments — creator program at 10 signed"
2. "Scaling Throwda past the ₦35M+/month pilot"
3. "Leading a 30–40M-record timezone migration at LineLeader"
Right-aligned link: "COMMIT LOG → github.com/godfreystorm" (real link). Add an HTML comment marking this as the editable "now" data. Keep honest and curated (no fake live counters).

## D5. NEW SECTION: The Lab (id="lab", between Vault and Contact; renumber header to 05 if Vault is 04)
Eyebrow `EXPERIMENTS · THE STUFF THAT DIDN'T MAKE THE HEADLINE`. Title "The Lab". Short lede: "Not everything ships to the App Store. Everything teaches something."
Compact grid (3 cols desktop / 1 mobile) of small cards — name (Fraunces, modest), one-liner (true descriptions only), mono tag:
- Content Machine — AI pipeline that generates and posts daily short-form content on its own · AI AGENTS
- Route Optimizer — AI multi-stop delivery routing desktop tool built at RHD Industries · PYTHON
- Elevator OS — elevator scheduler with an optimized round-robin algorithm · C++
- Language-Teaching Chatbot — NLP + OpenAI chatbot for interactive language learning · PYTHON
- Algorithm Visualizer — pathfinding and sorting, animated with pygame · PYTHON
- BFS Maze Solver — breadth-first search maze solver · PYTHON
- Finances Dashboard — personal finance dashboard for my own money · HTML/JS
- Plus a final card, slightly different styling: "…and a dozen more half-finished experiments on GitHub." linking to github.com/godfreystorm.
Cards: 1px themed border, hover lift, reveal stagger. Add "Lab" to nav if it fits pre-collapse; else nav unchanged and it's reachable by scroll.

## D6. Global readability pass
- Audit every text style at BOTH extremes (--nite 0 and 1): body copy ≥ 4.5:1 contrast, ledes ≥ 4.5:1, mono labels/eyebrows ≥ 3:1 (bump --ink-fade/--ink-faint alphas if needed — check the night side especially).
- Panel body copy: line-height ≥ 1.65, max ~62ch.
- Dusk interlude "RAIDERSOUL LLC" line and word-reveal: recheck at nite 0.35/0.5/0.65 against the PAINTED dusk sky now (not the old gradient) — the amber horizon band is busier than the old flat gradient; if the line sits over the painted sun/horizon at common viewports, nudge its position up or strengthen its color.
- Stat labels under count-ups: bump size/contrast one notch.
- Focus-visible: every link/button/canvas gets a clear tangerine outline.

## D7. Verify
Real constraint set as before (375/1440, no overflow, zero console errors, reduced-motion paths, 60fps). For the Vault specifically: confirm clusters are visually separated (measure mean intra- vs inter-cluster node distance), labels all have pills, pulsing rings render, legend present.
