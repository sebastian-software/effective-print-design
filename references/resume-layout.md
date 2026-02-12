# Resume / CV Print Layout

## Page Setup

```css
@page {
  size: A4 portrait;  /* or: letter */
  margin: 0;          /* handle margins in the element for screen preview */
}
```

Set `margin: 0` on `@page` and manage spacing with element padding -- this avoids double-margin issues and gives identical screen/print results.

## Layout Structure

```css
.resume {
  display: grid;
  grid-template-columns: 1fr 2fr;  /* sidebar + main */
  width: 210mm;
  min-height: 297mm;
  padding: 15mm 20mm;
  box-sizing: border-box;
  font-family: Charter, 'Bitstream Charter', Cambria, Georgia, serif;
  font-size: 10pt;
  line-height: 1.35;
  color: #000;
}
```

### Screen Preview

```css
@media screen {
  body { background: #e0e0e0; display: flex; justify-content: center; padding: 2rem 0; }
  .resume {
    background: white;
    box-shadow: 0 4px 20px rgba(0,0,0,.15);
  }
}

@media print {
  body { background: none; margin: 0; padding: 0; }
  .resume { box-shadow: none; max-width: none; }
}
```

## Scaling Trick: rem Units

Set everything in `rem` so one root change scales the entire resume.

```css
html { font-size: 10pt; }

h1 { font-size: 2rem; }       /* 20pt -- name */
h2 { font-size: 1.2rem; }     /* 12pt -- section headers */
h3 { font-size: 1rem; }       /* 10pt -- job titles */
p  { font-size: 1rem; }
.entry { margin-bottom: 0.6rem; }
```

If the resume overflows, reduce `html { font-size: 9.5pt; }` -- everything shrinks proportionally.

## Typography

- **Name:** 18-22 pt, font-weight 700, letter-spacing -0.01em
- **Section headings:** 11-12 pt, uppercase, letter-spacing 0.08em, border-bottom 0.5pt solid #000
- **Job title/company:** 10-11 pt bold
- **Dates:** 9-10 pt, color #555, aligned right
- **Body text:** 9.5-10.5 pt, line-height 1.3-1.4
- **Contact info:** 9 pt

## Fragmentation

```css
@media print {
  .job-entry { break-inside: avoid; }
  h2, h3 { break-after: avoid; }
  .resume { break-inside: avoid; }  /* single-page resume */
}
```

For multi-page CVs, remove `break-inside: avoid` from `.resume` and use `page-break-after: always` between logical sections.

## Sidebar Content

Sidebars work well for: contact info, skills, languages, education, certifications. Use a light background with `print-color-adjust: exact` if desired, or a border-left separator.

```css
.sidebar {
  border-right: 0.5pt solid #ccc;
  padding-right: 15mm;
}

/* Or with background */
.sidebar {
  background: #f5f5f5;
  -webkit-print-color-adjust: exact;
  print-color-adjust: exact;
  padding: 15mm;
}
```

## ATS (Applicant Tracking) Optimization

- Use semantic HTML5 (`<section>`, `<article>`, `<header>`)
- Avoid images for text content
- Disable ligatures: `text-rendering: optimizeSpeed`
- Use standard heading hierarchy (h1 for name, h2 for sections, h3 for entries)
- Keep HTML clean and parseable

## Responsive for Mobile Viewing

```css
@media screen and (max-width: 65rem) {
  .resume {
    grid-template-columns: 1fr;
    width: auto;
    min-height: auto;
    padding: 2rem;
  }
}
```

## Generating PDF

Browser "Save as PDF" (Cmd+P > Save as PDF) is the simplest and most reliable method for resumes. It respects all `@media print` and `@page` rules.
