# Kembara Fuel Tracker

A single-page web app for tracking fuel fill-ups on a Perodua Kembara. Logs every pump stop, auto-calculates km driven, and shows weekly/monthly spend, litres, and fuel efficiency (km/L) — all synced to Google Sheets with no backend.

**Live app:** `https://albert9108.github.io/kembara-fuel-tracker/`

---

## Features

- Log fill-ups: date, odometer, litres, amount (RM), notes
- Auto-calculates km driven from odometer difference
- Weekly and monthly summaries: spend, litres, km, km/L
- Full history view with per-entry efficiency badge
- Efficiency bar — green ≥ 13 km/L, orange 10–12.9, red < 10
- Data stored permanently in Google Sheets (no backend needed)
- Works on mobile and desktop

## How It Works

Data is saved to a Google Sheet via the Sheets API using OAuth2 (browser-only, no server). Every page load fetches fresh data from the sheet.

```
Fill-up form → auto-calc km driven → append to Google Sheet → render all tabs
```

**km/L tip:** For accurate per-fill efficiency, always fill to the brim. Mixing half and full tanks skews individual readings — but cumulative totals stay correct.

---

## Config

At the top of the `<script>` block in `index.html`:

```js
const CLIENT_ID = '...';       // Google OAuth2 client ID
const SHEET_ID  = '...';       // Google Sheets ID
const BASELINE  = 13;          // km/L target (Kembara typical: 13–15)
const ODO_START = 304323;      // starting odometer (7 Jun 2025)
```

## Google Sheets Structure

| Col | Field | Example |
|-----|-------|---------|
| A | Date | 2025-06-09 |
| B | Odometer_km | 304680 |
| C | Litres | 30.00 |
| D | Amount_RM | 65.40 |
| E | KM_Driven | 357 |
| F | Notes | RON95, Petron |

Row 1 = headers (auto-created on first save).

---

## Deployment

Push to `main` → GitHub Pages deploys automatically in ~1 minute. No build step.

```bash
git add index.html
git commit -m "your change"
git push origin main
```

## Stack

- Pure HTML + CSS + JavaScript (single file, no framework)
- [Google Sheets API v4](https://developers.google.com/sheets/api)
- [Google Identity Services](https://developers.google.com/identity) — OAuth2 token flow
- Hosted on GitHub Pages
