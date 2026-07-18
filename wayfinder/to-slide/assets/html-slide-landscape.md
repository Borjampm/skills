# HTML Slide-Deck Landscape

Research for the **to-slide** skill: turn source material into a standalone HTML deck
styled in a strict engineering-datasheet design system (IBM Plex Sans/Mono, warm paper
background, near-black ink, one signal red, 1px hairlines, no rounded corners, no shadows,
numbered sections). Deck is presented live in a browser. V1 must-haves: keyboard/click
navigation, slide-number/progress display, incremental reveals (fragments). A library
dependency is fine; decks are single standalone `.html` files.

_Compiled 2026-07-18. Versions/dates noted per tool; primary sources linked inline._

---

## Executive summary

| Approach | Authoring | Slide = | Scaling | Nav / progress | Fragments | Theming freedom | Single-file | Weight | Health |
|---|---|---|---|---|---|---|---|---|---|
| **reveal.js 6.0.1** | HTML `<section>` or Markdown | `<section>` | Built-in `transform: scale` | Built-in | Built-in `.fragment` | High, but styles fight you | Yes, CDN or inlined | ~120 KB JS + ~7 KB CSS (min) | Healthy; v6.0.1 ~Apr 2026, MIT |
| **Slidev 52.x** | Markdown + Vue | `---`-separated MD block | Built-in (fixed canvas) | Built-in | `v-click` directives | High, but needs a build | **No** (build → SPA/bundle) | Multi-MB toolchain | Very healthy; v52.18, MIT |
| **impress.js 2.0.0** | HTML `<div class="step">` | positioned `.step` div | 3D camera transforms | Built-in | `.substep` (add-on) | Medium; geared to 3D | Yes, CDN or inlined | ~40 KB JS (min) | Low activity; 2.0.0 Jul 2022, MIT |
| **bespoke.js 1.1.0** | HTML `<section>` + plugins | `<section>` | `bespoke-scale` plugin | Plugins | `bespoke-bullets` plugin | Very high (near-zero CSS) | Yes, but needs bundling | ~1 KB core + plugins | **Dormant**; 1.1.0 ~2015, MIT |
| **Hand-rolled** | You write the HTML | Whatever you define | You write ~15 lines | You write ~30 lines | You write ~10 lines | **Total** (no engine CSS) | Yes, trivially | ~0 (your code only) | You own it |

**Bottom line up front:** for total aesthetic control under a strict design system, the
strongest fit is a **thin hand-rolled deck** (a ~60-80 line JS + CSS template the skill
emits), with **reveal.js on a stripped/blank theme** as the pragmatic runner-up. Full
argument in the [Recommendation](#recommendation).

---

## Fundamentals for a newcomer

If you have never built HTML slides, four ideas explain 90% of every tool below.

### 1. A "slide" is just a full-screen box that you show one at a time

An HTML slide deck is one web page containing many slide elements (often `<section>` or
`<div>`). Exactly one is visible ("active") at a time; the rest are hidden
(`display:none`, moved off-screen, or `opacity:0`). "Navigation" means changing which one
is active. That's the whole trick. Everything else — transitions, scaling, progress bars —
is decoration on top of "show element N, hide the others."

### 2. The scaling problem: a fixed slide on an arbitrary screen

Slides are designed at a **fixed logical size** and a **fixed aspect ratio** — almost
always 16:9 (e.g. 1280×720 or 1920×1080; reveal.js defaults to an unusual 960×700, roughly
4:3-ish, configurable). But screens vary wildly: a 27" monitor, a laptop, a projector at
1024×768, a phone. You want the slide to fill as much of the screen as possible **without
distorting** — a circle must stay a circle, text must not stretch. This is "fit a fixed
rectangle inside a variable rectangle, preserving aspect ratio" (the same math as
`object-fit: contain`).

Two families of solution:

- **`transform: scale()` (the dominant approach).** Lay the slide out at its fixed logical
  size (say 1280×720) using normal pixels. Then compute a single scale factor
  `s = min(screenWidth / 1280, screenHeight / 720)` and apply `transform: scale(s)` to the
  whole slide, plus a translate to center it. Everything inside — font sizes, gaps,
  hairline widths — scales together, so the layout is pixel-identical on every screen, just
  bigger or smaller. reveal.js, Slidev, and bespoke-scale all do exactly this. The one
  gotcha: 1px hairlines scale too, so at large scale factors a `1px` border becomes 2-3
  physical pixels — usually fine, occasionally worth pinning with `1px / s`.

- **Viewport units / responsive layout (`vw`, `vh`, `vmin`, `clamp()`).** Instead of a
  fixed pixel canvas, size everything relative to the viewport (e.g. `font-size: 4vmin`).
  No transform needed; the browser reflows. Simpler and more "webby," but the layout can
  shift between screens (a line that fits on one aspect ratio wraps on another), so it's
  less predictable for a designed deck. Good enough for text-forward decks; the transform
  approach is safer when you want pixel-exact control (which a strict design system wants).

The **transform-scale** method is what makes an HTML deck behave like PowerPoint/Keynote:
you design once at one size and it renders identically everywhere.

### 3. The URL-hash-per-slide convention

Most engines put the current slide index in the URL hash: `deck.html#/3` or `#/4/2` for
nested slides (reveal), or `#5` (bespoke-hash). Two reasons: (a) **deep-linking / reload** —
refresh the page and you stay on slide 3 rather than jumping to slide 1; (b) **history** —
back/forward buttons step through slides. It's cheap to implement (`location.hash` +
a `hashchange` listener) and worth doing even in a hand-rolled deck.

### 4. Fragments (incremental reveals)

A "fragment" is a piece of a slide that appears on a later key-press rather than all at
once — bullet-by-bullet reveals, a punchline that lands after a beat. The universal model:
mark elements as fragments (a class or directive), start them hidden, and treat each
fragment as an extra "step" so pressing → reveals the next fragment before finally
advancing to the next slide. Reveal uses `class="fragment"`; Slidev uses `v-click`;
bespoke uses a bullets plugin. Hand-rolled, it's a few lines: collect
`slide.querySelectorAll('.fragment')`, track how many are shown, reveal one per keypress.

### 5. Print / PDF export (noted, out of scope)

Every serious engine offers a way to export to PDF (reveal via a `?print-pdf` query + the
browser's print dialog; Slidev via `slidev export`). It matters because
`transform: scale` + `100vh` slides do **not** print correctly by default — you need a
print stylesheet that lays each slide on its own page. **Out of scope for this research;**
just know it's a solved-but-fiddly problem you'll revisit if PDF handoff is ever required.

### 6. What a minimal hand-rolled deck actually costs

Concretely, a from-scratch deck that meets all three V1 must-haves is small:

- **Navigation** (next/prev on arrows, space, click; clamp at ends; update `.active`
  class): ~25-30 lines JS.
- **Fragments** (reveal within-slide before advancing): ~10-15 lines JS, folded into the
  same next()/prev() functions.
- **Progress + slide number** (`current / total`, a width-% bar): ~5-10 lines JS.
- **Scaling** (transform-scale on resize): ~10-15 lines JS.
- **URL hash** (optional but easy): ~5 lines.

Call it **~60-80 lines of vanilla JS** plus your CSS. No dependencies, no build step, no
engine styles to fight. This is the number that makes the hand-rolled route viable for a
strict design system — see the recommendation.

---

## reveal.js

**Primary sources:** <https://revealjs.com> · <https://github.com/hakimel/reveal.js>
**Version:** 6.0.1 (latest; published ~April 2026). **License:** MIT. The de-facto standard
HTML deck framework, ~68k GitHub stars, actively maintained by Hakim El Hattab.

### 1. Slide = DOM element & authoring
A slide is a `<section>`. Required nesting is strict: `.reveal > .slides > section`. Each
`<section>` is one horizontal slide; nesting `<section>`s inside a `<section>` creates
**vertical** sub-slides (a 2D grid). Content inside is ordinary HTML.
Markdown authoring is supported two ways: `<section data-markdown>` blocks with the markdown
plugin, or an external `.md` file via `data-markdown="slides.md"` (the latter breaks the
single-file story — see #6). Minimal deck:

```html
<div class="reveal"><div class="slides">
  <section>Slide 1</section>
  <section>Slide 2</section>
</div></div>
```

### 2. Scaling & aspect ratio
Built-in and automatic. You set a logical `width`/`height` (defaults **960×700**),
`margin` (default 0.04), and `minScale`/`maxScale` (0.2–2.0) in the config. reveal applies
a uniform `transform: scale()` to fit any viewport without changing aspect ratio or layout —
exactly the transform-scale method from Fundamentals #2. `Reveal.layout()` forces a
recompute. This is a genuine strength: you design at one size, it fits everywhere for free.

### 3. Navigation & progress
Fully built in. Arrow keys, space, Page Up/Down, touch/swipe, on-screen control arrows,
`Esc` overview mode, `?`-triggered keyboard help. `progress: true` shows a progress bar;
`slideNumber: true` (with format options like `'c/t'`) shows the slide number. All three
V1 must-haves are config flags, not code.

### 4. Fragments
The reference implementation of the pattern. `class="fragment"` on any element makes it
reveal on the next step; default is fade-in. Rich variants: `fade-up`, `fade-in-then-out`,
`grow`, `shrink`, `highlight-red`, `strike`, etc. Order overridable with
`data-fragment-index`. Fires `fragmentshown`/`fragmenthidden` events. This is the model
other tools imitate.

### 5. Theming — how far can you erase the default look?
Theming is CSS-file swapping: 12 built-in themes (black, white, league, …). Two custom
routes: (a) override the `:root` CSS custom properties the themes expose, or (b) **start
from a blank stylesheet and build up** — the docs explicitly bless this. So in principle
you can impose any design system.

**Where it fights you** (important for a strict system):
- reveal ships `reveal.css` (layout/positioning, essential) separately from the theme CSS
  (looks, discardable). You keep the former, drop the latter, and write your own — that's
  the clean path.
- Theme CSS is opinionated: several themes force `text-transform: uppercase` on headings
  and set letter-spacing, so a half-measure (keeping a theme and patching it) means fighting
  specificity. The fix is not to patch — replace the theme entirely.
- reveal injects some **inline styles** on the scaling wrapper (the `transform`, width,
  height on `.slides` and slide sections). You must not fight those — they're the scaling
  engine. Your design system lives *inside* the slide, not on the transform wrapper.
- Structural class names (`.reveal`, `.slides`, `.fragment`, `.present`, `.past`,
  `.future`) are required and your CSS must coexist with them.

Net: with a blank theme, near-total visual control is achievable, but you're authoring
against reveal's class names and its scaling wrapper rather than a clean slate. Verdict:
**high control, moderate friction.**

### 6. Single-file story
Yes. The standard `index.html` links `reveal.css`, a theme, and `reveal.js`. You can point
those at a CDN (`cdn.jsdelivr.net/npm/reveal.js@6/...`) → a genuinely standalone `.html`
that works anywhere **with a network connection**. To be fully offline/portable, inline the
CSS in a `<style>` and the JS in a `<script>` (paste the minified dist, ~120 KB) — one file,
zero external requests. **What breaks offline:** CDN links (obviously), external
`data-markdown="slides.md"` files, the highlight/math/notes **plugins** if loaded from CDN,
and any web-font `@import` (you'd inline or self-host IBM Plex). **Weight:** core
`reveal.js` min ≈ 120 KB (~35 KB gzipped); `reveal.css` min ≈ 7 KB; a theme ≈ 5-8 KB.

### 7. Health
Excellent. v6 line current (6.0.1 ~Apr 2026), regular releases, huge ecosystem, MIT. The
safest long-term bet of the batch.

---

## Slidev

**Primary sources:** <https://sli.dev> · <https://github.com/slidevjs/slidev>
**Version:** 52.18.0 (latest ~July 2026; ships as a monthly-ish major cadence).
**License:** MIT. Vite + Vue 3 powered, "presentation slides for developers." ~40k stars,
very active.

### 1. Slide = ? & authoring
Authoring is **Markdown-first**: one `slides.md` file, slides separated by `---`.
Frontmatter per slide sets layout/class. You can drop into raw HTML and, uniquely, embed
**Vue components** for interactivity. At runtime a slide compiles to a Vue-rendered DOM
node — you don't hand-author the slide DOM, the toolchain generates it.

### 2. Scaling & aspect ratio
Fixed canvas (default 980×552, 16:9-ish), auto-scaled to the viewport by the framework —
same transform-scale idea, handled for you. Aspect ratio configurable in `config`.

### 3. Navigation & progress
Fully built in, and then some: keyboard/click nav, an overview, a presenter mode with
notes and a second-screen timer, a slide-number display, a nav bar. Best-in-class out of
the box.

### 4. Fragments
`v-click` directives: `<div v-click>` reveals on next step; `v-clicks` wraps a list;
`v-after`, `v-click="3"` for ordering; `.click` motion helpers. Powerful, but it's a
Vue-directive model — it only exists because there's a Vue runtime and a build step.

### 5. Theming
Very flexible: npm themes, UnoCSS/Windi utility classes, per-slide `class:`, global CSS,
Vue component overrides. You can absolutely impose a design system — arguably the most
*capable* theming here. IBM Plex + custom colors + hairlines are all straightforward via
UnoCSS config and a `style.css`.

### 6. Single-file story — the dealbreaker
**No true single self-standing `.html`.** Slidev is a build tool: you author `slides.md`,
run `slidev build`, and get a **static SPA** — a folder of HTML + JS + CSS + hashed asset
chunks (a Vue/Vite bundle, multiple MB uncompressed). It's hostable and self-contained *as
a directory*, but it is not one portable `.html` file you save anywhere and double-click
offline without a build. There is an SPA "single-page" export, but it still emits a bundle,
not a lone inlined file. For a skill whose contract is "emit ONE standalone `.html`," this
is disqualifying. **Weight:** not a KB-per-library number — it's a whole toolchain
(Node, Vite, Vue) plus a multi-MB output bundle.

### 7. Health
Excellent — extremely active, v52.x, MIT. Health is not the issue; the artifact shape is.

---

## impress.js

**Primary sources:** <https://impress.js.org> · <https://github.com/impress/impress.js>
**Version:** 2.0.0 (released **22 July 2022**; prior 1.1.0 April 2020). **License:** MIT
(2011-2023, Bartek Szopka + Henrik Ingo + 70 contributors). ~38k stars.

### 1. Slide = DOM element & authoring
A step/slide is `<div class="step">`, positioned in an infinite 2D/3D canvas via data
attributes: `data-x`, `data-y`, `data-z`, `data-rotate`, `data-scale`. Authoring is HTML;
no built-in Markdown. The mental model is **a camera flying around a big canvas of
positioned slides**, Prezi-style, rather than a linear stack.

### 2. Scaling & aspect ratio
Different paradigm: instead of fitting a fixed slide to the viewport, impress moves a 3D
**camera** (CSS3 transforms) over positioned steps. v2.0.0 bumped the default target
resolution to 1920×1080. You *can* build "Classic Slides" (all steps same size, laid in a
row) that behave like a normal deck, but you're opting out of the feature the library
exists for. Aspect handling is more manual than reveal/Slidev.

### 3. Navigation & progress
Built-in keyboard (arrows, space, tab) and click-to-step navigation, touch. Progress bar
and slide numbers are **not** first-class — commonly added via the `impress-console` or a
custom plugin. Weaker on the exact V1 "progress display" must-have than reveal/Slidev.

### 4. Fragments
Supported as **substeps**: `class="substep"` elements appear incrementally within a step
(v2.0.0 added custom substep ordering). Functional but less rich/documented than reveal
fragments.

### 5. Theming
You control step CSS freely, so a design system is imposable. But impress's reason for
being — 3D zoom/rotate transitions — is at odds with a sober engineering-datasheet
aesthetic (no shadows, no flourish). Using impress and then suppressing all its motion is
using the wrong tool. Medium fit.

### 6. Single-file story
Yes — one HTML file plus `impress.js` (CDN or inlined). **Weight:** ~40 KB min JS, no
required CSS framework. Offline: inline the script, self-host fonts.

### 7. Health
Low but alive: last release 2.0.0 (Jul 2022), 423 commits, MIT. Not abandoned, but not
briskly maintained. Fine as a stable dependency; wrong paradigm for this design system.

---

## bespoke.js

**Primary sources:** <https://github.com/bespokejs/bespoke> ·
<https://www.npmjs.com/package/bespoke>
**Version:** 1.1.0 (npm; last published ~2015, **~11 years ago**). **License:** MIT.
Author: Mark Dalgleish.

### 1. Slide = DOM element & authoring
"DIY presentation micro-framework." A slide is a `<section>` inside a parent element.
Authoring is HTML by default; Markdown via the community `bespoke-markdownit` plugin. The
core is deliberately tiny — **~1 KB min+gzip** — and does almost nothing on its own:
it wires up slides, exposes a control API, and emits events. Everything else is a plugin.

### 2. Scaling
Not in core. The `bespoke-scale` plugin does the transform-scale-to-fit. Opt-in.

### 3. Navigation & progress
All plugins: `bespoke-keys` (keyboard), `bespoke-touch` (swipe), `bespoke-hash` (URL hash),
`bespoke-progress` (progress bar), `bespoke-classes` (state classes). You compose exactly
the behaviors you want.

### 4. Fragments
`bespoke-bullets` plugin: steps through child elements (or elements matching a selector)
within a slide. That's the fragment model.

### 5. Theming — the big draw
**Near-total.** The core ships essentially no visual CSS, so there is almost nothing to
erase or override — the opposite of reveal's opinionated themes. This is the best
*theming-freedom* profile of any library here: a blank canvas with a nav engine bolted on.
For a strict design system, that's ideal in principle.

### 6. Single-file story
Achievable but awkward. bespoke and its plugins are npm modules authored as CommonJS/ES
modules — the intended workflow is a **bundler** (Browserify/Webpack/Rollup, via
`generator-bespoke`). To get one standalone `.html` you'd bundle core + chosen plugins into
one script and inline it. Doable, but it reintroduces a build step the hand-rolled route
avoids. **Weight:** ~1 KB core + a few KB across plugins — tiny.

### 7. Health — the disqualifier
**Effectively dormant.** Core last published ~2015; plugins similarly stale. MIT and simple
enough that it still works, but depending on an unmaintained micro-framework — when its main
value (tiny + unopinionated) is something you can hand-roll in ~70 lines — is hard to
justify in 2026. Instructive as a design reference more than a live dependency.

---

## Hand-rolled (vanilla HTML/CSS/JS)

No library. You write the deck and a small script. Two sub-variants:

### Variant A — JS-driven slide switching (recommended sub-variant)
Slides are `<section>`s; JS toggles an `.active` class (all others hidden). This is the
same architecture reveal/bespoke use, minus the framework. You own:
- **Scaling:** ~15 lines — compute `min(vw/W, vh/H)`, set `transform: scale()` on a wrapper,
  recompute on `resize`.
- **Navigation:** ~30 lines — `keydown` (arrows/space), `click`, clamp indices, swap active
  class, update hash.
- **Fragments:** ~10 lines — per slide, `querySelectorAll('.fragment')`, reveal one per
  step before advancing.
- **Progress/number:** ~10 lines — set a bar width and a `n / total` label on each change.

Total **~60-80 lines JS** + your CSS. All three V1 must-haves, zero dependencies, one
`.html` file, and **not a single engine style to override** — the design system is the
*only* CSS present.

### Variant B — CSS scroll-snap (mostly no JS)
A scroll container with `scroll-snap-type: y mandatory`; each slide is `height:100vh;
scroll-snap-align:start`. The browser snaps between slides on scroll/PageUp-Down natively —
a full deck with near-zero JS (see CSS-Tricks' "CSS Scroll Snap Slide Deck," Yihui Xie's
"Snap Slides"). **Trade-offs for this project:** scroll-snap gives you free scaling-by-reflow
(uses viewport units, less pixel-exact than transform-scale), but arrow-key nav still wants
a few lines of JS to feel right, and **fragments are not natural** in a pure-scroll model —
you'd bolt JS back on for incremental reveals. Elegant for text decks; the fragment
requirement pushes you back toward Variant A. Good to know it exists; Variant A is the
better base here.

### Theming
Total control by definition — there is no framework CSS. IBM Plex, paper background, signal
red, 1px hairlines, numbered sections: all authored directly with nothing competing on
specificity and no injected inline styles except the ones you write.

### Single-file / weight / health
One `.html`, everything inline, works offline (self-host/inline the fonts). Zero library
weight. "Maintenance health" = your own code — which for ~70 lines is a feature, not a risk.
The cost is that you implement (and test) the scaling math and edge cases (rapid keypresses,
resize during transition, hash-on-load) yourself, and you get no free extras (overview mode,
speaker notes, PDF export) unless you add them.

---

## Recommendation

### Primary: hand-rolled JS-driven deck (Variant A), emitted by the skill as a template

For **total aesthetic control under a strict design system**, the hand-rolled route is the
best fit, and the design-system constraint is precisely why. Every library here carries CSS
and DOM assumptions you'd spend effort *subtracting*: reveal's opinionated themes and
injected transform styles, impress's 3D paradigm, Slidev's build output, bespoke's plugin
wiring. A datasheet aesthetic — hairlines, no shadows, no rounded corners, exact type — is a
*reductive* design: the ideal substrate is an empty stylesheet, and hand-rolled is the only
option that starts there. The V1 feature bar is low enough to clear cheaply: keyboard/click
nav, progress/number, and fragments are **~60-80 lines of vanilla JS** combined (see
Fundamentals #6). Because a skill emits this template **once** and reuses it for every deck,
that cost is paid a single time, not per presentation — the economics strongly favor owning
the ~70 lines. You also get the cleanest possible single-file story (everything inline, real
offline support, zero library weight) and no dependency that can rot.

The decisive trade-off you accept: you implement the scaling math and its edge cases
yourself and forgo free extras (overview mode, speaker notes, built-in PDF export). For this
skill's scope those extras are non-goals, and the scaling math is a well-understood
~15-line function.

### Runner-up: reveal.js 6.0.1 on a blank (theme-less) stylesheet

If hand-rolling the nav/scaling proves fiddlier than budgeted, or if speaker notes / robust
PDF export / battle-tested cross-browser scaling later become requirements, **reveal.js** is
the pragmatic fallback. It's MIT, actively maintained, single-file-able via CDN or inlining
(~120 KB), and its docs explicitly support **starting from a blank theme** — so you can get
most of the way to full control. The decisive reasons it's runner-up rather than the pick:
(1) you author against its required class names and its scaling wrapper's injected inline
styles rather than a clean slate, and (2) its default themes actively fight a custom system
(forced `text-transform`, letter-spacing, specificity battles) unless you fully discard the
theme layer — meaning much of reveal's value (its themes) is exactly what you'd be throwing
away. You'd be using ~15% of the framework and overriding the rest.

### Why not the others
- **Slidev** — best-in-class features and theming, but **cannot emit one standalone
  `.html`**; it's a Vite/Vue build producing a multi-MB SPA. Disqualified by the single-file
  contract, not by quality.
- **impress.js** — wrong paradigm (3D Prezi-style camera) for a sober datasheet look; you'd
  suppress its defining feature and still lack first-class progress display.
- **bespoke.js** — the right *philosophy* (tiny, unopinionated, maximal theming freedom) but
  **dormant since ~2015** and needs a bundler for one-file output. Its entire value is
  reproducible in the ~70-line hand-rolled template without an unmaintained dependency —
  which is effectively what the primary recommendation is.

### One-line decision
**Hand-roll a ~70-line vanilla template for full control and a clean single file; keep
reveal.js (blank theme) in your back pocket if you later need speaker notes, PDF export, or
you want the scaling engine handed to you.**
