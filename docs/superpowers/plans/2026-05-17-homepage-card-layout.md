# Homepage Card Layout Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the fixed-width flex card layout on the homepage with a responsive CSS grid that scales gracefully as more cards are added.

**Architecture:** Two CSS rules change in `styles.css` (`.thumbnails` switches from flex to grid; `.thumbnail-card` loses its fixed width) and one class is removed from the homepage section element in `index.html`. No new files, no structural HTML changes, no JS changes.

**Tech Stack:** Vanilla HTML, CSS Grid (no build step)

---

## File Map

| File | Change |
|---|---|
| `styles.css` | Replace flex with grid on `.thumbnails`; remove `width: 350px` from `.thumbnail-card` |
| `index.html` | Remove `thumbnails--blog` class from the `<section>` element |

---

## Task 1: Update styles.css

**Files:**
- Modify: `styles.css`

### Step 1.1: Replace flex with grid on `.thumbnails`

Find the `.thumbnails` rule (currently around line 137) and replace the three flex properties with a grid declaration:

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

Remove these three lines:
- `display: flex;`
- `flex-wrap: wrap;`
- `justify-content: center;`

Add these two lines:
- `display: grid;`
- `grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));`

(`gap: 32px` already exists and stays unchanged.)

### Step 1.2: Remove fixed width from `.thumbnail-card`

Find the `.thumbnail-card` rule (currently around line 151) and remove the `width: 350px` line. The rule should become:

```css
.thumbnail-card {
  aspect-ratio: 1 / 1;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  border-radius: 12px;
}
```

### Step 1.3: Verify in browser

Open `index.html` directly in a browser (`open index.html`).

Expected:
- The three cards fill the available width in a row (not centred with space on the sides)
- Resizing the window narrows cards gracefully — no horizontal scroll, no mid-word wrapping
- Cards remain square (aspect-ratio still applied)

- [ ] Step 1.1: Replace flex with grid on `.thumbnails`
- [ ] Step 1.2: Remove `width: 350px` from `.thumbnail-card`
- [ ] Step 1.3: Verify layout in browser

### Step 1.4: Commit

```bash
git add styles.css
git commit -m "feat: switch thumbnail grid from flex to CSS grid"
```

- [ ] Step 1.4: Commit

---

## Task 2: Update index.html

**Files:**
- Modify: `index.html`

### Step 2.1: Remove `thumbnails--blog` class from homepage section

Find the section element in `index.html` (currently around line 18):

```html
<section class="thumbnails thumbnails--blog">
```

Change it to:

```html
<section class="thumbnails">
```

`thumbnails--blog` sets `max-width: 780px`, which was incorrectly limiting the homepage grid to ~2 columns. Removing it lets the homepage use the full 1200px container width. The class stays in `blog-posts.html` where it belongs.

### Step 2.2: Verify in browser

Open `index.html` in a browser and resize the window.

Expected:
- At wide viewport (>1200px): cards fill a centred 1200px-wide grid
- At ~900px: 3 columns
- At ~600px: 2 columns
- At ~400px: 1 column
- No layout difference between dark and light mode

- [ ] Step 2.1: Remove `thumbnails--blog` from `index.html`
- [ ] Step 2.2: Verify responsive behaviour across viewport widths

### Step 2.3: Commit

```bash
git add index.html
git commit -m "fix: remove incorrect thumbnails--blog constraint from homepage"
```

- [ ] Step 2.3: Commit
