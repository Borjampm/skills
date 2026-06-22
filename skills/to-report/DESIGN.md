# DESIGN.md

The soul and guidelines of this design system. Colors, fonts, scales, and component classes live in `styles.css` — this document covers the posture and discipline that make a page *feel* right when those tokens are applied well, and *feel wrong* when they're applied lazily.

---

## North star

> **The page should feel like an instrument panel, not a webpage.**
> The reader should *scan it for information*, not *read it for understanding*.

A page from this system, glanced at across a room, should read as *technical document* — something produced for a reason, with someone's name on it, that could be folded and put in a binder. If it reads as *content marketing* or *a tasteful minimal template*, something has gone wrong.

---

## Principles

### 1. Engineered, not designed

The aesthetic comes from constraint and discipline, not from styling choices. Engineering documents look the way they look because they *have to* — they're structured to be referenced, not browsed.

Lean into the structural conventions that make technical documents technical: numbered sections, document IDs, revision marks, figure callouts, dimensioned diagrams, status indicators, signed approvals. These are not decoration. They *are* the design.

A page that looks merely "clean" or "minimal" has failed.

### 2. Numbering is the spine

Every section, panel, figure, and table is numbered. The number is always visible and set in mono.

- Sections: `§ 1.0`, `§ 2.0`
- Subsections / panels: `1.1`, `1.2`
- Figures: `Fig. 2.1`, `Fig. 4.2`
- Tables: `Tbl. 3.1`

Strip the numbering and the design loses its posture immediately. If a section feels like it doesn't deserve a number, it doesn't deserve to exist.

### 3. Mono carries data, sans carries prose

The mono / sans distinction is the system's grammar. The reader learns it within seconds and starts reading the page faster because of it.

**Mono** — every identifying or measurable thing:
- Doc IDs, revisions, dates, owners
- Section IDs, panel labels, figure refs
- Tokens, field names, types, units
- Status labels, version strings, counts, badges

**Sans** — every paragraph of human reading.

Violating this boundary cheapens the system. Prose set in mono for "engineer aesthetic" is illegible; a version number set in sans looks like a typo.

### 4. Density is a feature, not a problem

Webpages flow vertically through a single column of breathing prose. Engineering datasheets are panelized, gridded, and dense. A correct page may scroll, but it should feel *packed*, not airy.

- Default to grid layouts (`.grid-2`, `.grid-3`, `.grid-2-1`) over single columns
- Body text is small (14px), with tight leading — that's correct, not cramped
- The panel border *is* the breathing room; padding inside panels stays tight
- White space comes from rhythm and structure, not from generous margins

### 5. Show, don't explain

When you can choose between a paragraph and a table, choose the table. When you can choose between describing a flow and drawing it, draw it. Prose is reserved for the things data can't carry — rationale, intent, nuance.

- "The pipeline has 7 services" → a service inventory table
- "There are four status states" → a status-indicator list
- "The color tokens are…" → a table with swatches
- "Data flows through…" → a sequence diagram

### 6. Specificity is soul

The details that feel like overkill are the ones that make it real. A document without an ID, a revision, an issue date, an owner, and a sheet reference doesn't read as a document — it reads as a webpage trying to look like one.

Always include in the title block:
- A doc ID (project-type-number, e.g., `LMN-ARC-001`)
- A revision (`1.0`, `2.3`)
- An ISO date (`2026-05-17`)
- An owner (initials)
- A sheet reference (`Sheet 1 of 1`)

Even if the values are vestigial — a personal site, a single-page doc — their *presence* is what makes the page feel engineered.

### 7. One signal color, used decisively

The signal color (`--color-signal`) is reserved for things that genuinely demand attention:
- Section IDs (the `§ 1.0` markers)
- Panel-label numbers
- Callout-numbered balloons in diagrams
- Alert / fault status
- PK / FK marks in schemas
- "ACTIVE" / "FAULT" badges

If everything is red, nothing is. The discipline is what makes it work. When in doubt, leave it ink or muted.

### 8. Hairlines over heavy borders

Visual structure comes from thin, precise lines — not boxes-with-shadows, not soft cards.

- 1px soft rule for separators inside panels
- 1px ink for box borders and panel outlines
- 2px ink only for the most important dividers (doc head, section bars)
- Never use shadows, gradients, glows, or rounded corners

Restraint signals confidence. A page that uses three different border weights with discipline reads as more designed than a page that uses ten with abandon.

### 9. Tabular numerals always

Any numeric column, stat readout, ID, timestamp, or measurement uses `font-variant-numeric: tabular-nums`. Columns must align without manual fiddling. The reader's eye does this work unconsciously — they just feel that the page is "tighter" than other pages.

### 10. The page is a document, not a screen

Treat the page like printed output that happens to render in a browser:
- Page wrapper with corner crop marks
- Title block in the upper right, gridded with hairlines
- Section bars that include sheet / page references
- Footer with doc ID, revision, and `page X of Y`
- Drawn diagrams (not screenshots) with figure captions and, where useful, dimension lines

This framing tells the reader what posture to adopt.

---

## Failure modes

These are the ways the system falls apart. Watch for them.

- **The "tasteful minimal" trap.** A clean grayscale layout with a single accent and small caps is *not* this system — it's generic minimalism with a serif. The numbering, document IDs, panel labels, and diagrams *are* the design. Without them you're not "almost there"; you're somewhere else entirely.

- **Prose where data belongs.** Three full paragraphs explaining what could be a 4-row table. The first move on any new page should be to look for content that can become a table, a list, or a diagram.

- **Decorative engineering touches.** Adding `// CLASSIFIED // TOP SECRET` or `MIL-STD-1234` for vibes when it conveys no information. Every label must mean something — fictional but plausible is fine; pure costume is not.

- **Red leakage.** Using `--color-signal` for body links, headings, hover states, or decorative borders. Red earns its space. Don't spend it cheaply.

- **Soft borders, drop shadows, rounded corners, hover lifts.** These are webpage reflexes. Resist them. A corner that isn't sharp belongs on a different system.

- **Mono / sans bleed.** A version number set in sans, or a paragraph of prose set in mono. Keep the boundary clean even when the content is short.

- **Webpage rhythm.** Big hero, three feature cards, a CTA. If a page from this system would slot naturally into a marketing site without anyone noticing, it has lost its posture.

---

## When to break a rule

The rules exist to keep the system coherent; breaking one should be a deliberate, considered choice, not a slip.

- If a panel contains only one short value, the panel border may carry more weight than the content deserves. Use a `.field` or an inline mono pair instead.
- A section with no real subsections doesn't need a `1.1` panel — the section bar can introduce content directly.
- A title page or launch view may earn larger display type, or even the serif. Apply sparingly and never twice in the same document.

When in doubt, choose the more disciplined option. The system is more interesting when it stays tight than when it relaxes.

---

## A working test

Before shipping a new view, ask:

1. Does every section have a number, and is the number visible?
2. Is every identifier (IDs, dates, versions, counts) in mono?
3. Could any paragraph be replaced by a table, list, or diagram?
4. Does the page have a title block, a doc ID, and a footer with page reference?
5. Is the signal color used fewer than ~6 times on the page?
6. Would the page survive being printed in black-and-white?

If the answer to any of these is no, fix it before adding anything new.
