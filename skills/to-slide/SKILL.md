---
name: to-slide
description: Turn source material into a self-standing HTML slide deck in the personal design system — one claim per sheet, drawn exhibits, presented live in the browser.
disable-model-invocation: true
---

Turn the source material into a **deck**: engineering datasheets re-set for projection, presented live in the browser. A deck carries an **argument** — the through-line the room must take away — not the record; the record belongs to a to-report datasheet.

This skill is self-contained — read its bundled reference as needed:

- `DESIGN.md` — the projection discipline and the deck grammar; the deck must pass its "A working test" before it's presented.
- `styles.css` — the slide-scale tokens and components; use them, don't invent.
- `examples/` — copy-ready exhibit idioms, one file per type: `tables.html`, `figures.html`, `charts.html`, `status-lists.html`. Open the one matching the exhibit you're building.
- `TEMPLATE.html` — the blank deck to copy and fill; the engine script lives inside it.

## 1 — Mine the source for the argument

Per the grammar in `DESIGN.md`:

1. Name the **through-line** — the one sentence the room must take away.
2. Extract **one claim per sheet**, ordered to carry it.
3. Pick the **single strongest exhibit** per claim, using the form mapping.
4. Drop the leftovers, or point them at a to-report datasheet.

Then write the **sheet outline**: title sheet, each content sheet as *claim + exhibit form*, closing takeaways and asks — with the index sheet included only at ~5+ content sheets.

Completion: the outline is shown to the user and **approved** — edits folded in — before any rendering.

## 2 — Place the deck

The output is one standalone HTML file; ask the user for the destination path unless it's obvious. Assign the metadata: doc ID (`<PROJ>-SLD-NNN`), rev letter (`A` for a new deck), ISO date, owner initials, sheet count.

Completion: destination path and every titleblock value are fixed.

## 3 — Render

Copy `TEMPLATE.html`, replace its stylesheet `<link>` with an inlined copy of `styles.css`, and fill it sheet by sheet from the approved outline, cribbing each exhibit from the matching file in `examples/`. Build figures as inline SVG in the system's idiom. Add per-deck CSS only in a local `<style>` block, and only when no existing component fits.

Completion: the file passes every check in `DESIGN.md` → "A working test".

## 4 — Present

Open the deck in the browser and click through it end to end: navigation forward and back, fragments landing one per keypress, progress bar and `n / N` advancing, a mid-deck reload landing on the same sheet.

Completion: the click-through reaches the closing sheet with no dead keys, no overflowing sheet, and no exhibit unreadable at the window size presented.
