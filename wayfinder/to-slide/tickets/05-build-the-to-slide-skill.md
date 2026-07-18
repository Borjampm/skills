# Build the to-slide skill

- Status: closed
- Type: `wayfinder:task` (AFK)
- Assignee: borjampm
- Blocked by: [Prototype a design-system deck](03-prototype-a-design-system-deck.md), [Define the deck grammar](04-define-the-deck-grammar.md)

## Question

Write `skills/to-slide/` mirroring to-report's anatomy: SKILL.md (stepped, with completion criteria, per the writing-great-skills skill), design guidance for the projection-scale rules the prototype settled, the slide stylesheet, a TEMPLATE deck, and `examples/` files for the idioms the grammar named. Resolved when the skill files exist and a dry run of the SKILL.md steps holds together.

## Resolution

Built `skills/to-slide/` (2026-07-18), mirroring to-report's anatomy — eight files:

- **`SKILL.md`** — user-invoked (`disable-model-invocation: true`, like to-report), four steps with checkable completion criteria: (1) mine the source for the argument, ending at the outline-approval checkpoint the grammar mandated; (2) place the deck and fix `<PROJ>-SLD-NNN` metadata; (3) render from TEMPLATE with `styles.css` inlined (decks are standalone); (4) present — full click-through as the completion bar.
- **`DESIGN.md`** — records only the projection *deltas*, pointing at `to-report/DESIGN.md` for the parent discipline (single source of truth): canvas/stage, the slide type scale, one-header-line furniture, the two numbering axes, the ≤2-red budget, plus the full deck grammar (skeleton, lead + one exhibit ceiling, form mapping, fragments rule, mining step) and a 6-check "A working test" for decks.
- **`styles.css`** — tokens copied verbatim from the datasheet sheet + the slide-scale type set and 3px `--rule-heavy`, the engine CSS, and every component the prototype accepted (slide-head, titleblock, agenda, data-table + `.pick`, figure, status-list, panels, `end-list` for the closing sheet). Prototype-only demos (specimen, hairline demo) dropped.
- **`TEMPLATE.html`** — «PLACEHOLDER» scaffold: title sheet, deletable index sheet, one clonable content sheet, closing takeaways/asks sheet, and the ~70-line engine; header comment carries the rules the scaffold can't enforce.
- **`examples/`** — one file per exhibit type as the grammar cut: `tables.html`, `figures.html` (schematic + flowchart), `charts.html` (ink bars), `status-lists.html` (fragment build + end-list); each is itself a clickable mini-deck linking `../styles.css`.

Dry run: TEMPLATE and figures example opened in a real browser — title/content/closing sheets render on the dark stage, fragments land one per keypress, backward nav unwinds them, progress bar advances, and `#n` hash deep-links to the right sheet. Screenshots verified at each stop.
