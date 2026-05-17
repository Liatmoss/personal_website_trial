# Homepage Card Layout Design

**Date:** 2026-05-17
**Status:** Approved

## Problem

The homepage thumbnail cards use a fixed `350px` width inside a flexbox container. This causes two issues:
1. Card text wraps mid-word when the label is longer than the fixed card width allows
2. The layout does not scale gracefully as more cards are added — flex-wrap produces uneven last rows and the `thumbnails--blog` max-width cap (780px) limits the grid to ~2 columns

## Goal

Replace the fixed-width flex layout with a CSS grid that adapts to any number of cards, gives text room to breathe, and works correctly from mobile up to wide desktop.

## Design

### Layout (styles.css)

**`.thumbnails`** — switch from flexbox to CSS grid:

```css
.thumbnails {
  max-width: 1200px;
  margin: 48px auto;
  padding: 0 24px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 32px;
}
```

- `justify-content: center` is removed — no longer needed; grid fills its container naturally
- Column count is viewport-driven: ~4 at 1200px, 3 at 900px, 2 at 600px, 1 at 400px
- No explicit media queries required

**`.thumbnail-card`** — remove `width: 350px`:

```css
.thumbnail-card {
  aspect-ratio: 1 / 1;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  border-radius: 12px;
}
```

Grid columns control card width. `aspect-ratio: 1/1` is retained so cards stay square at any column width. All other rules (image, label, link) are unchanged.

### HTML (index.html)

Remove the `thumbnails--blog` modifier from the homepage section:

```html
<!-- before -->
<section class="thumbnails thumbnails--blog">

<!-- after -->
<section class="thumbnails">
```

`thumbnails--blog` (which sets `max-width: 780px`) remains in use on `blog-posts.html` only. The homepage uses the standard `.thumbnails` max-width of 1200px.

### Card structure

No changes. Label on top, image below — existing structure is retained.

## Files Changed

| File | Change |
|---|---|
| `styles.css` | `.thumbnails`: replace flex with grid, remove `justify-content`. `.thumbnail-card`: remove `width: 350px` |
| `index.html` | Remove `thumbnails--blog` class from the section element |
