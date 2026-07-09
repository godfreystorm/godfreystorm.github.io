# Godfrey Osagiede — Portfolio Site Spec ("Day Shift / Night Shift")

Build a single-file static portfolio: `/Users/godfrey/Documents/godfrey-portfolio/index.html` (all CSS + JS embedded, no build step, no frameworks). Production-grade, stunning, fun, intriguing. **NO EMOJIS anywhere — use inline SVG icons only.**

## Core concept
The page is a journey from day to night. It opens in a warm cream daylight world (his payments day-job at LineLeader) and, as the user scrolls through a "dusk" transition, the sky darkens to deep indigo, the sun sets and becomes a moon, stars fade in on a canvas, and the reader enters the "night shift" (his founder work: FocusDown + Throwda). The scroll drives the theme continuously — not a toggle.

## Aesthetic
- Editorial premium. Fonts via Google Fonts: **Fraunces** (display, use variable axes incl. italic + high weight, tight tracking) + **Instrument Sans** (body) + **JetBrains Mono** (stats, labels, eyebrows).
- Day palette: bg cream `#FDF8F3`, ink espresso `#2A1E17`, accent tangerine `#E8621A`, secondary `#F5A623`.
- Night palette: bg deep indigo-black `#0E0D16`, ink warm off-white `#F4EEE4`, same tangerine/amber accents glowing.
- All themed colors defined as CSS custom properties computed with `color-mix(in oklab, var(--day-X), var(--night-X) calc(var(--nite) * 100%))` where `--nite` (0→1) is set on `<html>` by JS from scroll position. Sections in the day zone force nite≈0, night zone nite≈1, with smooth interpolation across the dusk band.
- Texture: subtle grain overlay (SVG feTurbulence data-URI, low opacity, fixed, pointer-events none). Generous negative space, asymmetric layouts, oversized numerals, thin rules.

## Fixed atmosphere layers (behind content)
1. Sky: `body` background = themed bg var (interpolates via --nite). Add a subtle radial warm glow near the sun position during day.
2. **Sun/Moon**: one fixed circle top-right (~120px, tangerine with soft glow). As --nite goes 0→1 it arcs downward-left slightly, crossfades to a pale moon (`#E8E2D6`) with 2–3 crater circles (SVG or overlapping divs), glow becomes cool.
3. **Stars canvas**: fullscreen fixed `<canvas>`, ~140 stars, gentle twinkle via rAF, opacity = --nite. Pause rAF when opacity 0 or `prefers-reduced-motion`.

## Structure (in order)

### Nav (fixed, top)
Monogram "G.O." in Fraunces italic; right side: mono links DAY / NIGHT / METHOD / CONTACT (anchor links), themed colors. Thin bottom border with low opacity. Backdrop-blur.

### Hero (day, 100vh)
- Eyebrow (mono, tangerine): `DALLAS–FORT WORTH, TX · PAYMENTS & AI PRODUCT`
- Giant Fraunces headline: **Godfrey Osagiede** (clamp up to ~9rem, tight leading). Staggered load animation: each line rises in with opacity (CSS keyframes + animation-delay).
- Tagline in Fraunces italic: “Payments engineer by day. **Founder by night.**” — “by night” in tangerine.
- Sub-line (Instrument Sans, ~65ch): "I own Adyen payment operations across 8,000+ stores in production — then go home and ship revenue-generating products end-to-end: an iOS app with 10,000+ customers and a Nigerian WhatsApp fintech moving ₦35M+ a month."
- Scroll cue: thin vertical line + mono "SCROLL — THE SUN IS SETTING" with a slow pulse.

### Ticker marquee (edge-to-edge, thin borders top/bottom)
Infinite CSS marquee, mono, uppercase, separated by tangerine diamond SVGs: `ADYEN · 8,000+ STORES · TYPESCRIPT · SWIFT · MONGODB · POSTGRES · REACT NATIVE · SUPABASE · PAYSTACK · REVENUECAT · CLAUDE CODE · MCP · DOUBLE-ENTRY LEDGERS · ₦35M+/MONTH · 10,000+ CUSTOMERS` (duplicate content for seamless loop).

### §01 — DAY SHIFT (id="day")
- Section header pattern (reuse everywhere): huge outlined Fraunces number "01" (transparent fill, 1px themed stroke via -webkit-text-stroke) overlapping title "Day Shift", mono eyebrow `LINELEADER · TECHNICAL SUPPORT ENGINEER, PAYMENTS & DATA OPS · 2024 — PRESENT`.
- Lede: primary engineer for all Adyen payment operations (Balance Platform, Management, Legal Entity APIs) for a childcare SaaS serving 2,500+ franchise orgs across US/AU/UK.
- **Stat grid** (2×2 on desktop): count-up numbers in Fraunces (animate on IntersectionObserver, ~1.4s, ease-out; respect reduced-motion by snapping):
  - `8,651` — Adyen stores audited by a Node.js audit-and-repair tool I built
  - `4,088` — misconfigured stores safely repaired under a compliance deadline
  - `2,393 → 87` — indeterminate cases cut via Legal Entity API lookups (animate the 87)
  - `30–40M` — records in a timezone migration I'm leading across 2,905 orgs
- **Production ritual diagram**: horizontal 5-step pipeline with connecting line and mono labels: DRY-RUN → VERIFIED BACKUP → STAKEHOLDER APPROVAL → PER-DOCUMENT APPLY → COUNT VERIFICATION. Steps reveal sequentially on scroll. Caption: "Every production mutation, every time."

### Dusk interlude (the transition band, ~120vh)
Mostly empty sky. Centered giant Fraunces italic line, revealed word-by-word on scroll: **“Then the sun goes down.”** This band is where --nite interpolates 0→1 (map scroll progress across this element). Below it in mono, small: `RAIDERSOUL LLC · FOUNDED 2025 · TWO PRODUCTS IN PRODUCTION`.

### §02 — NIGHT SHIFT (id="night")
Section header: outlined "02", title "Night Shift", eyebrow `RAIDERSOUL LLC · SOLO FOUNDER & ENGINEER`.

Two large project panels, alternating asymmetric layout, themed card bg slightly lifted from night bg, 1px border, glow on hover (translateY -4px, tangerine border-color shift, soft shadow):

**FocusDown — iOS focus app.** Eyebrow: `IOS · REACT NATIVE + ~8K LINES NATIVE SWIFT · LAUNCHED FEB 2026`.
Copy: A focus timer with a travel mascot that journeys around the world as you complete sessions. Screen Time (Family Controls) app blocking, WidgetKit widgets, Live Activities & Dynamic Island — all native Swift.
Stats row (mono): `10,000+ customers` · `4.8★ App Store` · `~1,500 MAU` · `510 solo commits`.
One more line: Built the whole monetization funnel (StoreKit + RevenueCat + Superwall) and a 20+ step Mixpanel onboarding funnel — found and fixed the #1 drop-off (22.6%).
Link (arrow SVG): "On the App Store →" https://apps.apple.com/us/app/focus-down-adhd-study-timer/id6755708778
(NEVER use the word "kawaii". No mascot image — typographic panel with a small orbiting-globe SVG motif.)

**Throwda — WhatsApp-native fintech for Nigeria.** Eyebrow: `FINTECH · TYPESCRIPT · SUPABASE/POSTGRES · PAYSTACK · IN PRODUCTION`.
Copy: Bill payments, P2P, and community collections entirely inside WhatsApp. Live pilot with a Lagos church processing ₦35M+/month. Double-entry bookkeeping with DB-enforced balanced journals, bank reconciliation from real bank statements, ~45K-LOC dashboard with 75+ test files.
Highlight callout (tangerine left-border): "Designed a direct-charge architecture (zero stored value) that legally sidestepped a ₦100M PSSP licensing requirement."
Stats row: `₦35M+/month` · `45K LOC` · `75+ test files` · `0 stored value`.
Link: "throwda.com →" https://throwda.com

### §03 — METHOD (id="method")
Eyebrow `HOW I SHIP`. Title "Multiple-engineer velocity." Short lede: entire workflow is AI-agent-driven — Claude Code, custom agents, MCP servers, autonomous pipelines. Three principle cards (numbered 01/02/03, small SVG icons: crosshair, bolt, loop):
1. "Every screen should convert. Every touchpoint is a growth channel." — I think like a product CEO.
2. "Ship imperfect, iterate fast." — I'd rather ship today and learn than plan for a week.
3. "Test like a BFS." — I walk every path and edge case until it breaks or I trust it.

### §04 — CONTACT (id="contact")
Deep night. Fraunces italic line: "The sun comes up, and I do it again."
Giant email link `godfreystorm@gmail.com` (Fraunces, underline none, animated underline-sweep on hover, tangerine).
Row of icon links (inline SVG icons, 24px, mono labels): GitHub → https://github.com/godfreystorm · LinkedIn → https://www.linkedin.com/in/godfrey-osagiede-a29013144 · Throwda → https://throwda.com · FocusDown → (App Store URL above).
Footer bar: `© 2026 GODFREY OSAGIEDE · RAIDERSOUL LLC` left; right: `DESIGNED & BUILT WITH CLAUDE CODE` and a tiny sun/moon SVG mark.

## Motion & interaction rules
- Scroll reveals: IntersectionObserver adds `.in`; elements start `opacity:0; translateY(28px)`, transition 0.8s cubic-bezier(0.22,1,0.36,1), staggered via transition-delay on children.
- --nite computed in rAF-throttled scroll handler: 0 before dusk band, progress within it, 1 after. Also drives sun/moon transform + crossfade + stars opacity.
- `@media (prefers-reduced-motion: reduce)`: kill marquee animation, twinkle, count-ups snap to final, reveals appear instantly.
- Nav anchor smooth scroll (CSS `scroll-behavior: smooth`).
- Selection color: tangerine bg, cream text.

## Head/meta
- `<title>Godfrey Osagiede — Payments engineer by day, founder by night</title>`
- Meta description + full OG/Twitter tags (og:title, og:description; no image needed).
- Inline SVG favicon data-URI: tangerine circle on transparent.
- lang="en", semantic HTML (header/main/section/footer), alt/aria-labels on icon links, honest heading hierarchy.
- Mobile: fully responsive, clamp() typography, stat grid → 1 col, panels stack, nav collapses to just monogram + CONTACT link. No horizontal overflow ever.

## Quality bar
This must look like a $20K agency one-pager, not a template. Obsess over: consistent spacing scale, aligned baselines, tracking on mono eyebrows (+0.12em), max-width containers (~1200px) with asymmetric bleeds, hover states on every interactive element, focus-visible outlines.
