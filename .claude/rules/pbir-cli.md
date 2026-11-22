# pbir CLI learnings (0.9.33)

- Report-page tooltip: `pbir visuals tooltip --show --type Canvas` + `pbir set "<V>.visualTooltip.section" --value <page internal name>` (`ReportPage` is rejected). Page name from `pbir pages json`.
- Button bookmark action: `pbir visuals action --type Bookmark --target "<display name>"` stores the display name; set `visualLink.bookmark` to the bookmark's internal id (file name in `definition/bookmarks/`).
- `pbir add bookmark` writes empty `explorationState`; capture visibility/filters in Desktop (Update).
- `pbir cp` of a visual to another page gives it a generated name; address copies with `--where "visual_type=..."`.
- `pbir add page` always adds a `Title.Visual` textbox (20,20,h90); no `--no-title` – `pbir rm` it or reposition (24pt text needs h ≥ 54).
- Dropdown slicer with header needs height ≥ ~76 px.
- Matrix gradient CF: `pbir visuals cf --gradient --field <measure> --on values.backColor` (default container is dataPoint, invalid for pivotTable).
- Boolean visual filter: `--values true` stores literal `true` (correct).
- No slicer sync-group command yet – set sync in Desktop.
- In zsh, loop over subcommands with `${=var}` (no word splitting by default).
