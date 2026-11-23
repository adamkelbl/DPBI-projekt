# DPBI-projekt

University final project (Power BI, 30 pts): e-commerce dashboard on the Kaggle **Olist Brazilian E-Commerce** dataset (CC BY-NC-SA 4.0).
Story: where/what/to whom Olist sells, and how late delivery hurts reviews (on-time ≈ 4.29 ★ vs late ≈ 2.27 ★).

## Grading requirements
- Source data ≥ 1 000 rows, ≥ 7 columns
- Combined visuals + cross-interactivity
- Tooltip (report-page tooltip) + bookmarks
- Calculated column(s) + measures (DAX)
- Logical, meaningful, usable

## Workflow
- Authoring on macOS: TMDL (semantic model) + PBIR JSON (report) edited as text.
- Validation on remote Windows in Power BI Desktop (open `.pbip`, Refresh). Files move via **git**.
- Bookmarks, final layout tweaks: done in Desktop on Windows, then committed.
- Parameter `DataFolder` (expressions.tmdl) = absolute path to `data\` on the Windows machine, must end with `\`.

## Structure
- `data/` – Olist CSVs (geolocation CSV is gitignored, unused)
- `DPBI-projekt.SemanticModel/definition/` – TMDL: `model.tmdl`, `expressions.tmdl`, `relationships.tmdl`, `tables/*.tmdl`
- `DPBI-projekt.Report/definition/` – report in PBIR format (converted from legacy with `pbir convert`)
- `docs/data-model.md` – tables, relationships, calculated columns, measures (source of truth)
- `docs/report.md` – pages, visuals, tooltip, bookmarks, Desktop-only steps
- `.claude/rules/pbir-cli.md` – pbir CLI gotchas

## Validation benchmarks (whole dataset, no filters)
Revenue R$ 13 591 644 · GMV R$ 15 843 553 · Order Count 99 441 · AOV R$ 160.58 · Customer Count 96 096 · On-Time Rate 93.2 % · Avg Review Score 4.09

## Tooling (macOS)
- Claude Code plugins (project scope, `.claude/settings.json`) from marketplace `data-goblin/power-bi-agentic-development`: `pbip` (TMDL/PBIR skills + validation hooks), `semantic-models` (DAX, Power Query, model review), `reports` (report design, theme, pbir-cli skill).
- `pbir` CLI (`uv tool install pbir-cli`, by Kurt Buhler & Maxim Anatsko): `pbir validate "DPBI-projekt.Report"`, `pbir model "DPBI-projekt.Report" -d` (reads TMDL – quick parse check), `pbir add visual …`, `pbir schema describe <type>`.
- No local engine on macOS: DAX/M are only executed after Refresh in Desktop on Windows.

## Conventions
- Report changes only through `pbir` commands (no hand-edited report JSON); `pbir validate "DPBI-projekt.Report" --all` after each batch.
- Model names (tables, columns, measures) in English; report labels and data values (status, sentiment, months) in Czech.
- CSV type conversion always with culture `"en-US"` (Czech Windows locale would misread decimals).
- TMDL indented with tabs; no `//` comments outside expressions (use `///` descriptions).
- Auto date/time disabled; use the `Date` table.
- A sort-by column must never reference the column it sorts (circular dependency) – compute it from the same base columns.

## Current phase
Phase 3 – report fixed after first render on Windows: map replaced by bar chart, header re-laid out, bookmarks + slicer sync written in files (no Desktop-only steps). Tooltip and drillthrough pages removed at the user's request. On this Windows machine `pbir` is not installed; report JSON was edited directly and checked by parsing.
