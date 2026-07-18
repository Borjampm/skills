# Prototype a design-system deck

- Status: closed
- Type: `wayfinder:prototype` (HITL)
- Assignee: borjampm
- Blocked by: [Choose the slide engine](02-choose-the-slide-engine.md)

## Question

What does the personal design system look like at projection scale? Build a rough 6–8 slide deck in the chosen engine — real enough to present, cheap enough to throw away — and react to it live. The prototype must force the open design questions into the concrete:

- type scale: what the datasheet's 14px density becomes at one-idea-per-slide;
- the titleblock: does every slide carry doc-ID/rev furniture, or only the title and closing slides?
- numbering: how the `§ 1.0` spine and slide-number/progress display coexist;
- signal red budget: per slide or per deck;
- panels, hairlines, mono/sans grammar at distance;
- one table, one SVG figure, and one fragment build rendered at slide scale;
- stylesheet strategy (graduated fog, sharpened by the hand-rolled engine choice): does the slide stylesheet start from `styles.css`'s `:root` tokens copied into a new slide-scale sheet, or diverge — and does the 1px-hairline discipline survive `transform: scale` or need `1px / s` pinning?

Resolved when the reaction session has produced verdicts on each of the above.

## Assets

- [`assets/prototype-deck.html`](../assets/prototype-deck.html) — 10-sheet prototype (PDS-SLD-001 Rev B), hand-rolled engine, one open question per sheet; sheet 10 lists the verdicts.

## Resolution

Reaction session run live against the prototype (Rev A reviewed, revised to Rev B on feedback, Rev B accepted). Verdicts:

- **Type scale** — slide canvas is 1280×720; the 14px datasheet becomes a **28px body floor**, nothing below **16px furniture**. Full slide-scale set: meta 16 / small 20 / body 28 / lead 38 / title 56 / display 96 (title sheet only).
- **Furniture** — full gridded titleblock on the title sheet only. Every content sheet carries exactly **one header line**: `§ id + section name` left, `DOC-ID · n / N` right, 1px ink rule under it. **No footer** (rejected as duplicating the header) and **no corner crop marks** (rejected as barely visible — the sheet edge against the dark stage does the framing).
- **Numbering** — two independent axes: the `§` spine numbers the *content* (sections, `Fig. 3.2`, `Tbl. 2.1`, panel `6.1`); sheet position is `n / N` in the header. They never compete.
- **Signal red budget** — **≤ 2 uses per sheet**: the § id plus at most one emphasis (selected table row, figure callout balloons). Progress bar is *not* red (paper-on-dark, 4px, viewport-fixed under the sheet).
- **Hairlines** — **scaled (A)**: 1px logical scaling with the sheet, holding visual weight at projection. Pinned `calc(1px / s)` rejected (goes wire-thin at distance).
- **Stylesheet strategy** — copy `styles.css`'s `:root` tokens **verbatim** (colors, fonts, rules) into a slide sheet and add the slide-scale type set on top; only the scale is new. One deviation: `--rule-heavy` is 3px at slide scale (2px reads thin at distance).
- **Diagram & chart idioms** (added mid-session at the dev's request) — approved: SVG flowcharts (ink boxes/diamonds, YES/NO labeled edges, square corners) and single-hue ink bar charts per the dataviz method (no legend for one series, mono tabular value labels at bar ends, recessive gridlines, square bar ends — the system bans rounding). Figures sit in the dotted-grid `.figure` frame with mono `Fig. n.n` captions.
- **Engine confirmations** — the hand-rolled ~70-line engine (transform-scale fit, arrow/space/click nav, fragments, hash, progress) survived a full click-through; dark stage `#141414` around the sheet approved.
