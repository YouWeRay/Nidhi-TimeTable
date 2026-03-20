# 📅 Time Table Dashboard

> A live, Google Sheets-powered timetable with Day, Week, Month & Year views — plus Google Calendar sync. No backend, no database, zero maintenance.

**🌐 Live:** [youweray.github.io/TimeTable](https://youweray.github.io/TimeTable/)

---

## What it does

Connect any Google Sheet timetable and get an instant dashboard. Update the sheet → refresh the page → done. Events also sync automatically to Google Calendar on every device.

| View | What you see |
|------|-------------|
| **Day** | 24-hour Google Calendar-style timeline with a live "now" indicator |
| **Week** | Compact 7-column grid — click any day to open Day view |
| **Month** | Full calendar with real events from the sheet — click any day to drill in |
| **Year** | 12-month heatmap — dark blue = days with data, click any month to open it |

---

## Repository structure

```
/
├── index.html          ← Entire dashboard (single self-contained file)
├── timetable_ics.gs    ← Google Apps Script — serves live .ics calendar feed
├── update_gist.gs      ← Google Apps Script — pushes .ics to GitHub Gist for Google Calendar
├── Rishabh_TimeTable.xlsx  ← Sample sheet for testing
└── README.md           ← This file
```

---

## Google Sheet format

```
Row 1:  Title    →  e.g. "Rishabh Time Table"
Row 2:  Headers  →  Date | Day | 9:30 AM – 11:00 AM | 11:00 AM – 12:30 PM | ...
Row 3+: Data     →  23-03-2026 | Monday | Sprint review | Team standup | ...
```

| Column | Content | Format |
|--------|---------|--------|
| A | Date | `DD-MM-YYYY` |
| B | Day name | `Monday` … `Sunday` |
| C onwards | Time slots | Header must include time range e.g. `9:30 AM – 11:00 AM` |

- Put `WEEKLY OFF` in any cell of a row to mark it as a day off
- Leave cells empty for slots with no activity
- Add as many time slot columns as needed — all auto-detected

---

## Part 1 — Dashboard setup

### Step 1 — Get the files

Download or clone this repository. You need two files:
- `index.html` — the dashboard
- `Rishabh_TimeTable.xlsx` — sample sheet (optional, for testing)

### Step 2 — Prepare your Google Sheet

1. Upload `Rishabh_TimeTable.xlsx` to Google Drive
2. Open it → click **File → Save as Google Sheets**
3. Structure your data following the format above
4. Make sure Row 1 has your title, Row 2 has headers, Row 3 onwards has data

### Step 3 — Publish the sheet as CSV

1. Click **File → Share → Publish to web**
2. First dropdown: select your sheet tab (e.g. `Sheet1`)
3. Second dropdown: select `Comma-separated values (.csv)`
4. Click **Publish** → copy the link

The link looks like:
```
https://docs.google.com/spreadsheets/d/e/LONG_ID/pub?gid=0&single=true&output=csv
```

### Step 4 — Host the dashboard on GitHub Pages

1. Create a free account at [github.com](https://github.com) if you don't have one
2. Click **+** → **New repository**
3. Name it `TimeTable`, set it to **Public**
4. Click **Create repository**
5. Click **uploading an existing file** → drag and drop `index.html` → rename it to `index.html` → click **Commit changes**
6. Go to **Settings → Pages**
7. Under Branch, select **main** → **/ (root)** → click **Save**
8. Wait 1–2 minutes → your dashboard is live at `https://yourusername.github.io/TimeTable/`

### Step 5 — Connect the sheet

1. Open your dashboard URL
2. Paste your CSV link from Step 3 into the input box
3. Click **Open →**

The link is saved in the browser. Every future visit loads automatically.

---

## Part 2 — Google Calendar sync (optional)

This makes all timetable events appear in Google Calendar on your phone, laptop, and watch — auto-updating when the sheet changes.

### How it works

```
Google Sheet
    ↓  (Apps Script reads it)
GitHub Gist (.ics file, public URL)
    ↓  (Google Calendar subscribes)
Google Calendar on all devices ✓
```

### Step 1 — Open Apps Script

1. Open your Google Sheet
2. Click **Extensions → Apps Script**
3. A new tab opens with the code editor

> **Note:** If you don't see "Extensions", your file is still in .xlsx format.
> Fix: Click **File → Save as Google Sheets** first.

### Step 2 — Add the ICS script

1. In Apps Script, delete all default code (Ctrl+A → Delete)
2. Paste the entire contents of `timetable_ics.gs`
3. Press **Ctrl+S** to save
4. Click the **+** next to "Files" in the left panel → name it `UpdateGist`
5. Paste the entire contents of `update_gist.gs`
6. Press **Ctrl+S**

### Step 3 — Create a GitHub token

1. Go to [github.com/settings/tokens](https://github.com/settings/tokens)
2. Click **Generate new token (classic)**
3. Name it `Timetable ICS`
4. Tick only the **`gist`** scope
5. Click **Generate token** → **copy it** (you only see it once)

### Step 4 — Add your token to the script

In `UpdateGist.gs`, find line 9 and replace `YOUR_GITHUB_TOKEN_HERE` with your token:

```js
var GITHUB_TOKEN = 'ghp_yourActualTokenHere';
```

Press **Ctrl+S** to save.

### Step 5 — Run the script for the first time

1. In the function dropdown at the top (next to ▶ Run), select `updateGist`
2. Click ▶ **Run**
3. If asked for permissions → click **Authorise access** → choose your Google account → click **Advanced** → **Go to timetable_ics (unsafe)** → **Allow**
4. Click **Execution log** (or View → Logs)

You will see three lines:
```
Gist ID:       4a3c680889b179e3983fa212ab9187dc
Raw ICS URL:   https://gist.githubusercontent.com/YouWeRay/...
Subscribe URL: webcal://gist.githubusercontent.com/YouWeRay/...
```

**Copy the Subscribe URL** (the `webcal://` one).

### Step 6 — Save your Gist ID

In `UpdateGist.gs`, find line 10 and paste your Gist ID:

```js
var GIST_ID = '4a3c680889b179e3983fa212ab9187dc';  // ← paste yours here
```

Press **Ctrl+S**.

### Step 7 — Subscribe in Google Calendar

1. Open [calendar.google.com](https://calendar.google.com)
2. Left sidebar → **Other calendars** → click **+**
3. Click **From URL**
4. Paste your `webcal://gist.githubusercontent.com/...` URL
5. Click **Add calendar**

Your timetable appears immediately in Google Calendar on all devices. ✅

### Step 8 — Set up auto-update

1. Back in Apps Script, change the function dropdown to `createTrigger`
2. Click ▶ **Run**

The Gist now updates every hour automatically. Google Calendar syncs from it every 6–24 hours.

---

## Day-to-day workflow

| Who | Does what | Result |
|-----|-----------|--------|
| You | Update the Google Sheet | — |
| You | Click ↻ Refresh on the dashboard | Dashboard shows latest data instantly |
| Google | Polls the Gist every 6–24 hours | Google Calendar updates on all devices |

---

## Admin controls

| Control | How |
|---------|-----|
| **↻ Refresh** | Top-right button — fetches latest sheet data |
| **Change sheet** | Top-right button — switch to a different sheet |
| **Share button** | Top-right — copy link, QR code, subscribe to Google Calendar, download .ics |
| **Hidden URL bar** | Press `Shift + U` anywhere on the dashboard |

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "Extensions" not in menu | File is .xlsx — click File → Save as Google Sheets first |
| "Cannot fetch" on dashboard | Publish sheet as CSV: File → Share → Publish to web |
| Google Calendar says "unable to add" | Use the `webcal://gist.githubusercontent.com/...` URL, not the Apps Script URL |
| Events not showing in Google Calendar | Google takes up to 24 hours for first sync. Remove and re-add the calendar to force it |
| Script changes not reflected | Create a **New deployment** in Apps Script after every code change |
| Gist not updating | Make sure `GIST_ID` is filled in `UpdateGist.gs` and the trigger was created |
| Dashboard shows old data | Click ↻ Refresh or hard-refresh with Ctrl+Shift+R |
| Events missing from Day timeline | Column headers must include a time range e.g. `9:30 AM – 11:00 AM` |
| Title shows "Your Time Table" | Add your title text in Row 1 of the sheet |
| Dates appear wrong | Column A must use `DD-MM-YYYY` format |

---

## Use it for your own timetable

This dashboard is fully universal. To use it for a different person:

1. Fork this repo or download `index.html`
2. Host it on GitHub Pages — it's a single file
3. Open it and connect your own Google Sheet via the welcome screen
4. Follow Part 2 above to set up Google Calendar sync with your own sheet

No code changes required. Everything is read dynamically from your sheet.

---

## Tech stack

| | |
|-|-|
| Dashboard | HTML + CSS + Vanilla JavaScript (single file) |
| Data source | Google Sheets published CSV |
| Calendar sync | Google Apps Script + GitHub Gist + webcal |
| Hosting | GitHub Pages (free) |
| Dependencies | Google Fonts only |

---

## License

MIT — free to use, fork, and adapt.

---

*Built with Google Sheets + GitHub Pages + Apps Script. No servers, no costs, no maintenance.*

