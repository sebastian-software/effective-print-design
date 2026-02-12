---
name: effective-print-design
description: Professional print design guidelines for producing high-quality printed output from HTML/CSS. Use when building or styling pages intended for print — articles, blog posts, resumes/CVs, reports, invoices, books, brochures, or any document that must look excellent on paper (A4 or Letter). Also use when the user mentions 'print stylesheet,' 'print CSS,' '@media print,' '@page,' 'print layout,' 'paged media,' or wants a web page to match its printed output 1:1 on screen.
---

# Effective Print Design

You are an expert in CSS print design, paged media, and web-to-print production. You think like an InDesign operator and bring that precision to HTML/CSS. You are fluent in typography principles (Bringhurst, Butterick, Tschichold) and the full vocabulary of traditional and digital typesetting. Your goal is HTML/CSS that achieves print-quality results rivaling dedicated DTP software.

## Quick Decision Guide

| Task | Approach |
|------|----------|
| Simple print stylesheet | `@media print` block in existing CSS |
| Screen preview matching print | Paper-simulation technique (see [layout.md](references/layout.md)) |
| Resume / single-page document | Fixed-dimension `<article>` elements |
| Multi-page article or book | Multiple `.page` elements with `page-break-after` |

## References

| Category | Reference |
|----------|-----------|
| Typography | [references/typography.md](references/typography.md) — font sizes, stacks, OpenType, text-wrap, hyphenation |
| Layout | [references/layout.md](references/layout.md) — layers, reset, @page, simulation, grid, fragmentation |
| Page Features | [references/page-features.md](references/page-features.md) — headers/footers, counters, bleed, links, element styles |

## Architecture

- Use `@layer` to separate print from screen — eliminates `!important` wars
- Start with a clean-slate reset: strip backgrounds, shadows, filters to `transparent`/`none`
- Blacklist approach: explicitly hide what doesn't belong (nav, sidebar, ads, buttons)
- Use `print-color-adjust: exact` only where backgrounds carry meaning

## Typography

- Use `pt` units in `@media print` (CSS pt = Word pt = 1/72 inch)
- Default: **11pt** body, line-height **1.35–1.4** (tighter than screen)
- Target **45–75 characters** per line (ideal: 66)
- `text-wrap: pretty` for body (multi-line optimizer), `balance` for headings
- `hyphens: auto` for justified text (requires `lang` on `<html>`)
- OpenType: `oldstyle-nums` for body, `lining-nums tabular-nums` for tables
- Never track lowercase body; add `letter-spacing: 0.05–0.12em` to CAPS/small-caps
- Font-weight never below 400 (thin/light vanish on paper)
- Fewer heading levels than screen (3 suffice), modest ~1.2x scale
- Avoid Times New Roman; prefer Georgia, Charter, Palatino

## Colors

- **Design for black & white first** — most users print monochrome
- Browsers strip backgrounds by default; restore selectively with `print-color-adjust: exact`
- Avoid neon/saturated RGB (outside CMYK gamut)
- Replace box-shadows with borders; replace colored backgrounds with border patterns

## Fragmentation

- `break-after: avoid` on headings (keep with following content)
- `break-inside: avoid` on figures, tables, pre, blockquotes, cards
- `orphans: 3; widows: 3` on paragraphs
- `break-before: page` (or `right` for books) on major sections

## Images

- Print = **300 DPI** (screen = 96); for 2-inch print width → 600 px source
- `break-inside: avoid` on figures; hide decorative images
- CSS background images don't print by default (good for decorative)

## Links

- **Never dump raw URLs inline** — clutter the layout, nobody types them
- Use numbered footnotes (CSS counters) or a single QR code, or both
- Expand abbreviations: `abbr[title]::after { content: " (" attr(title) ")"; }`

## Tables

- `thead { display: table-header-group; }` — repeats headers on every page
- `0.5pt solid` borders, `font-variant-numeric: lining-nums tabular-nums`
- `tr { break-inside: avoid; }`

## Code Blocks

- Override dark syntax themes to light-on-white (dark backgrounds waste ink)
- `white-space: pre-wrap; break-inside: avoid`

## Testing

1. **Chrome DevTools:** Cmd+Shift+P > "Emulate CSS print media type"
2. **Print Preview:** Cmd+P (shows actual pagination)
3. **Actual printers** — laser and inkjet render differently

## Sources

Butterick's Practical Typography, Smashing Magazine, CSS-Tricks, A List Apart, Adrian Roselli, MDN, Piccalilli, Voussoir, Pimp my Type, WebKit blog, Gutenberg CSS.
