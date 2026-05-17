# Dark Mode Design

**Date:** 2026-05-17
**Status:** Approved

## Overview

Add a manually-toggled dark mode to liatmoss.github.io. The user's preference is persisted in `localStorage` so it is remembered across page navigations and return visits.

## Color Palette

All hardcoded color values in `styles.css` are replaced with CSS custom properties. Light mode is the default; dark overrides are applied when `html[data-theme="dark"]` is set.

| Role | CSS Variable | Light | Dark |
|---|---|---|---|
| Page background | `--bg` | `#ffffff` | `#0d0f1e` |
| Body text | `--text` | `#05071f` | `#e8e9f5` |
| Accent (headings, links) | `--accent` | `#3d3580` | `#a8b0ff` |
| Surface (cards, footer bg) | `--surface` | `#ece9ff` | `#1a1d3a` |
| Surface text | `--surface-text` | `#3d3580` | `#a8b0ff` |
| Surface border | `--surface-border` | `#d4cefc` | `#2d3060` |
| Secondary body text | `--text-secondary` | `#1a1a2e` | `#c8caf0` |

The hero section uses a fixed dark navy background with an overlay image — it looks identical in both modes and requires no variable changes.

## Architecture

### CSS

`styles.css` is updated in two places:

1. A `:root` block defines the light-mode CSS variables.
2. An `html[data-theme="dark"]` block overrides them with dark-mode values.
3. All hardcoded color values throughout the file are replaced with `var(--variable-name)`.

### Theme initialisation (per HTML file, in `<head>`)

A small inline `<script>` is placed in `<head>` **before** the `<link rel="stylesheet">` tag. It reads `localStorage` and sets `data-theme` on `<html>` before the browser renders anything, preventing a flash of the wrong theme during page load.

```html
<script>
  if (localStorage.getItem('theme') === 'dark')
    document.documentElement.setAttribute('data-theme', 'dark');
</script>
```

This script is added to all 4 HTML files: `index.html`, `about.html`, `experience.html`, `blog-posts.html`.

### Toggle logic (per HTML file, end of `<body>`)

A second inline `<script>` at the bottom of `<body>` defines the toggle function and initialises the button label on load:

```js
function toggleTheme() {
  const isDark = document.documentElement.getAttribute('data-theme') === 'dark';
  document.documentElement.setAttribute('data-theme', isDark ? 'light' : 'dark');
  localStorage.setItem('theme', isDark ? 'light' : 'dark');
  document.getElementById('theme-toggle').textContent = isDark ? '🌙' : '☀️';
}
document.getElementById('theme-toggle').textContent =
  document.documentElement.getAttribute('data-theme') === 'dark' ? '☀️' : '🌙';
```

## Toggle Button

A button is added to the hero markup on all 4 pages, positioned absolutely in the top-right corner of the `.hero` section:

```html
<button id="theme-toggle" class="hero__theme-toggle" onclick="toggleTheme()" aria-label="Toggle dark mode"></button>
```

Styling (added to `styles.css`):

- Positioned `absolute`, `top: 16px`, `right: 16px`
- Small transparent pill with a white border, matching the existing `.hero__button` aesthetic
- Font size `1.25rem` to make the emoji legible
- Focus ring for accessibility

## Files Changed

| File | Change |
|---|---|
| `styles.css` | Add CSS variables, dark theme overrides, `.hero__theme-toggle` styles |
| `index.html` | Add init script in `<head>`, toggle button in hero, toggle script in `<body>` |
| `about.html` | Same as above |
| `experience.html` | Same as above |
| `blog-posts.html` | Same as above |
