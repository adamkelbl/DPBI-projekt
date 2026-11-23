# Report

PBIR, theme `sqlbi`, pages 1280×720. Field display names in visuals are Czech, model names stay English.
Everything is defined in the files – no Desktop-only steps remain (open `.pbip` → Refresh).

## Common header (4 main pages)
| Visual (folder) | x, y, w, h |
|---|---|
| Title textbox: page name 28pt Segoe UI Semibold + one-line Czech subtitle 11pt grey, padding 0 | 24, 6, 760, 80 |
| Slicer Rok (`Date.Year`, dropdown, sync group `Rok`) | 800, 6, 130, 76 |
| Slicer Region (`States.Region`, dropdown, sync group `Region`) | 942, 6, 190, 76 |
| Button Reset filtrů (ClearAllSlicers) | 1144, 24, 112, 40 |

Czech strings in PowerShell scripts: PS 5.1 reads BOM-less .ps1 as ANSI and garbles diacritics – keep them in a UTF-8 data file.

KPI cards row y = 92, first content row y = 192.
No page navigator buttons (removed at the user's request) – pages are switched with the page tabs.

The `sqlbi` theme gives every visual a title, divider, 10 px padding and drop shadow. Buttons and textboxes override this
(title off, padding 0, no shadow, text 10pt centred) – otherwise their text is clipped. Buttons get a visible box via
container background `#E6E6E6` + border `#B3B0AD` (the theme hides actionButton backgrounds), fill darker on hover.
Year slicers carry a visual filter `Date[Year] IN {2017, 2018}`: 2016 has only ~330 orders (Sep–Dec) and lies outside
`IsAnalysisPeriod`, so picking it left the trend charts empty.
Slicers show only the container title (slicer header off) so the dropdown fits in 76 px.
Hand-edited JSON is re-indented to 2 spaces (PowerShell 5.1 `ConvertTo-Json` output is reformatted).
Slicers sync across pages via `visual.syncGroup` (field + filter changes).

## Pages

| Page | Visuals |
|---|---|
| Úvod (first page, `a0c1e2d3f4b5a6c7`) | title textbox · 3 text cards: O datech, O reportu, Jak report vznikl (Mac → PBIP/TMDL/PBIR, Claude Code + Data Goblins plugins/skills, pbir CLI by Kurt Buhler & Maxim Anatsko, Git → Windows). No slicers. Source text: Czech, edited in the visual.json files |
| Přehled | 5 KPI cards (Tržby, Objednávky, Průměrná objednávka, Doručeno včas, Ø hodnocení) · combo chart monthly Revenue + Order Count (`TrendChart`) ↔ matrix Year/Month (`TrendMatrix`, hidden) toggled by buttons Graf / Tabulka (bookmarks, placed in the chart's title row) · bar Revenue by region · bar payments by type (`PaymentDonut` folder) · line running total |
| Geografie a doručení | 5 KPI cards (on-time, delivery days, late orders, avg delay, review gap) · bar chart Revenue by state (`StateMap` folder, colour = On-Time Color) · scatter delivery days vs review per state (labels = state code, size = orders) · column Avg review by DelayBucket (colour = Review Color) |
| Produkty a prodejci | Top 10 categories bar (TopN filter) · treemap Revenue by seller region · column late rate by weight class · matrix category performance (data bars on Revenue, Review Color font) · table Top 20 sellers (TopN) |
| Zákazníci a hodnocení | 5 KPI cards · column review score distribution · line Avg review + On-time rate (Y2) over time · 100 % bar sentiment by delivery status · matrix weekday × hour heatmap (gradient) |

Trend visuals carry a visual-level filter `Date[IsAnalysisPeriod] = TRUE` (2017-01 … 2018-08).
The hidden tooltip page and drillthrough page were removed on purpose (2026-11-23).

## Bookmarks

| Bookmark | Options | State |
|---|---|---|
| Trend – graf | targets TrendChart + TrendMatrix, applyOnlyToTargetVisuals, suppressData, suppressActiveSection | TrendChart visible, TrendMatrix `display.mode = hidden` |
| Trend – tabulka | same | TrendChart hidden, TrendMatrix visible |

Buttons `BtnShowChart` / `BtnShowTable` on Přehled link to them (`visualLink.bookmark` = bookmark id).

## Grading map
- Combined visuals + interactivity: combo chart, cross-filtering, synced slicers, drill-down matrix, Top N, chart/table toggle
- Tooltips: custom tooltip fields (`Tooltips` role, Czech display names) – report-page tooltip removed by request:
  - Revenue by state (Geografie): Objednávky, Průměrná objednávka, Doručeno včas, Ø doba doručení, Ø hodnocení
  - Scatter delivery vs review (Geografie): Tržby, Doručeno včas, Opožděné objednávky
  - Revenue by region (Přehled): Objednávky, Zákazníci, Doručeno včas, Ø hodnocení
  - Top 10 categories (Produkty): Ø hodnocení, Objednávky, Prodané položky, Podíl opožděných
- Bookmarks: chart/table toggle on Přehled
- Calculated columns / measures: see `docs/data-model.md`
