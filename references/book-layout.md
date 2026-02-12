# Book / Report Print Layout

For multi-page documents with chapters, table of contents, running headers, and cross-references.

## Page Setup

```css
@page {
  size: A4;  /* or: 5.5in 8.5in for books, letter for reports */
  margin: 25mm 20mm;
}

@page :left {
  margin-left: 20mm;   /* outer */
  margin-right: 30mm;  /* gutter (binding) */
}

@page :right {
  margin-left: 30mm;   /* gutter */
  margin-right: 20mm;  /* outer */
}

@page :first {
  margin: 0;  /* full-bleed title page */
}

@page :blank {
  @top-center { content: none; }
  @bottom-center { content: "This page intentionally left blank"; font-size: 9pt; color: #999; }
}
```

## Running Headers and Page Numbers

Supported in Chrome 131+.

```css
@page :right {
  @top-right {
    content: "Document Title";
    font-size: 9pt;
    font-style: italic;
  }
  @bottom-right {
    content: counter(page);
    font-size: 9pt;
  }
}

@page :left {
  @top-left {
    content: "Book Title";
    font-size: 9pt;
    font-style: italic;
  }
  @bottom-left {
    content: counter(page);
    font-size: 9pt;
  }
}

@page :first {
  @top-left { content: none; }
  @top-right { content: none; }
  @bottom-left { content: none; }
  @bottom-right { content: none; }
}
```

## Named Pages

Different sections can have different page configurations.

```css
.title-page { page: title; }
.chapter     { page: chapter; }
.appendix    { page: appendix; }

@page title {
  size: A4;
  margin: 0;
  @top-center { content: none; }
  @bottom-center { content: none; }
}

@page chapter {
  @bottom-center { content: counter(page); }
}

@page appendix {
  @top-center { content: "Appendix"; font-size: 9pt; }
  @bottom-center { content: "A-" counter(page); font-size: 9pt; }
}
```

## Chapter Starts

```css
.chapter { break-before: right; }  /* start on right-hand page */

.chapter h1 {
  margin-top: 30%;
  text-align: center;
  font-size: 28pt;
  line-height: 1.1;
  text-wrap: balance;
  break-after: avoid;
}
```

## Counters

```css
body { counter-reset: chapternum figurenum tablenum; }

h1.chapter::before {
  counter-increment: chapternum;
  content: "Chapter " counter(chapternum) " — ";
}

figcaption::before {
  counter-increment: figurenum;
  content: "Figure " counter(chapternum) "." counter(figurenum) ". ";
}

.table-caption::before {
  counter-increment: tablenum;
  content: "Table " counter(chapternum) "." counter(tablenum) ". ";
}
```

## Footnotes

Convert links and annotations to numbered footnotes with a collected list at the bottom. See the "Links on Paper" section in the main SKILL.md for the concept and styling.

## Bleed and Crop Marks (Professional Printing)

```css
@page {
  bleed: 3mm;
  marks: crop cross;
}
```

For full-bleed images, extend into bleed area:
```css
.full-bleed {
  width: calc(100% + 6mm);
  margin-left: -3mm;
  margin-right: -3mm;
}
```

## Grid-Based Layouts for Structured Sections

Use CSS Grid for structured sections within a book/report (e.g., two-column intros, figure+text combos, data panels). Grid is reliable in print.

```css
@media print {
  .two-col-section {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 15pt;
    break-inside: avoid;
  }

  .figure-text {
    display: grid;
    grid-template-columns: 2fr 3fr;
    gap: 12pt;
    break-inside: avoid;
    align-items: start;
  }
}
```

**Avoid CSS `column-count`** for flowing text in books — page breaks inside columns are fragile across browsers. Use Grid for structured sections with `break-inside: avoid` instead.
