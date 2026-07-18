# Choose the slide engine

- Status: closed (2026-07-18)
- Type: `wayfinder:grilling` (HITL)
- Assignee: BMP (claimed 2026-07-18)
- Blocked by: [Survey the HTML slide landscape](01-survey-the-html-slide-landscape.md)

## Question

Given the survey's findings, which engine does to-slide standardize on — reveal.js, another library, or a small hand-rolled navigation layer? The decision weighs aesthetic control (the design system must be imposable without fighting the engine), the V1 feature set (keyboard nav, progress, fragments), single-file/offline behavior of decks, and how much slide-authoring convention the skill inherits versus defines itself.

## Resolution

**Hand-rolled vanilla template** (research's Variant A — JS-driven slide switching), decided 2026-07-18 with the survey's findings in hand.

- The skill emits a self-standing template carrying ~60–80 lines of vanilla JS: `transform: scale(min(vw/W, vh/H))` fitting, arrow/space/click navigation, `.fragment` incremental reveals, `n / total` progress display, and `#/N` URL-hash deep-linking.
- Rationale: the datasheet aesthetic is reductive and wants an empty stylesheet as its substrate — every library (reveal's themes and injected transform styles, impress's 3D paradigm, Slidev's build output, bespoke's dormancy) would be effort spent subtracting. The template cost is paid once by the skill, not per deck; single-file/offline is trivially clean; no dependency can rot.
- Accepted trade-off: we own the scaling math and edge cases (rapid keypresses, resize, hash-on-load); no free overview/speaker-notes/PDF — all out of scope for V1.
- reveal.js 6 on a blank theme remains the known fallback if a future effort needs speaker notes or PDF export; switching would be a destination redraw, not a reopening of this ticket.
