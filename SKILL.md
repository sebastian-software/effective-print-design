---
name: effective-print-design
description: Professional print design guidelines for producing high-quality printed output from HTML/CSS. Use when building or styling pages intended for print — articles, blog posts, resumes/CVs, reports, invoices, books, brochures, or any document that must look excellent on paper (A4 or Letter). Also use when the user mentions 'print stylesheet,' 'print CSS,' '@media print,' '@page,' 'print layout,' 'paged media,' or wants a web page to match its printed output 1:1 on screen.
---

# Effective Print Design

You are an expert in CSS print design, paged media, and web-to-print production. You have deep expertise in professional typesetting — you think like an InDesign or QuarkXPress operator and bring that same precision to HTML/CSS. You are fluent in typography principles (Bringhurst, Butterick, Tschichold), understand kerning, tracking, leading, baseline grids, optical alignment, and the full vocabulary of traditional and digital typesetting. Your goal is to produce HTML/CSS that achieves print-quality results rivaling dedicated DTP software.

## Quick Decision Guide

| Task | Approach |
|------|----------|
| Simple print stylesheet | `@media print` block in existing CSS |
| Screen preview matching print | Paper-simulation technique (see Page Simulation) |
| Resume / single-page document | Fixed-dimension `<article>` elements |
| Multi-page article or book | Multiple `.page` elements with `page-break-after` |
| React component printing | `react-to-print` library |

## Core Architecture

### CSS Layer Strategy

Use `@layer` to cleanly separate print overrides from screen styles. This eliminates `!important` wars — layer order trumps specificity.

```css
/* Define layer order: print layer wins over base and components */
@layer base, components, print;

/* Or import a dedicated print stylesheet into its layer */
@import url("print.css") layer(print) print;
```

All print styles go inside `@media print { @layer print { ... } }`. The layer only activates during printing, keeping screen styles untouched.

### Print Reset

Start every print stylesheet with a clean-slate reset. Print should feel like a controlled, PDF-like environment — strip all screen-specific visual noise.

```css
@media print {
  @layer print {
    *,*::before,*::after {
      color: #000;
      background: transparent;
      box-shadow: none;
      text-shadow: none;
      filter: none;
      -webkit-filter: none;
    }

    body {
      background: #fff;
      margin: 0;
      padding: 0;
    }
  }
}
```

Then selectively restore backgrounds only where essential (data tables, branded elements) using `print-color-adjust: exact`.

**Note:** If not using `@layer`, use `!important` on the reset properties to override inline styles and high-specificity screen rules.

### Page Setup

```css
@page {
  size: A4 portrait;       /* or: letter, A3, legal, 210mm 297mm */
  margin: 20mm 25mm;       /* top/bottom  left/right */
}

@page :first {
  margin-top: 35mm;        /* extra space for title page */
}

/* Book-style asymmetric margins (gutter for binding) */
@page :left  { margin-left: 20mm; margin-right: 30mm; }
@page :right { margin-left: 30mm; margin-right: 20mm; }
```

**Paper sizes:** A4 = 210 x 297 mm, Letter = 8.5 x 11 in, A3 = 297 x 420 mm, Legal = 8.5 x 14 in.

**Margin guidelines:** Office documents 20-25 mm. Minimum safe margin 5 mm (printer hardware limit). Book interiors 20 mm outer, 30 mm gutter.

### Page Simulation on Screen

Show exact page boundaries on screen so the design matches print 1:1.

```css
@media screen {
  body { background: #e0e0e0; padding: 10mm 0; }
  .page {
    background: white;
    width: 210mm; min-height: 297mm; /* A4 */
    margin: 10mm auto;
    padding: 20mm 25mm;
    box-sizing: border-box;
    box-shadow: 0 0 5mm rgba(0,0,0,.2);
  }
}

@media print {
  @page { size: A4; margin: 20mm 25mm; }
  body { background: white; margin: 0; padding: 0; }
  .page {
    width: auto; min-height: auto; margin: 0; padding: 0;
    box-shadow: none;
    page-break-after: always;
  }
  .page:last-child { page-break-after: auto; }
}
```

### Hide / Show Elements

Use the blacklist approach — explicitly hide what doesn't belong on paper.

```css
@media print {
  nav, .sidebar, .comments, .ads, .cookie-banner,
  .social-share, footer nav, video, audio, button,
  .no-print { display: none !important; }
}
.print-only { display: none; }
@media print { .print-only { display: block; } }
```

## Typography

### Font Sizes (pt = Word pt exactly in print)

CSS `pt` and Word `pt` are identical: 1 pt = 1/72 inch. Use `pt` in `@media print`. Default to **11pt** body text.

| Element | Size | Line-height | Notes |
|---------|------|-------------|-------|
| Body text | 11pt | 1.35-1.4 | Serif or sans — depends on CI |
| h1 | 22-24pt | 1.05-1.1 | |
| h2 | 16-18pt | 1.1-1.15 | |
| h3 | 13-14pt | 1.15-1.2 | |
| h4 | 11pt bold | 1.2-1.3 | Same size as body, bold |
| Sidenotes / marginalia | 8pt | 1.2-1.25 | |
| Captions / footnotes | 8-9pt | 1.2-1.25 | |
| Code blocks | 9pt | 1.3-1.4 | Monospace |

Note: Different fonts at the same pt size appear visually different. 11pt Georgia looks larger than 11pt Garamond. Always verify the visual result; adjust by half-points (10.5pt, 11.5pt) if needed.

**Print line-height is tighter than screen.** Butterick: 120-145% of point size. Longer lines need more; at ~66 chars/line use 1.35.

### Font Choice: Depends on Brand/CI

Both serif and sans-serif work well in print. The choice depends on the brand identity, not on one being objectively better. Classic pairings:

- **Serif body + sans-serif headings** — traditional, bookish
- **Sans-serif body + serif headings** — modern, clean
- **All-serif** — academic, literary
- **All-sans** — corporate, technical

### Font Stacks (system fonts, no web-font loading risk)

```css
@media print {
  /* Serif stack */
  body {
    font-family: Charter, 'Bitstream Charter', 'Sitka Text', Cambria, Georgia, serif;
    font-size: 11pt;
    line-height: 1.35;
  }
  /* Sans-serif stack (alternative) */
  body {
    font-family: Inter, Roboto, 'Helvetica Neue', 'Arial Nova', Arial, sans-serif;
    font-size: 11pt;
    line-height: 1.4;
  }
  code, pre {
    font-family: 'Courier New', Courier, monospace;
    font-size: 9pt;
  }
}
```

Avoid Times New Roman (overused, not especially readable per Butterick). Georgia, Charter, and Palatino are excellent serif choices.

### Advanced Text Wrapping

Modern CSS provides typesetting-quality text wrapping that rivals InDesign.

```css
@media print {
  /* Headings: balanced lines (prevents single-word last line) */
  h1, h2, h3, h4, h5, h6 {
    text-wrap: balance;
    hyphens: none;
  }

  /* Body text: pretty wrapping (optimized paragraph shaping) */
  p, li, dd, blockquote {
    text-wrap: pretty;
    text-align: justify;
    hyphens: auto;
    -webkit-hyphens: auto;

    /* Fine-grained hyphenation (word min 6 chars, 3 before / 2 after hyphen) */
    hyphenate-limit-chars: 6 3 2;
    -webkit-hyphenate-limit-before: 3;
    -webkit-hyphenate-limit-after: 2;

    /* Max 2 consecutive hyphenated lines (typographic best practice) */
    hyphenate-limit-lines: 2;
    -webkit-hyphenate-limit-lines: 2;
  }

  /* Captions: balanced like headings */
  figcaption, caption, summary {
    text-wrap: balance;
  }
}
```

**`text-wrap: pretty`** uses a multi-line algorithm (like InDesign's paragraph composer) instead of the web's default greedy line-by-line approach. It prevents orphaned words, improves rag quality, and reduces unnecessary hyphenation. Chrome 117+, Safari 19+.

**`text-wrap: balance`** equalizes line lengths in headings. Chrome 114+, Firefox 121+, Safari 17.5+.

**Important:** `hyphens: auto` requires `lang` attribute on `<html>` (e.g., `<html lang="de">`).

### OpenType Features

Professional typesetting uses OpenType features for refined typography.

```css
@media print {
  body {
    font-variant-ligatures: common-ligatures;
    font-kerning: normal;
  }

  /* Old-style (lowercase) figures for body text — like a well-set book */
  p, li {
    font-variant-numeric: oldstyle-nums proportional-nums;
  }

  /* Lining (uppercase) figures for tables — aligned columns */
  table {
    font-variant-numeric: lining-nums tabular-nums;
  }

  /* Small caps for abbreviations */
  abbr {
    font-variant-caps: all-small-caps;
    letter-spacing: 0.05em;
  }

  /* Hanging punctuation (Safari only — progressive enhancement) */
  article p {
    hanging-punctuation: first allow-end last;
  }

  /* Drop cap — classic print design element */
  article > p:first-of-type::first-letter {
    initial-letter: 3;
    -webkit-initial-letter: 3;
    font-weight: bold;
    margin-right: 0.05em;
  }
}
```

### Letter-Spacing

- **Body text:** `normal` (never track lowercase body)
- **ALL CAPS / small-caps:** `letter-spacing: 0.05em` to `0.12em` + `font-variant-ligatures: no-common-ligatures`
- **Large display headings:** `letter-spacing: -0.01em` (tighten)
- **Font-weight:** Never below 400 for print (thin/light can vanish on paper)

### Line Length

Target **45-75 characters** per line (ideal: 66). Use page margins or `max-width: 66ch` to constrain.

### Hierarchy Principles

1. Use **fewer heading levels** in print than on screen (3 levels usually suffice)
2. Differentiate by **weight, style, caps, or spacing** — not just size
3. Add **more space before** headings than after to group them with following content
4. Use a modest scale (~1.2x between levels); print needs less contrast than screen
5. Use **`text-box-trim`** (Chrome 133+, Safari 18.2+) on headings for optically precise vertical spacing

## Fragmentation Control

```css
@media print {
  /* Keep headings with following content */
  h1, h2, h3, h4, h5, h6 { break-after: avoid; }

  /* Keep figures, tables, code, blockquotes, cards together */
  figure, table, pre, blockquote, img,
  .card, .entry, .job-entry { break-inside: avoid; }

  /* Orphans and widows */
  p { orphans: 3; widows: 3; }

  /* Force new page before major sections */
  .chapter { break-before: page; }

  /* Book-style: chapters start on right page */
  .chapter-start { break-before: right; }
}
```

Always add `break-inside: avoid` to any self-contained visual block (cards, entries, sidebar sections, skill groups in a CV) to prevent mid-element page breaks.

## Colors and Backgrounds

### Key Rules

1. **Design for black and white first.** Most users print monochrome.
2. **Use the aggressive reset** (see Print Reset above) — then restore selectively.
3. Browsers strip backgrounds by default. Use `print-color-adjust: exact` only where essential.
4. **Avoid neon/saturated RGB colors** that fall outside CMYK gamut.
5. Replace box-shadows with borders; replace colored backgrounds with border patterns.

```css
@media print {
  /* Selectively restore backgrounds where they carry meaning */
  .data-table th {
    background: #eee !important;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }

  /* Branded header — only if the brand requires it */
  .brand-header {
    background: #1a1a2e !important;
    color: #fff !important;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
}
```

## Images

### Resolution

Print standard is **300 DPI**. Screen is 96 DPI. For a 2-inch print width, provide a 600 px wide source image.

```css
@media print {
  img {
    max-width: 100% !important;
    height: auto !important;
    break-inside: avoid;
  }
  figure { break-inside: avoid; margin: 8pt 0; }
  figcaption { font-size: 9pt; font-style: italic; margin-top: 4pt; }
}
```

### Decorative vs Content Images

- Hide decorative images: `img[role="presentation"], img[alt=""] { display: none; }`
- CSS background images do not print by default (good for decorative ones)
- Use `<picture>` with `media="print"` source for high-res print variants

## Links on Paper

**Never dump raw URLs inline after links** (`content: attr(href)` — avoid this). Long URLs clutter the layout and nobody types them from paper.

### Better Approaches

1. **Numbered footnotes** — Replace links with superscript numbers, collect URLs in a footnote section at the bottom of the page or document. Use CSS counters for numbering. In React, render the footnote list conditionally for print.
2. **QR code** — A single QR code linking to the original page URL. Show only in print (`.print-only`). Readers scan to get all links digitally.
3. **Combination** — Footnote numbers for important links + one QR code for the full page.

### Styling Footnote References

```css
@media print {
  sup.fnref {
    font-size: 75%;
    line-height: 0;
    vertical-align: super;
    padding-left: 0.1em;
  }

  .footnotes {
    margin-top: 2em;
    border-top: 0.5pt solid #000;
    padding-top: 1em;
    font-size: 8pt;
    break-inside: avoid;
  }

  .footnotes li { word-break: break-all; margin-bottom: 0.2em; }
}
```

### Abbreviations

```css
@media print {
  abbr[title]::after { content: " (" attr(title) ")"; }
}
```

## Tables

```css
@media print {
  table { width: 100%; border-collapse: collapse; break-inside: auto; }
  thead { display: table-header-group; }  /* repeat headers on every page */
  tfoot { display: table-footer-group; }
  tr { break-inside: avoid; }
  td, th { border: 0.5pt solid #999; padding: 4pt 6pt; font-size: 10pt; }
}
```

## Code Blocks

```css
@media print {
  pre {
    white-space: pre-wrap;
    word-wrap: break-word;
    overflow: visible;
    break-inside: avoid;
    background: #f8f8f8 !important;
    color: #333 !important;
    border: 1px solid #ccc;
    padding: 8pt;
    font-size: 9pt;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
}
```

Override dark syntax themes to light-on-white for print. Dark backgrounds waste ink and may be stripped, leaving invisible text.

## Page Headers, Footers, and Page Numbers

Supported in **Chrome 131+**.

```css
@page {
  margin: 20mm;

  @top-center {
    content: "Document Title";
    font-size: 9pt; color: #666;
  }

  @bottom-right {
    content: "Page " counter(page) " of " counter(pages);
    font-size: 8pt;
  }
}

@page :first {
  @top-center { content: none; }
  @bottom-right { content: none; }
}
```

## Document-Type Guidance

For layout-specific guidelines, see:
- **Resumes/CVs**: See [references/resume-layout.md](references/resume-layout.md)
- **Articles/Blog Posts**: See [references/article-layout.md](references/article-layout.md)
- **Books/Reports**: See [references/book-layout.md](references/book-layout.md)

## Tools and Libraries

| Tool | Type | Best for |
|------|------|----------|
| **react-to-print** | React library | Printing React components via `window.print()` |
| **Gutenberg** | CSS framework | Drop-in print stylesheet base |
| **Browser Print** | Built-in | Simple docs, resumes (Cmd+P > Save as PDF) |

## Testing

1. **Chrome DevTools:** Cmd+Shift+P > "Emulate CSS print media type" (shows styles, not page breaks)
2. **Browser Print Preview:** Cmd+P / Ctrl+P (shows actual pagination)
3. **Test on actual printers** — laser and inkjet render differently

## Sources

Butterick's Practical Typography, Smashing Magazine (2011-2018), CSS-Tricks, A List Apart, Adrian Roselli, MDN, Piccalilli, Voussoir, Pimp my Type, WebKit blog, Gutenberg CSS.
