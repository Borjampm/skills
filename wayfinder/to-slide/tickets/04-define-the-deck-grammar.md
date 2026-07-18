# Define the deck grammar

- Status: closed
- Type: `wayfinder:grilling` (HITL)
- Assignee: borjampm
- Blocked by: [Prototype a design-system deck](03-prototype-a-design-system-deck.md)

## Question

How does arbitrary source material become a deck? to-report has a mapping (comparison → table, flow → figure, options → status-list); slides need their own:

- the slide inventory: title slide, agenda?, one-decision-per-slide?, closing slide;
- what each slide type carries, and the density ceiling per slide;
- when content becomes a fragment build versus appearing whole;
- deck metadata: doc ID scheme (`<PROJ>-SLD-NNN`?), rev, date, owner — where it lives on screen;
- how the skill's "mine the source" step differs from to-report's step 1 when the source is not a conversation.

Resolved when the mapping is written down sharply enough to become the skill's planning step.

## Resolution

Grilled live, 2026-07-18. The grammar, sharp enough to be the skill's planning step:

**Skeleton (adaptive index).** Every deck: title sheet → content sheets → closing sheet. The `§` index sheet appears only when the deck has ~5+ content sheets; short decks stay lean.

**Closing sheet: takeaways + asks.** A short mono list of key takeaways (`T-n`) plus any decisions the audience must act on (`A-n`) — the presentation equivalent of conclusions. With nothing to ask, it falls back to plain END OF DECK furniture.

**Density ceiling: lead + one exhibit.** A content sheet is one claim (the lead, ≤38px, ≤24ch) plus at most ONE exhibit proving it: a table (≤6 rows), a figure/chart, a status list (≤6 items), or one grid-2 panel pair. Two exhibits means two sheets. A full-bleed exhibit may stand alone when it *is* the claim.

**Form mapping (to-report's, re-scoped to the sheet):** comparison → `data-table`; flow/architecture/sequence → SVG figure (flowchart idiom for logic, schematic for structure); magnitude → single-hue ink-bar chart; option/state set → status list; rationale → the lead itself. Prose paragraphs don't exist on sheets.

**Fragments: sequence builds, evidence lands whole.** Fragments are reserved for content the presenter walks through item-by-item (status lists, takeaway/ask lists, step sequences). Leads, tables, figures, charts and panel pairs always appear whole. Rule of thumb: if you'd say "next…" aloud between items, it builds.

**Metadata: `<PROJ>-SLD-NNN`**, rev letters (A, B, … — decks revise like drawings), ISO date, owner initials, `Sheet nn of NN`. Lives where the prototype put it: full titleblock on the title sheet, `DOC-ID · n / N` in every content header.

**Mining step: argument + outline check.** Unlike to-report's exhaustive record, the deck mines for the argument: (1) name the through-line — one sentence the room must take away; (2) extract one claim per sheet, ordered to carry it; (3) pick the single strongest exhibit per claim; (4) leftovers are dropped or pointed at a to-report datasheet; (5) show the user the sheet outline for approval **before** rendering.

**Examples cut (settles the map's fog):** `examples/` ships one file per exhibit type — tables, figures (schematic + flowchart), charts, status-lists/fragments — with the title, index, and closing idioms living in the TEMPLATE deck itself.
