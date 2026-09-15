# Live-Oak-Managers-Dashboard-
The control center for Live Oak Manager Metric analysis.

## Directors tab & SMART goals

- **Directors tab** — gated behind the same director password as Store Targets. Shows
  company-wide health (average % of tracked metrics on target across all stores),
  a "Current Focus Areas" list of every off-target metric on each store's most
  recently logged week (worst stores first), and a company-average table per metric.
- **SMART goals** — directors add/edit/delete goals per store from the Directors tab.
  Each shop's own tab shows its goals read-only, with an auto-computed on-track/
  off-track badge when a goal is linked to a scorecard metric. Goals are **not**
  included in the "Save Snapshot" PNG export.

### Backend note: goals sync

Weeks and targets already sync through the Google Apps Script Web App at
`SHEET_API_URL` (see `syncWeekToSheet` / `syncTargetsToSheet` in `index.html`).
Goals follow the same pattern (`action: 'saveGoals'`, one row per store with a
`GoalsJson` column holding the JSON-encoded goal list, loaded back via a `goals`
array in the Web App's GET response) — but the deployed Apps Script itself lives
outside this repo, so it needs a matching handler added there (a `Goals` sheet
tab + a `saveGoals` case in `doPost`, plus a `goals` key added to `doGet`'s JSON,
mirroring however `saveTargets`/`targets` are already implemented). Until that's
added, goals still work fully across a single session but show "updated locally,
but sync to Sheet failed" and won't persist across devices/reloads — same
graceful-degradation behavior the app already has for weeks/targets when the
Sheet is unreachable.
