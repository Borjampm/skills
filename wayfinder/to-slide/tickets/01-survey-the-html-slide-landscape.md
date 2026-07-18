# Survey the HTML slide landscape

- Status: closed (2026-07-18)
- Type: `wayfinder:research` (AFK)
- Assignee: research subagent (fired at charting, 2026-07-18)
- Blocked by: —

## Question

How are HTML slide decks actually built, and which route fits this design system? Survey the landscape — reveal.js and the other serious options (impress.js, Slidev, bespoke.js, hand-rolled CSS/JS) — and for each answer:

- markup conventions: what a "slide" is in the DOM, how content is authored;
- scaling and aspect ratio: how a fixed slide (e.g. 16:9) fits arbitrary screens;
- keyboard/click navigation and slide-number/progress display;
- incremental reveals (fragments): how per-element builds are authored;
- theming: how hard it is to impose a fully custom design system (fonts, colors, layout) — can the engine's own look be erased completely?
- single-file story: can a deck be one self-standing HTML file (library inlined or CDN-loaded), and what breaks offline;
- weight and health: bundle size, maintenance status.

Deliverable: a comparison with a recommendation for this use case — full aesthetic control under the personal design system, keyboard nav + progress + fragments as V1 features, decks as standalone files.

## Resolution

Full findings: [`assets/html-slide-landscape.md`](../assets/html-slide-landscape.md) (researched 2026-07-18, Opus subagent).

Gist:

- **Fundamentals**: a slide is a full-screen box shown one at a time; the fixed-16:9 problem is solved by `transform: scale(min(vw/W, vh/H))` (dominant, pixel-exact) or viewport units (simpler, reflows); `#/N` URL-hash-per-slide is the convention; a from-scratch deck with keyboard nav + fragments + progress is ~60–80 lines of vanilla JS.
- **reveal.js 6.0.1** (Apr 2026, MIT): the standard; scaling/nav/progress/`.fragment` built in, but its themes actively fight a custom design system and it injects transform styles.
- **Slidev 52.x**: superb authoring but a Vite/Vue build emitting a multi-MB SPA, not one standalone `.html` — disqualified by the single-file contract.
- **impress.js 2.0.0** (Jul 2022): 3D/Prezi paradigm — wrong for a sober datasheet look.
- **bespoke.js 1.1.0**: right minimalist philosophy but dormant since ~2015.
- **Recommendation**: hand-roll a ~70-line vanilla JS/CSS template the skill emits — a reductive aesthetic wants an empty stylesheet as substrate, and every library makes you subtract its opinions; cleanest single-file/offline story, zero rot risk. **Runner-up**: reveal.js on a blank theme, the fallback if hand-rolled scaling proves fiddly or speaker notes / PDF export later become requirements.
