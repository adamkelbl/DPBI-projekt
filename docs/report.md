# Report

PBIR, theme `sqlbi`, pages 1280×720. Built with `pbir` CLI. Field display names in visuals are Czech (`pbir fields rename`), model names stay English.

## Common header (4 main pages)
Title textbox · slicer Rok (`Date.Year`) · slicer Region (`States.Region`) · button Reset filtrů (action ClearAllSlicers) · page navigator.
Slicer sync across pages is **not** set (no pbir command) – do it in Desktop (View → Sync slicers).

## Pages

| Page | Visuals |
|---|---|
| Přehled | 5 KPI cards (Tržby, Objednávky, Průměrná objednávka, Doručeno včas, Ø hodnocení) · combo chart monthly Revenue + Order Count (`TrendChart`) ↔ matrix Year/Month (`TrendMatrix`, hidden) toggled by bookmarks · bar Revenue by region (tooltip page) · donut payments by type · line running total |
| Geografie a doručení | 5 KPI cards (on-time, delivery days, late orders, avg delay, review gap) · Azure map bubbles by state (size = Revenue, colour = On-Time Color, tooltip page) · scatter delivery days vs review per state (tooltip page) · column Avg review by DelayBucket (colour = Review Color) |
| Produkty a prodejci | Top 10 categories bar (TopN filter) · treemap Revenue by seller region · column late rate by weight class · matrix category performance (data bars on Revenue, Review Color font) · table Top 20 sellers (TopN) |
| Zákazníci a hodnocení | 5 KPI cards · column review score distribution · line Avg review + On-time rate (Y2) over time · 100 % bar sentiment by delivery status · matrix weekday × hour heatmap (gradient) |
| Detail kategorie (hidden) | drillthrough on `Products[Category]` · Back button · category header card · 4 KPI · monthly trend · revenue by region · score distribution |
| Tooltip stát (hidden) | tooltip page 320×240: Selection Title, Tržby, Objednávky, Doručeno včas, Ø hodnocení |

Trend visuals carry a visual-level filter `Date[IsAnalysisPeriod] = TRUE` (2017-01 … 2018-08).

## Bookmarks

| Bookmark | Scope | Used by | State |
|---|---|---|---|
| Trend – graf | display + current page, targets TrendChart, TrendMatrix | button Graf (Přehled) | capture in Desktop: TrendChart visible, TrendMatrix hidden → Update |
| Trend – tabulka | same | button Tabulka (Přehled) | capture in Desktop: TrendChart hidden, TrendMatrix visible → Update |
| Insight – zpoždění a recenze | data + display + page | bookmarks pane | capture in Desktop on Zákazníci a hodnocení with a chosen filter → Update |

`pbir add bookmark` creates empty state; Power BI fills it on Update in Desktop.

## Grading map
- Combined visuals + interactivity: combo chart, cross-filtering, drillthrough, drill-down matrix, Top N, slicers
- Tooltip: report-page tooltip `Tooltip stát` on map, scatter, region bar
- Bookmarks: chart/table toggle + insight bookmark
- Calculated columns / measures: see `docs/data-model.md`
