---
name: to-report
description: Turn the current research conversation into an engineered datasheet report (HTML) in the personal design system — the decisions reached, the comparisons made, and the diagrams that carry them.
disable-model-invocation: true
---

Turn this research conversation into a **report**: an engineering **datasheet** in the personal design system — dense, panelized, numbered, drawn. The report exists to preserve **the record** — what was decided and why — so the conversation never has to be reread.

This skill is self-contained — read its bundled reference as needed:

- `DESIGN.md` — the system's discipline; the report must pass its "A working test" before it ships.
- `styles.css` — the component classes and tokens; use them, don't invent.
- `examples/` — copy-ready idioms, rendered in-system, one file per resource type: `tables.html`, `graphs.html`, `diagrams.html`, `annotations.html`. Open the one matching what you're building.
- `TEMPLATE.html` — the blank scaffold to copy and fill.

## 1 — Mine the conversation for the record

Re-read the whole conversation. Capture, in note form:

- every **decision** reached, and the rationale behind it;
- the alternatives that were weighed, and why each was rejected;
- open questions still unresolved;
- every concrete datum — numbers, comparisons, option sets, tradeoffs, sequences.

Completion: the record is exhaustive — someone reading the finished report alone would know everything decided in this conversation and why, without opening the chat. Nothing of consequence is left only in the conversation.

## 2 — Place the report

The output is one self-contained HTML file; where it lives depends on the project, so ask the user unless it's obvious.

- **Inside this design-system repo** → the next free number, `experiments/NN-<slug>.html`, with `<link rel="stylesheet" href="../styles.css">`. Register it in step 5.
- **Any other project** → ask for the destination path, and make the file standalone: inline the full contents of the bundled `styles.css` into a `<style>` block instead of linking it. Skip step 5.

Completion: destination path and stylesheet strategy (link vs. inline) are both fixed.

## 3 — Plan the datasheet

Assign a doc ID (`<PROJ>-RPT-NNN`), title, owner, ISO date, revision, and the headline numbers for the metric strip. Then give every item from the record a form — **show, don't explain**:

- a comparison → a `data-table`
- a flow, architecture, sequence, or relationship → a drawn SVG `figure`
- an option set or set of states → a `status-list`
- discrete values → `field`s
- rationale, intent, nuance → prose (the exception, never the default)

Completion: every record item is assigned to a numbered section and a form, and prose carries only what data cannot.

## 4 — Render

Copy `TEMPLATE.html` and fill it, cribbing idioms from the matching file in `examples/` (a comparison → `tables.html`, a chart → `graphs.html`, a flow or schema → `diagrams.html`, a callout/code/status block → `annotations.html`). Build figures as inline SVG in the system's idiom — 1px ink strokes, signal-red numbered callout balloons, mono labels, dimension lines. Add per-report CSS only in a local `<style>` block, and only when no existing component fits (the helpers in `examples/` — code blocks, ranking bars, PK/FK keys — are ready to lift).

Completion: the file passes every check in `DESIGN.md` → "A working test" — every section numbered and the number visible, every identifier in mono, no paragraph that should be a table/list/diagram, title block + doc ID + footer present, signal color used fewer than ~6 times, and the page survives black-and-white.

## 5 — Register (repo experiments only)

Add a row to the register table in `index.html` (ID, title, doc ID, status `ACTIVE`, ISO date) and bump the entry count in both the titleblock (`Entries`) and the metric strip so they match the new total.

Completion: the new entry links correctly and every count in `index.html` equals the number of experiments.
