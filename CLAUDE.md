# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file HTML widget (`index.html`) hosted on GitHub Pages and embedded via `<iframe>` on the 360Learning platform homepage. No build tools, no framework, no dependencies beyond a Google Font.

## Deploying changes

```bash
git add index.html
git commit -m "description"
git push
```

GitHub Pages rebuilds automatically. Allow ~30 seconds for changes to go live at `https://leeeccles.github.io/360learning-hero-widget/`.

## Architecture

Everything lives in `index.html`: HTML structure, CSS `<style>` block, and a small inline `<script>`. The headshots are in `Circle Headshots copy/` and referenced via relative paths from `index.html`.

**Layout:** CSS Grid with `grid-template-columns: 5fr 3fr` and a fixed `height: 320px`. Both columns are always the same height — no responsive breakpoints that collapse to single column (360Learning's sidebar makes the content area narrower than expected).

**Left column** is a flex column with a `.hero-spacer` (flex: 1, min-height: 12px) between the top content block and the bottom contributor stream block. This absorbs leftover vertical space without creating dead white space.

**Right column** has two benefit cards, each `flex: 1`, so they always fill the column height exactly. Each card is `flex-direction: column; justify-content: center` with a `.benefit-inner` row inside for the icon + text layout.

**Contributor stream** is a CSS `scrollLeft` animation on a duplicated card list (32 cards = 16 × 2) so the loop is seamless. The `.stream-viewport` uses `mask-image` for edge fading and `overflow: hidden` to clip the animation.

## 360Learning embedding rules

- `<script>` tags are **blocked** in 360Learning HTML widgets — do not add scripts to the embed code
- `<iframe>` sources must use HTTPS
- Inline CSS and `<style>` tags are allowed

**Embed code:**
```html
<iframe
  class="lw_hero_widget"
  src="https://leeeccles.github.io/360learning-hero-widget/"
  frameborder="0"
  scrolling="no"
  allowtransparency="true">
</iframe>

<style>
.lw_hero_widget {
  border: none;
  width: 100%;
  height: 335px;
  display: block;
  background: transparent;
}
</style>
```

Adjust `height` in the embed if widget content changes. Rule of thumb: `hero-inner` height + 4px (body top padding) + 8px (body bottom padding) + ~3px buffer.

## Key constraints to preserve

- `html, body` must stay `background: transparent` — the widget sits on the platform's own background
- `overflow: hidden` on `body` (not `html`) prevents scrollbars without clipping card shadows
- `body` padding (`4px 12px 8px 0`) gives card shadows breathing room — remove it and shadows clip
- `min-width: 0` on both grid children prevents content from blowing out column widths
- No `aspect-ratio` on the iframe — our widget has fixed-height content, not aspect-ratio-driven content, so it gives wrong heights at different widths
- No responsive breakpoints that stack columns — keep two columns at all widths

## Brand tokens

| Token | Value |
|---|---|
| Brand green | `#00b28a` |
| Brand green hover | `#009e7a` |
| Dark text | `#111827` |
| Body text | `#6b7280` |
| Muted / meta | `#9ca3af` |
| Font | Plus Jakarta Sans (Google Fonts), weights 400/500/600/700/800 |
