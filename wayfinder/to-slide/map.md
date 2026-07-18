# Map: to-slide skill

Label: `wayfinder:map`
Tickets: [`tickets/`](tickets/)

## Destination

`skills/to-slide/` exists, mirroring to-report's anatomy (SKILL.md + design guidance + template + examples), and turns any source material into a self-standing HTML slide deck presented live in the browser — validated by producing one real deck and clicking through it.

## Notes

- Domain: the personal design system — engineering-datasheet aesthetic. Canonical reference: [`skills/to-report/DESIGN.md`](../../skills/to-report/DESIGN.md) and [`skills/to-report/styles.css`](../../skills/to-report/styles.css).
- The dev driving this map has never built HTML slides — research before engine decisions, prototypes before design decisions.
- Consult the `writing-great-skills` skill when authoring the SKILL.md.
- Standing preferences (settled at charting):
  - A slide library dependency (e.g. reveal.js) is acceptable — self-containment is not a hard requirement.
  - Aesthetic: **same design system, adapted for projection** — same tokens and mono/sans grammar, rethought for big type and one-idea-per-slide density.
  - V1 features: keyboard/click navigation, slide numbers / progress, incremental reveals.
  - Decks are standalone files saved wherever the user asks — no experiments-index registry step.
- This map carries execution: the destination is the built skill, so the build and validation tickets are in scope, not a hand-off.

## Decisions so far

<!-- one line per closed ticket: gist + link -->

- [Survey the HTML slide landscape](tickets/01-survey-the-html-slide-landscape.md) — surveyed reveal.js/Slidev/impress.js/bespoke.js/hand-rolled; research recommends hand-rolling a ~70-line vanilla template (runner-up: reveal.js on a blank theme); full findings in [`assets/html-slide-landscape.md`](assets/html-slide-landscape.md).
- [Choose the slide engine](tickets/02-choose-the-slide-engine.md) — hand-rolled vanilla template (transform-scale fitting, ~70 lines JS for nav/fragments/progress/hash); reveal.js blank-theme noted as the fallback only if scope ever grows to speaker notes / PDF.
- [Prototype a design-system deck](tickets/03-prototype-a-design-system-deck.md) — 10-sheet prototype accepted ([`assets/prototype-deck.html`](assets/prototype-deck.html), PDS-SLD-001 Rev B): 28px body floor on a 1280×720 canvas; one header line of furniture (no footer, no crop marks); § spine + `n / N` header position; ≤ 2 red per sheet; scaled (not pinned) hairlines; copied `:root` tokens + slide-scale type set; flowchart and ink-bar chart idioms approved.
- [Define the deck grammar](tickets/04-define-the-deck-grammar.md) — adaptive skeleton (index only at ~5+ content sheets); closing = takeaways + asks; density ceiling = lead + one exhibit per sheet; fragments only for spoken sequences; `<PROJ>-SLD-NNN` with rev letters; mining = argument + through-line with an outline checkpoint before rendering; examples cut = one file per exhibit type.
- [Build the to-slide skill](tickets/05-build-the-to-slide-skill.md) — `skills/to-slide/` built mirroring to-report's anatomy (SKILL.md · DESIGN.md · styles.css · TEMPLATE.html · 4 examples); dry run passed in a live browser — nav, fragments, progress, and hash all working.

## Not yet specified

<!-- empty — the grammar settled the examples cut; the remaining route is fully ticketed -->

## Out of scope

- **Speaker notes / presenter view** — not selected for V1; returns only as a fresh effort if the destination is redrawn.
- **PDF export / print layout** — the deck medium is live-in-browser; print is a different artifact.
- **Registering decks in the design-system experiments index** — decks are standalone; no registry step.
