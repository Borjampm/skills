# DESIGN.md

The datasheet system re-set for projection. Everything in [`to-report/DESIGN.md`](../to-report/DESIGN.md) — the instrument-panel posture, the mono/sans grammar, numbering as the spine, hairlines, tabular numerals — still holds. This document records only what *changes* when the page becomes a **sheet** read across a room, plus the grammar that turns source material into a deck.

---

## North star

> **A sheet is a datasheet page blown up for the wall — one claim, one exhibit, read in five seconds.**
> The room should *get* each sheet before the presenter finishes its first sentence.

---

## The canvas

- Every sheet is a fixed **1280×720** canvas, `transform: scale()`-fitted to the viewport by the engine — one scale factor for the whole sheet, so the layout never reflows.
- The sheet sits on a **dark stage** (`#141414`); the sheet edge against the stage does the framing — no crop marks.
- Hairlines are **1px logical and scale with the sheet**, holding their visual weight at projection. `--rule-heavy` is 3px here (2px reads thin at distance).

## Type at distance

Same tokens, new scale — the only new thing in the stylesheet. **28px body floor; nothing below 16px.**

| Token | px | Datasheet was | Carries |
|---|---|---|---|
| `--s-meta` | 16 | 11 | furniture, labels, captions' labels |
| `--s-small` | 20 | 13 | table body, captions, panel prose |
| `--s-body` | 28 | 14 | slide body — the working voice |
| `--s-lead` | 38 | 18 | the one-claim statement, ≤ 24ch |
| `--s-title` | 56 | 28 | sheet titles |
| `--s-display` | 96 | 48 | title sheet only |

## Furniture

- **Title sheet only** carries the full gridded titleblock: Doc, Rev, Date, Owner, `Sheet 01 of NN`.
- **Every content sheet** carries exactly one header line — `§ id + section name` left, `DOC-ID · n / N` right — over a 1px ink rule. No footer.
- The progress bar is viewport-fixed under the stage, 4px, **paper-on-dark** — never red.

## Two numbering axes

The `§` spine numbers the *content* — sections, `Fig. 3.2`, `Tbl. 2.1`, panel `6.1` — exactly as in the datasheet. Sheet *position* is `n / N` in the header. They never compete.

## Red budget

**≤ 2 uses of `--color-signal` per sheet**: the `§` id, plus at most one emphasis — a `.pick` table row, a figure's callout balloons, an `end-list` index. When in doubt, ink or muted.

## Metadata

Doc IDs are **`<PROJ>-SLD-NNN`**. Revisions are **letters** (`A`, `B`, …) — decks revise like drawings. ISO date, owner initials, sheet count. Vestigial values still ship; their presence is what makes the deck a document.

---

## The deck grammar

How source material becomes sheets. This is the skill's planning contract.

### Skeleton

Title sheet → *(index sheet, only at ~5+ content sheets)* → content sheets → closing sheet. The closing sheet is **takeaways + asks**: a mono `end-list` of `T-n` takeaways plus any `A-n` decisions the audience must act on; with nothing to ask, plain END OF DECK furniture.

### Density ceiling: lead + one exhibit

A content sheet is **one claim** — the lead, ≤ 38px, ≤ 24ch — plus **at most one exhibit** proving it. Two exhibits means two sheets. A full-bleed exhibit may stand alone when it *is* the claim.

### Form mapping

to-report's mapping, re-scoped to the sheet — prose paragraphs don't exist on sheets:

| Content | Exhibit | Ceiling |
|---|---|---|
| comparison | `data-table` | ≤ 6 rows |
| flow / architecture / sequence | SVG figure — flowchart for logic, schematic for structure | one figure |
| magnitude | single-hue ink-bar chart | one chart |
| option / state set | `status-list` | ≤ 6 items |
| rationale, intent | the lead itself | one sentence |

### Fragments: sequence builds, evidence lands whole

Fragments are reserved for content the presenter walks through item-by-item — status lists, takeaway/ask lists, step sequences. Leads, tables, figures, charts and panel pairs always appear whole. Rule of thumb: **if you'd say "next…" aloud between items, it builds.**

### Mining: the argument, not the record

A deck argues; a datasheet records. Mine the source for the argument: name the **through-line** (one sentence the room must take away), extract one claim per sheet ordered to carry it, pick the single strongest exhibit per claim. Leftovers are dropped or pointed at a to-report datasheet — the deck is not the archive.

---

## A working test

Before presenting a new deck, ask:

1. Does every content sheet carry exactly one header line — `§` id, section name, `DOC-ID · n / N`?
2. Is every sheet one claim + at most one exhibit, with nothing below 16px?
3. Could the room get each sheet in five seconds — or is a sheet carrying prose that should be an exhibit?
4. Does the title sheet carry the full titleblock (`<PROJ>-SLD-NNN`, rev letter, date, owner, sheet count), and the closing sheet takeaways + asks?
5. Is signal red at ≤ 2 uses per sheet, and never on the progress bar?
6. Do fragments appear only where you'd say "next…" aloud?

If the answer to any of these is no, fix it before presenting.
