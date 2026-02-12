# Effective Print Design

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code) for producing high-quality printed output from HTML/CSS. Think like an InDesign operator, build with web standards.

## What It Does

Guides Claude to generate print-quality CSS for articles, resumes, reports, invoices, books, brochures — any document that must look excellent on paper. Covers typography, page layout, fragmentation, colors, tables, images, links, and locale-specific conventions.

## Install

```bash
claude skill add effective-print-design
```

## When It Activates

Automatically used when you mention print stylesheets, `@media print`, `@page`, paged media, or ask for a web page to match its printed output.

## Structure

| File | Contents |
|------|----------|
| `SKILL.md` | Core principles and quick decision guide |
| `references/typography.md` | Font sizes, stacks, OpenType, text-wrap, hyphenation |
| `references/layout.md` | Layers, reset, @page, page simulation, grid, fragmentation |
| `references/page-features.md` | Headers/footers, counters, bleed, links, element styles |
| `references/locale.md` | Quotation marks, dashes, numbers, spacing per locale |

## Sources

Bringhurst (Elements of Typographic Style), Butterick's Practical Typography, Rutter (Web Typography), van Aaken (Webtypobuch), Stein (Webfont Handbook), Santa Maria (On Web Typography), Smashing Magazine, CSS-Tricks, A List Apart, MDN.

## License

[MIT](LICENSE.md)
