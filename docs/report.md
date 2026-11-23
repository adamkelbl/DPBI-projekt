# Report

PBIR, theme `sqlbi`, pages 1280×720. Field display names in visuals are Czech, model names stay English.
Everything is defined in the files – no Desktop-only steps remain (open `.pbip` → Refresh).

## Common header (4 main pages)
| Visual (folder) | x, y, w, h |
|---|---|
| Title textbox (24pt) | 24, 18, 400, 60 |
| Slicer Rok (`Date.Year`, dropdown, sync group `Rok`) | 440, 6, 130, 86 |
| Slicer Region (`States.Region`, dropdown, sync group `Region`) | 582, 6, 170, 86 |
| Button Reset filtrů (ClearAllSlicers) | 764, 30, 116, 40 |
| Page navigator | 892, 26, 364, 48 |

Page display names are short (Přehled / Geografie / Produkty / Zákazníci) so the navigator buttons fit; the title textbox carries the full name.
Slicers sync across pages via `visual.syncGroup` (field + filter changes).

## Pages

| Page | Visuals |
|---|---|
| Přehled | 5 KPI cards (Tržby, Objednávky, Průměrná objednávka, Doručeno včas, Ø hodnocení) · combo chart monthly Revenue + Order Count (`TrendChart`) ↔ matrix Year/Month (`TrendMatrix`, hidden) toggled by buttons Graf / Tabulka (bookmarks) · bar Revenue by region · donut payments by type · line running total |
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
- Combined visuals + interactivity: combo chart, cross-filtering, synced slicers, drill-down matrix, Top N, page navigator
- Tooltips: default visual tooltips only (report-page tooltip removed)
- Bookmarks: chart/table toggle on Přehled
- Calculated columns / measures: see `docs/data-model.md`
