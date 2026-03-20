[README.md](https://github.com/user-attachments/files/26136097/README.md)
# 📅 Time Table Dashboard

> A live, Google Sheets-powered timetable with Day, Week, Month & Year views — no backend, no database, zero maintenance.

**🌐 Live:** [youweray.github.io/TimeTable](https://youweray.github.io/TimeTable/)

---

## What it does

Connect any Google Sheet timetable and get an instant dashboard. Every time the sheet is updated, the user just refreshes the page — no re-deployment, no uploads, no code.

| View | What you see |
|------|-------------|
| **Day** | 24-hour Google Calendar-style timeline with a live "now" indicator |
| **Week** | Compact 7-column grid — click any day to open Day view |
| **Month** | Full calendar with real events from the sheet — click any day to drill in |
| **Year** | 12-month heatmap — dark blue = days with data, click any month to open it |

---

## Google Sheet Format

Your sheet must follow this structure:

```
Row 1:  Title    →  e.g. "Rishabh's Time Table"  (becomes the dashboard heading)
Row 2:  Headers  →  Date | Day | 9:30 AM – 11:00 AM | 11:00 AM – 12:30 PM | ...
Row 3+: Data     →  16-02-2026 | Monday | Paper Prep | Online Class | ...
```

| Column | Content | Notes |
|--------|---------|-------|
| A | Date | Format: `DD-MM-YYYY` |
| B | Day name | `Monday`, `Tuesday` … `Sunday` |
| C | First time slot | Header must include a time range: `9:30 AM – 11:00 AM` |
| D, E … | More time slots | Add as many columns as needed — all auto-detected |

**Special rows:**
- Put `WEEKLY OFF` anywhere in a row (or use `Sunday` in column B) to mark a day off
- Leave cells empty for slots with no activity — the dashboard shows nothing for them

> **Tip:** Time slot headers must include a parseable time range for events to appear correctly on the Day timeline. They still show in Week and Month views regardless.

---

## Setup (~3 minutes, one time only)

### 1. Prepare your Google Sheet

Structure your sheet as shown above. A ready-to-use sample file — **`Rishabh_TimeTable.xlsx`** — is included in this repo. Upload it to Google Sheets to get started instantly.

### 2. Publish as CSV

1. Open your Google Sheet
2. Go to **File → Share → Publish to web**
3. First dropdown: select your sheet tab (e.g. `Sheet1`)
4. Second dropdown: select **Comma-separated values (.csv)**
5. Click **Publish** → copy the link

The link looks like:
```
https://docs.google.com/spreadsheets/d/e/LONG_ID/pub?gid=0&single=true&output=csv
```

### 3. Connect to the dashboard

1. Open [youweray.github.io/TimeTable](https://youweray.github.io/TimeTable/)
2. Paste the CSV link into the input box
3. Click **Open →**

The link is saved in the browser. Every future visit loads automatically — no re-entering needed.

---

## Day-to-day workflow

```
Update the Google Sheet  →  Refresh the dashboard page  →  Done ✓
```

No code, no uploads, no re-deployment. Ever.

---

## Adding new time slots

The dashboard auto-detects all columns from the header row. To add a new session:

1. Add a new column in the sheet with a time range as the header
   (e.g. `6:00 PM – 8:00 PM`)
2. Fill in the data
3. Refresh the dashboard

The new slot appears in all views automatically with a unique colour. No code changes needed.

---

## Admin controls

| Control | How |
|---------|-----|
| **↻ Refresh** | Button top-right — fetches latest data without a full page reload |
| **Change sheet** | Button top-right — clears the saved URL and returns to the welcome screen |
| **Hidden URL bar** | Press `Shift + U` anywhere on the dashboard to reveal it — useful for updating the sheet URL without going through onboarding |

---

## How it works

```
Google Sheet
     │
     │  published as CSV (File → Share → Publish to web)
     ▼
fetch() on every page load
     │
     │  browser parses CSV, auto-detects time slot columns
     ▼
Renders Day / Week / Month / Year — entirely in the browser
```

- **No backend** — pure HTML + CSS + Vanilla JavaScript
- **No API keys** — uses Google Sheets' built-in CSV publish feature
- **No build step** — single `index.html`, open and it works
- **No dependencies** — only Google Fonts loaded via CDN

---

## File structure

```
/
├── index.html              ← Entire dashboard (one self-contained file, ~22 KB)
├── Rishabh_TimeTable.xlsx  ← Sample sheet for testing all features
└── README.md               ← This file
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "Cannot fetch" error | Publish the sheet first: File → Share → Publish to web → CSV |
| Dashboard shows old data | Click **↻ Refresh** or hard-refresh with `Ctrl+Shift+R` |
| Events missing from Day timeline | Column header must include a time range, e.g. `9:30 AM – 11:00 AM` |
| Title shows "Your Time Table" | Put the title text in Row 1 of the sheet (row before the headers) |
| Dates appear wrong | Column A must use `DD-MM-YYYY` format |
| Month / Year shows no data | Verify column A has actual dates, not just day names |
| Loads the wrong week | Dashboard automatically picks the week in the sheet closest to today |

---

## Use it for your own timetable

This dashboard is fully universal. To use it for a different person or organisation:

1. Fork this repo or download `index.html`
2. Host it on GitHub Pages, Netlify, or any static host — it's a single file
3. Open it and connect any Google Sheet via the welcome screen

Everything — the title, column count, time slots, and colour scheme — is read dynamically from the sheet. No code changes required.

---

## Tech stack

| | |
|-|-|
| Language | HTML + CSS + Vanilla JavaScript |
| Hosting | GitHub Pages (free) |
| Data source | Google Sheets published CSV |
| External dependencies | Google Fonts only |
| Browser support | All modern browsers |
| File size | ~22 KB (single file) |

---

## License

MIT — free to use, fork, and adapt.

---

*Built with Google Sheets + GitHub Pages. No servers, no costs, no maintenance.*
