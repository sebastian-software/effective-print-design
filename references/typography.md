# Typography Reference

Detailed CSS for print typography. See SKILL.md for principles.

## Font Sizes

CSS `pt` and Word `pt` are identical: 1 pt = 1/72 inch. Different fonts at the same pt size appear visually different — always verify; adjust by half-points (10.5pt, 11.5pt) if needed.

| Element | Size | Line-height | Notes |
|---------|------|-------------|-------|
| Body text | 11pt | 1.35–1.4 | Serif or sans — depends on CI |
| h1 | 22–24pt | 1.05–1.1 | |
| h2 | 16–18pt | 1.1–1.15 | |
| h3 | 13–14pt | 1.15–1.2 | |
| h4 | 11pt bold | 1.2–1.3 | Same size as body, bold |
| Sidenotes / marginalia | 8pt | 1.2–1.25 | |
| Captions / footnotes | 8–9pt | 1.2–1.25 | |
| Code blocks | 9pt | 1.3–1.4 | Monospace |

### Document-Type Variations

| Property | Resume / CV | Article / Blog | Book / Report |
|----------|-------------|----------------|---------------|
| Body size | 9.5–10.5 pt | 11 pt | 11–12 pt |
| Line-height | 1.3–1.4 | 1.35 | 1.4–1.5 |
| Body font | Serif or Sans (CI) | Serif preferred | Serif preferred |
| Heading font | Contrasting pair | Sans-serif | Sans-serif or matching serif |
| Text-align | Left (not justified) | Justify | Justify |
| Hyphens | No | Yes | Yes |

## Font Choice

Both serif and sans-serif work well in print. The choice depends on brand identity. Classic pairings:

- **Serif body + sans-serif headings** — traditional, bookish
- **Sans-serif body + serif headings** — modern, clean
- **All-serif** — academic, literary
- **All-sans** — corporate, technical

## Font Stacks (System Fonts)

```css
@media print {
  /* Serif */
  body {
    font-family: Charter, 'Bitstream Charter', 'Sitka Text', Cambria, Georgia, serif;
    font-size: 11pt;
    line-height: 1.35;
  }
  /* Sans-serif (alternative) */
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

## Text Wrapping & Hyphenation

```css
@media print {
  /* Headings: balanced lines (prevents single-word last line) */
  h1, h2, h3, h4, h5, h6 {
    text-wrap: balance;
    hyphens: none;
  }

  /* Body: pretty wrapping (multi-line optimizer like InDesign's paragraph composer) */
  p, li, dd, blockquote {
    text-wrap: pretty;
    text-align: justify;
    hyphens: auto;
    -webkit-hyphens: auto;
    hyphenate-limit-chars: 6 3 2;       /* word min 6, 3 before / 2 after hyphen */
    -webkit-hyphenate-limit-before: 3;
    -webkit-hyphenate-limit-after: 2;
    hyphenate-limit-lines: 2;           /* max 2 consecutive hyphenated lines */
    -webkit-hyphenate-limit-lines: 2;
  }

  /* Captions: balanced like headings */
  figcaption, caption, summary {
    text-wrap: balance;
  }
}
```

**`text-wrap: pretty`** — Chrome 117+, Safari 19+. **`text-wrap: balance`** — Chrome 114+, Firefox 121+, Safari 17.5+. **`hyphens: auto`** requires `lang` attribute on `<html>`.

## OpenType Features

```css
@media print {
  body {
    font-variant-ligatures: common-ligatures;
    font-kerning: normal;
  }

  /* Old-style (lowercase) figures for body text */
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

  /* Drop cap */
  article > p:first-of-type::first-letter {
    initial-letter: 3;
    -webkit-initial-letter: 3;
    font-weight: bold;
    margin-right: 0.05em;
  }
}
```

## Letter-Spacing

- **Body text:** `normal` (never track lowercase body)
- **ALL CAPS / small-caps:** `letter-spacing: 0.05em` to `0.12em` + `font-variant-ligatures: no-common-ligatures`
- **Large display headings:** `letter-spacing: -0.01em` (tighten)

## Hierarchy Principles

1. Use **fewer heading levels** in print than on screen (3 usually suffice)
2. Differentiate by **weight, style, caps, or spacing** — not just size
3. **More space before** headings than after to group them with following content
4. Modest scale (~1.2x between levels); print needs less contrast than screen
5. Use **`text-box-trim`** (Chrome 133+, Safari 18.2+) for optically precise vertical spacing

## Resume Typography

```css
h1 { font-size: 2rem; font-weight: 700; letter-spacing: -0.01em; }           /* Name: 18–22pt */
h2 { font-size: 1.2rem; text-transform: uppercase; letter-spacing: 0.08em;    /* Sections: 11–12pt */
     border-bottom: 0.5pt solid #000; }
h3 { font-size: 1rem; font-weight: 700; }                                     /* Job titles: 10–11pt */
.dates { font-size: 0.9rem; color: #555; text-align: right; }                 /* 9–10pt */
.contact { font-size: 0.9rem; }
```

## rem Scaling Trick

Set everything in `rem` so one root change scales the entire document. If the layout overflows, reduce to `9.5pt` — everything shrinks proportionally.

```css
html { font-size: 10pt; }
h1 { font-size: 2rem; }       /* 20pt */
h2 { font-size: 1.2rem; }     /* 12pt */
h3 { font-size: 1rem; }       /* 10pt */
p  { font-size: 1rem; }
.entry { margin-bottom: 0.6rem; }
```

## ATS (Applicant Tracking) Optimization

For resumes parsed by automated systems:

- Use semantic HTML5 (`<section>`, `<article>`, `<header>`)
- Avoid images for text content
- Disable ligatures: `text-rendering: optimizeSpeed`
- Use standard heading hierarchy (h1 for name, h2 for sections, h3 for entries)
