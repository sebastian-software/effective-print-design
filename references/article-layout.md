# Article / Blog Post Print Layout

## Page Setup

```css
@page {
  size: A4;
  margin: 20mm 25mm;
}

@page :first {
  margin-top: 30mm;
}
```

## Complete Article Print Stylesheet

```css
@media print {
  /* --- Clean-slate reset --- */
  *,*::before,*::after {
    color: #000 !important;
    background: transparent !important;
    box-shadow: none !important;
    text-shadow: none !important;
    filter: none !important;
  }

  body {
    font-family: Charter, 'Bitstream Charter', Cambria, Georgia, serif;
    font-size: 11pt;
    line-height: 1.35;
    color: #000;
    background: #fff;
    margin: 0;
    padding: 0;
    font-variant-ligatures: common-ligatures;
    font-kerning: normal;
  }

  /* --- Hide non-essential --- */
  nav, .sidebar, .comments, .related-posts, .social-share,
  .newsletter-signup, .cookie-banner, .ads, footer nav,
  .back-to-top, video, audio, .interactive-widget,
  .no-print { display: none !important; }

  /* --- Headings --- */
  h1, h2, h3, h4, h5, h6 {
    font-family: Inter, Roboto, 'Helvetica Neue', Arial, sans-serif;
    color: #000 !important;
    break-after: avoid;
    text-wrap: balance;
    hyphens: none;
  }
  h1 { font-size: 24pt; line-height: 1.1; margin-bottom: 8pt; }
  h2 { font-size: 18pt; line-height: 1.15; margin-top: 24pt; }
  h3 { font-size: 14pt; line-height: 1.2; margin-top: 18pt; }

  /* --- Body text --- */
  p, li, dd {
    orphans: 3;
    widows: 3;
    text-wrap: pretty;
    text-align: justify;
    hyphens: auto;
    -webkit-hyphens: auto;
    hyphenate-limit-chars: 6 3 2;
    hyphenate-limit-lines: 2;
    -webkit-hyphenate-limit-lines: 2;
    overflow-wrap: break-word;
  }

  /* Old-style figures for body, lining for tables */
  p, li { font-variant-numeric: oldstyle-nums proportional-nums; }
  table { font-variant-numeric: lining-nums tabular-nums; }

  /* Hanging punctuation (Safari — progressive enhancement) */
  article p { hanging-punctuation: first allow-end last; }

  /* --- Drop cap (optional — remove if not desired) --- */
  article > p:first-of-type::first-letter {
    initial-letter: 3;
    -webkit-initial-letter: 3;
    font-weight: bold;
    margin-right: 0.05em;
  }

  /* --- Images --- */
  img {
    max-width: 100% !important;
    height: auto !important;
    break-inside: avoid;
  }
  figure { break-inside: avoid; margin: 10pt 0; }
  figcaption {
    font-size: 9pt;
    font-style: italic;
    margin-top: 4pt;
    text-wrap: balance;
  }

  /* --- Code --- */
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
    line-height: 1.4;
    font-family: 'Courier New', Courier, monospace;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
  :not(pre) > code {
    background: #f0f0f0 !important;
    color: #333 !important;
    padding: 1pt 3pt;
    font-size: 90%;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }

  /* --- Tables --- */
  table { width: 100%; border-collapse: collapse; break-inside: auto; }
  thead { display: table-header-group; }
  tr { break-inside: avoid; }
  td, th { border: 0.5pt solid #999; padding: 4pt 6pt; font-size: 10pt; }
  th {
    background: #eee !important;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }

  /* --- Blockquotes --- */
  blockquote {
    border-left: 3pt solid #333;
    padding-left: 12pt;
    margin-left: 0;
    font-style: italic;
    break-inside: avoid;
  }

  /* --- Abbreviations --- */
  abbr[title]::after { content: " (" attr(title) ")"; }
  abbr { font-variant-caps: all-small-caps; letter-spacing: 0.05em; }
}
```

## Links: Use Footnotes (Not Inline URLs)

Do not use `content: attr(href)` after links. Instead, convert links to numbered footnotes with a collected URL list at the bottom. See the "Links on Paper" section in the main SKILL.md for the concept and styling.

## Structured Layouts with CSS Grid

Use CSS Grid for structured sections (e.g., author bio, metadata sidebar). Grid is reliable in print — unlike CSS Columns, it does not suffer from fragile page-break behavior.

```css
@media print {
  .article-meta {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 12pt;
    break-inside: avoid;
  }
}
```

**Avoid CSS `column-count`** for article body text — it is technically fragile with page breaks. If a multi-column layout is desired, use Grid-based sections with `break-inside: avoid` on each section instead.

## Linearize Complex Screen Layouts

Multi-column grid/flex layouts from screen should collapse to single-column for print.

```css
@media print {
  .grid-layout { display: block; }
  .flex-layout { flex-direction: column; }
  main { width: 100%; margin: 0; padding: 0; }
}
```

## Print Source Notice

Show a "printed from" notice only in print.

```html
<div class="print-source-notice">
  Originally published at https://example.com/article
</div>
```

```css
.print-source-notice { display: none; }
@media print {
  .print-source-notice {
    display: block;
    margin-top: 2em;
    padding-top: 1em;
    border-top: 0.5pt solid #ccc;
    font-size: 8pt;
    color: #666 !important;
  }
}
```

## Page Numbers (Chrome 131+)

```css
@page :left  { @bottom-left  { content: counter(page); font-size: 9pt; color: #666; } }
@page :right { @bottom-right { content: counter(page); font-size: 9pt; color: #666; } }
@page :first { @bottom-left { content: none; } @bottom-right { content: none; } }
```
