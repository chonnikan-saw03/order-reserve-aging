# Order Reserve Aging Dashboard

A Tableau Dashboard Extension that shows Order Reserve Aging by branch, aging
tier, and delivery priority. It reads its data directly from a worksheet
named **"Reserve"** in the same Tableau dashboard (via the Tableau Extensions
API) — that worksheet should have every field from
`boonthavorn-data-analytic.tableau.ds_vdo_order_reserve_aging` on it at
row-level detail. This means the extension automatically respects whatever
filters are applied elsewhere on the dashboard, with no separate sign-in step.

A standalone BigQuery + Google OAuth data path also exists in the code as a
fallback/alternative (see "Connect to BigQuery directly" below), but it isn't
wired to any button right now — the worksheet is the primary path.

## Files

- `reserve_aging.html` — the extension itself (single-file, no build step: HTML + CSS + JS inline)
- `manifest.trex` — the Tableau extension manifest

## Setup

### 1. Hosting

This repo is served via **GitHub Pages**, already live at:

`https://chonnikan-saw03.github.io/order-reserve-aging/reserve_aging.html`

`manifest.trex` already points there. Tableau Desktop/Server/Cloud loads the extension
from that URL — no local server needed for normal use.

For local testing, run `python -m http.server 8765` in this folder and temporarily
change the `<url>` in `manifest.trex` to `http://localhost:8765/reserve_aging.html`.
Remember to change it back before sharing the dashboard with anyone else, since
`localhost` only resolves on the machine running the server.

### 2. Add the extension in Tableau

1. In the same dashboard, add a worksheet named **"Reserve"** with every field from
   `ds_vdo_order_reserve_aging` at row-level detail (no aggregation/grouping).
2. Drag the **Extension** object onto the dashboard.
3. Choose **My Extensions** and select `manifest.trex`.
4. The extension finds the "Reserve" worksheet by name (case-insensitive) and reads
   its data via `getSummaryDataAsync()`. Field names are matched loosely (spaces/case
   are ignored), so Tableau's default captions like "So Qty" still match `so_qty`.
   If the worksheet isn't found, or its name changes, update `CONFIG.WORKSHEET_NAME`
   near the top of the `<script>` block in `reserve_aging.html`.
5. If the "Reserve" worksheet ever changes (a filter is applied elsewhere in the
   dashboard, for example), the extension picks up the change automatically.

### 3. Connect to BigQuery directly (fallback / alternative)

If you'd rather have the extension query BigQuery itself instead of reading from
a worksheet:

1. In [Google Cloud Console](https://console.cloud.google.com/), create an **OAuth Client ID**
   (Web application) for project `boonthavorn-data-analytic`.
2. Add your hosting origin (e.g. `https://<your-github-username>.github.io`) to
   **Authorized JavaScript origins**.
3. Open `reserve_aging.html`, find `CONFIG.GOOGLE_CLIENT_ID` near the top of the `<script>` block,
   and paste the Client ID in.
4. The `refreshData()` / `loadFromBigQuery()` functions that sign in and pull live data
   from BigQuery already exist in `reserve_aging.html`, but nothing calls them right
   now — wire one to a button (or trigger it automatically) if you want to use this
   path instead of the worksheet.

## License

Internal use — Boonthavorn.
