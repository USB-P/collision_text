# Hero Section — Text Collision Animation

Interactive hero animation built as a self-contained HTML file. Words and badge elements float and physically collide. Users can drag any element with pointer/touch input.

There are two language versions:

| File | Language | Use on |
|---|---|---|
| `index.html` | Slovenian | Slovenian page |
| `index-en.html` | English | English page |

The two files are identical in structure and behaviour. The differences are the badge SVGs (sponsor/partner labels) and the accent date words (`7.—9. oktober` vs `7—9 October`).

---

## Implementation

### How to embed

The animation is a **standalone HTML page**. The simplest integration is an `<iframe>` that fills the hero section:

```html
<!-- Slovenian page -->
<iframe
  src="/path/to/index.html"
  style="width: 100%; height: 100%; border: none; display: block;"
  scrolling="no"
  title="Hero animation"
></iframe>

<!-- English page -->
<iframe
  src="/path/to/index-en.html"
  style="width: 100%; height: 100%; border: none; display: block;"
  scrolling="no"
  title="Hero animation"
></iframe>
```

Alternatively, the contents of the `<style>` block, `<main>` element, and `<script>` block can be lifted directly into the parent page. If doing so, make sure the Typekit stylesheet (see Font section below) is loaded in the parent `<head>`.

---

## Desktop

The animation is designed at **1440 × 773 px** as its reference desktop size.

- Give the hero section `width: 100%; height: 100vh` (or a fixed pixel height that matches the design intent).
- The SVG viewport scales automatically to fill whatever container it is placed in while maintaining the correct aspect ratio.
- Element positions, font sizes, and grid density all interpolate between 393 px (mobile) and 1440 px (desktop) using a linear ramp, so the layout adapts smoothly across breakpoints without any extra work.
- Above 1440 px the layout stays at its maximum density; text and badges continue to scale up proportionally.

---

## Mobile

The animation handles mobile automatically when the viewport is narrower than **760 px**.

- The background text grid switches from 7 repetitions per row (desktop) to 3 repetitions per row, keeping the grid readable at small sizes.
- Word and accent text sizes lerp down to their mobile minimums (110 px → ~64 px reference sizes at 393 px width).
- Touch dragging works natively via the Pointer Events API; no extra configuration is needed.
- Set the iframe (or container) to `height: 100svh` on mobile to avoid browser chrome overflow issues.

The breakpoint constants in the source are:

```
GRID_MOBILE_BREAKPOINT = 760   // px — switches grid density
Mobile reference width  = 393  // px (iPhone 14 natural width)
Desktop reference width = 1440 // px
```

---

## Font

The animation uses **Univers Next Pro** (Bold 700) loaded from Adobe Fonts via Typekit.

```html
<link rel="stylesheet" href="https://use.typekit.net/xfw8hqh.css">
```

This `<link>` is already present in both HTML files. If you embed the animation code directly into a parent page instead of using an iframe, move this line into the parent `<head>`. The animation waits for `document.fonts.ready` before finalising text metrics, so the font must be available on the same domain/CDN.

---

## Assets

The `svg/` folder contains the source SVG files for the badge elements used in the physics scene:

| File | Version | Description |
|---|---|---|
| `Black1_si.svg` | Slovenian | Small black pill badge (first row) |
| `Black1_en.svg` | English | Small black pill badge (first row) |
| `Black2_si.svg` | Slovenian | Small black pill badge (second row) |
| `Black2_en.svg` | English | Small black pill badge (second row) |
| `Blue_si.svg` | Slovenian | Blue rectangular badge (Cukrarna) |
| `Purple_si.svg` | Slovenian | Purple circle badge |
| `Purple_en.svg` | English | Purple circle badge |

These SVG files are **inlined as strings** inside the `loadInlineSvg()` function in each HTML file. They do not need to be served separately — the HTML files are fully self-contained. The `svg/` folder is provided as a source reference if badge content needs to be updated.

To update a badge: edit the SVG source file, then copy the SVG markup into the corresponding `content:` string inside `loadInlineSvg()` in the relevant HTML file.

---

## Colours

| Role | Value |
|---|---|
| Page / SVG background | `#ffffff` |
| Paper fill | `#ffffff` |
| Main words | `#000000` |
| Accent words (date) | `#FF2D55` |
| Background grid text | `#000000` |

---

## Dependencies

None. Both files are vanilla HTML + JavaScript with no build step, no npm packages, and no external JS libraries. The only external resource is the Typekit font stylesheet.
