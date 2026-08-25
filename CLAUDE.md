# CLAUDE.md — Komini Construction website

Guidance for AI assistants working in this repository.

## What this is

A single-page marketing site for **Komini Construction**, a drywall and
interior-finishes company working in Messinia and the Peloponnese, Greece.
It is a static site with no build step, no framework, and no dependencies:
three things ship — `index.html`, `style.css`, and `images/`.

```
index.html    the entire page: markup + inline <script> at the bottom
style.css     all styling, including an appended mobile/layout override block
images/       photography, grouped by project
README.md     one line
```

## Running it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

There is nothing to install, compile, or bundle. Do not add a package
manager, bundler, or framework unless the user explicitly asks.

## Page structure

`index.html` is one `<main>` with sections in this order, each anchored by
`id` for the nav links: `#home` (hero), `#services`, `#work` (project rail),
the before/after `.comparison` section, `#about`, `.trust`, `#contact`.
A `.site-header` with the brand, a `.menu-toggle` hamburger and `#site-nav`
sits above; `.site-footer` closes the page.

The inline script at the bottom of `index.html` does four things and nothing
else:

1. **Mobile nav** — toggles `aria-expanded` on `.menu-toggle` and `.is-open`
   on `#site-nav`; closes on any link click.
2. **Before/after comparison slider** — pointer capture on `#compare` plus
   arrow-key support on `#compare-handle`, driving the width of
   `#after-image` and keeping `aria-valuenow` in sync.
3. **Contact form** — `#contact-form` never posts anywhere; it builds a
   `mailto:` URL to `komini.construction@gmail.com` and updates `#form-note`.
   There is no backend.
4. Nothing else — keep new behavior small and vanilla, in the same script
   block, in the same style.

`.reveal` classes appear in the markup as animation hooks; check `style.css`
before assuming any JS drives them.

## CSS conventions

- `style.css` uses `@layer reset, theme, components` and CSS custom
  properties (e.g. `--ink`, `--max`) declared in the theme layer.
- Fonts are Google Fonts (`DM Sans` body, `Space Grotesk` display), loaded via
  `<link>` with `preconnect`. Keep the preconnects if you touch the head.
- Breakpoints in use: `900px` and `700px`.
- The **top of the file** is a later-added override block (project rail forced
  to a grid, `.rail-controls` hidden, `overflow-x: hidden` on body) that
  deliberately uses `!important` to beat the original rail styles further
  down. When changing the projects grid, edit that block rather than adding a
  third competing rule set — and be aware the arrow buttons (`#prev`/`#next`)
  are currently hidden by it, so their markup is inert.

## Images

`images/` is grouped by project: `BANKS/` (with `AlphaBankNafplios`,
`AlphaBankTripoli`, `NationalBankKalamata`), `GermanDoc/`, `HOME/`,
`Mixalis/`, `OvalVilla/`, `RANDOMS/`. Filenames are original camera names
(`20180310_101025.jpg`) — keep them as-is when referencing; renaming breaks
existing paths for no benefit.

When adding an `<img>`, match the existing pattern: explicit `width`/`height`,
a descriptive `alt`, `loading="lazy"` below the fold, `sizes` for rail cards,
and `fetchpriority="high"` only on the hero image.

## Content and accessibility

- Copy is English, understated, and specific to the trade. Preserve the tone:
  short lines, `<em>` used for the second line of headings, `.eyebrow` labels
  above each section heading.
- The site is marketing material for a real business — do not invent client
  names, project counts, years in business, or testimonials. The existing
  figures ("Since 1996", "29+ years", named banks and the Kalamata Municipal
  Theatre) are the company's own claims; leave them unless asked to update.
- Keep the accessibility affordances already present: `aria-label` on the
  brand and rail buttons, `aria-expanded`/`aria-controls` on the menu toggle,
  the `role="slider"` contract on the compare handle, and labelled form
  inputs.

## Git workflow

Default branch is `main`. Commits here are short and informal
(`v2`, `mobile fix`) — a clear descriptive message is still preferred. Push
to the branch you were assigned; do not open a pull request unless asked.
