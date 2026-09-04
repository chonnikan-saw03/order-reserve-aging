# Order Reserve Aging Dashboard

A Tableau Dashboard Extension that connects to Google BigQuery
(`boonthavorn-data-analytic.tableau.ds_vdo_order_reserve_aging`) via Google OAuth
and shows Order Reserve Aging by branch, aging tier, and delivery priority.

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

1. Open Tableau Desktop, go to the dashboard.
2. Drag the **Extension** object onto the dashboard.
3. Choose **My Extensions** and select `manifest.trex`.

### 3. Connect real data (optional)

By default the dashboard shows mock data. To connect to BigQuery:

1. In [Google Cloud Console](https://console.cloud.google.com/), create an **OAuth Client ID**
   (Web application) for project `boonthavorn-data-analytic`.
2. Add your hosting origin (e.g. `https://<your-github-username>.github.io`) to
   **Authorized JavaScript origins**.
3. Open `reserve_aging.html`, find `CONFIG.GOOGLE_CLIENT_ID` near the top of the `<script>` block,
   and paste the Client ID in.
4. Reload the extension in Tableau. Note: the current UI has no visible "Refresh Data"
   button (it was removed in an earlier revision) — the `refreshData()` function that
   signs in and pulls live data still exists in `reserve_aging.html`, but nothing calls it yet.
   Wire it to a button (or trigger it automatically) before relying on live data.

## License

Internal use — Boonthavorn.
