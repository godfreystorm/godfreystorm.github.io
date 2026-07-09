# V2 — "Blow the doors off" upgrade

Two workstreams. Design tokens/fonts/voice: match the existing site (read index.html first). NO EMOJIS anywhere, SVG only. Never the word "kawaii". Facts only from SPEC.md + this file — invent nothing.

---

## Workstream A — index.html upgrades

### A1. NEW SECTION: "The Vault" (the centerpiece — an Obsidian-graph-view of Godfrey's head)
Placement: after §03 Method, before Contact. Section id="vault". Header pattern like other sections: outlined giant "04", title **"The Vault"**, eyebrow `EVERYTHING CONNECTS · MY HEAD, MAPPED`. Short lede: "I live in an Obsidian vault and ship with an agent fleet. This is what the inside looks like — every node is real. Drag them. Click the bright ones."

Full-width canvas panel, height ~85vh (70vh mobile), transparent background so the night sky + stars show through; thin themed border top/bottom. Custom force-directed physics on `<canvas>` (vanilla, verlet integration: pairwise repulsion, spring edges, weak centering gravity, velocity damping ~0.9, gentle idle drift so it never fully freezes). rAF loop paused via IntersectionObserver when offscreen. Target 60fps (spatial early-out fine at this node count).

**Interaction:** pointer drag any node (grab cursor); hover → node glows brighter, its label goes full opacity, connected edges + neighbors light up, everything else dims to ~25%; click/tap a MAJOR node → detail panel slides in from the right (mobile: bottom sheet) with Fraunces title, mono meta line, 2–3 sentence story from real facts, optional link, close X (SVG). Only one panel open; Esc/backdrop closes.

**Visual language (Obsidian graph, branded):** nodes are soft-glow dots, radius by weight (hubs ~10px, major ~7px, minor ~4px). Colors: tangerine `#E8621A` = products/day-job hubs; amber `#F5A623` = skills/tech; cream `#F4EEE4` at 75% = places/meta. Edges: 1px warm white at 10–14% alpha; highlighted edges tangerine at 55%. Labels: JetBrains Mono 10–11px, uppercase; hubs+major always visible at 85%, minor at 30% (full on hover). Nodes drift subtly; a slow breathing glow pulse on the 4 hubs.

**Graph data (embed as JS array; ~55 nodes).** Hubs: `GODFREY` (center), `DAY SHIFT`, `NIGHT SHIFT`, `METHOD` — all edged to GODFREY.
- DAY SHIFT cluster: LineLeader, Adyen, Balance Platform, Legal Entity API, KYC, 8,000+ stores, Audit-and-repair tool, 4,088 repaired, MongoDB, MySQL, 30–40M-record migration, 24+ runbooks, Production ritual.
- NIGHT SHIFT splits to two sub-hubs (major): **FocusDown** → Swift (~8K lines), WidgetKit, Live Activities, Dynamic Island, Screen Time API, React Native, Expo, RevenueCat, Superwall, Mixpanel, 10,000+ customers, 4.8★, Travel mascot, TikTok growth, 10 signed creators. **Throwda** → WhatsApp, Nigeria, Lagos pilot, ₦35M+/month, Paystack, Double-entry ledger, Bank reconciliation, Direct-charge architecture, ₦100M license (sidestepped), VTPass/eBills, Supabase, Postgres, 75+ test files, Nigerian Pidgin.
- METHOD cluster: Claude Code, AI agents, MCP servers, Ship fast, BFS testing, Product-CEO thinking.
- Meta (cream, edged to GODFREY): Dallas–Fort Worth, Texas Tech B.S. CS '24, Raidersoul LLC, The Alchemist.
Cross-links that make it feel alive: Supabase↔FocusDown, TypeScript? (skip TS node), Adyen↔KYC↔Throwda? NO fabrication — valid cross-links: Supabase–FocusDown, Supabase–Throwda, Postgres–Supabase, Claude Code–FocusDown, Claude Code–Throwda, AI agents–TikTok growth, Nigerian Pidgin–BFS testing (he tests the bot in pidgin), Paystack–Direct-charge architecture, KYC–Adyen.
MAJOR (clickable, has panel story): GODFREY, DAY SHIFT, FocusDown, Throwda, METHOD, Adyen, Audit-and-repair tool, Swift, Screen Time API, RevenueCat, Double-entry ledger, Direct-charge architecture, ₦35M+/month, Claude Code, The Alchemist, Texas Tech, Raidersoul LLC, 10,000+ customers. Write panel stories from SPEC.md facts; The Alchemist: "The book that stuck. Solo founder, trust the process, follow the Personal Legend."; links where relevant (App Store, throwda.com, github).
Mobile: hide minor labels, same physics, touch drag + tap.

### A2. Hero & day-sky world-building
- 2–3 enormous soft cloud shapes (blurred CSS ellipse stacks, 8–12% opacity) drifting extremely slowly across the day sky (120–200s loops); their opacity ×(1 − nite) so they vanish at night.
- A tiny flock of 4–5 birds (simple two-stroke chevron SVGs, staggered) crossing the hero sky every ~45s, day only. Subtle, small, far away.
- Per-letter staggered hero-name entrance (split "Godfrey Osagiede" into spans, translateY+rotate 2deg rise, 30ms stagger) replacing the current line-level animation. On hover of the name, letters do a gentle individual lift (translateY −6px, 60ms stagger ripple from the hovered letter — CSS/JS lightweight).

### A3. Night-sky upgrades
- Shooting stars: every 7–15s while night is visible, a streak (bright head, fading tail) crosses a random upper-sky path on the existing stars canvas, ~700ms.
- Moon cursor-parallax: moon translates up to ±8px toward cursor position (lerped, subtle).

### A4. Global micro-interactions
- Custom cursor (fine pointers only, `@media (pointer:fine)`): 6px tangerine dot + 28px themed ring lerp-trailing it; ring scales ×1.8 over links/buttons/graph nodes; hidden over the graph canvas when dragging (use grab/grabbing there). Native cursor hidden only where custom is active. Respect reduced-motion (static dot, no trail).
- Magnetic hover on nav links + the giant contact email (element translates up to 4px toward cursor, springs back).
- Marquee pauses on hover.
- Nav: add link `RÉSUMÉ` → `resume.html` (before Contact). Also add same link near contact icons row.
- prefers-reduced-motion: disable clouds/birds/shooting stars/cursor trail/magnetic/per-letter (fall back to simple fade); graph renders static layout (pre-run simulation N ticks synchronously, no idle drift) but hover/click still work.

Quality bar unchanged: no jank, no horizontal overflow, everything themed via existing custom-property system.

## Workstream B — resume.html (new file)
An editorial, print-perfect rendering of the real résumé. Read content from `/Users/godfrey/Documents/godfrey_resume/Godfrey_Osagiede_Resume_MAIN.md` — content verbatim (fix: LineLeader start date is "Dec 2024", replacing the "[START MONTH YEAR]" placeholder).
- Same head/meta patterns, fonts, and day-palette tokens as index.html (copy the token block; static day theme — no scroll theming). Title: "Résumé — Godfrey Osagiede".
- Fixed minimal nav: "← G.O." (to index.html) left; right: mono button "PRINT / SAVE PDF" → `window.print()`.
- Layout: editorial masthead (giant Fraunces "Godfrey Osagiede", mono contact line with real links), tangerine hairline, then two-column (main 2/3: Summary, Experience with company/role/dates in a hanging mono column; aside 1/3: Skills grouped, Education, Links). Bullets get tangerine diamond markers (SVG/CSS). clamp() responsive → single column mobile.
- `@media print`: white bg, near-black text, hide nav/button, serif headings kept, tight margins, page fits 1–2 pages, links show as plain text (no URL dumps), `print-color-adjust: exact` for the tangerine accents.
- Subtle entrance reveals on load; reduced-motion respected. NO emojis.
