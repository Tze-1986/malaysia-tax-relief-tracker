# Malaysia Tax & Finance Tracker

A single-file, offline-capable web app for tracking Malaysian personal income tax reliefs (LHDN/Hasil) **and** your everyday personal finances — bills, passive income, investment contributions and portfolio — across a Year of Assessment.

Everything — React, the Excel export engine (SheetJS), and the ZIP packer (JSZip) — is bundled inline in `index.html`. There is no build step and no server: it's one static file.

## What it does

**Tax relief**
- All LHDN individual relief categories with correct caps and subtopics, plus the automatic RM9,000 self relief.
- A **Household profile** toggle (Single/Married, No kids/Have kids) that auto-hides reliefs that don't apply — you can still switch any individual category back on.
- Rental income, loan interest and other side-income tracking.

**Personal finance**
- Recurring **bills** (housing, utilities, insurance, subscriptions, loans) and **passive income**, each with monthly, annually or one-time frequency.
- **Investment contributions** (EPF top-ups, PRS, unit trust, brokerage transfers) and an **investment portfolio** snapshot (current value vs. cost basis, gain/loss).
- Any bill or investment contribution can be **optionally linked to a tax relief category** (e.g. a life-insurance bill linked to "EPF & life insurance") — the linked amount counts toward that category's cap automatically, so you don't have to enter it twice.
- A **Dashboard** tab with monthly expenditure, passive income, investing and net cash flow, plus a rolling list of upcoming payments.
- **Amounts-hidden privacy toggle** (the eye icon in the header) masks headline totals with `•` — handy over someone's shoulder. Editable entry values in the tables stay visible so you can keep working.
- **Payment reminders**: every bill and investment row with a payment date has a "📅 .ics" button that downloads a calendar file (with a repeating rule for monthly/annual items and a 1-day-before alert) — double-click to add it to Outlook/Apple Calendar, or import it into Google Calendar via Settings → Import. This is the tracker's actual reminder mechanism: it's a static file with no server, so it can't open a live Google OAuth connection from inside itself. If you'd rather have entries appear on your Google Calendar automatically, the cleanest route is Google Calendar's own "Subscribe from URL"/Import flow using the downloaded `.ics` files.
- A missing-receipt warning surfaces on the Dashboard and Data tab any time an entry has an amount but no attached receipt. Any entry that genuinely doesn't need one (cash purchase, no receipt kept, etc.) can be marked **"No receipt"** — either via the checkbox right on its row, or with the one-click "No receipt needed" button in the warning list itself — which removes it from the alert until you uncheck it again.

**Shared**
- Receipts can be attached and saved directly to a folder you choose on your computer (Chrome/Edge only — uses the File System Access API).
- One-click export to a ZIP containing a consolidated Excel workbook (Overview, Tax Relief Detail, Other Income, Bills, Passive Income, Investment Contributions, Investment Portfolio sheets) plus copies of your receipts.
- Autosaves to the browser's local storage as you type.
- **Backup & transfer**: download a single `.json` snapshot covering everything — tax relief, bills, income, investments and portfolio — and import it on another device/browser to keep working there.

## Running it locally

No install needed — just open the file:

```bash
open index.html      # macOS
start index.html      # Windows
xdg-open index.html   # Linux
```

Or serve it (useful for testing the File System Access folder features, which some browsers restrict on `file://`):

```bash
npx serve .
```

## Deploying to Vercel

This repo needs zero configuration — it's a static `index.html`, and `vercel.json` is included for cache headers.

**Option A — Vercel dashboard (no CLI, no token needed):**
1. Push this repo to GitHub (see below).
2. Go to [vercel.com/new](https://vercel.com/new), click **Import Git Repository**, and select this repo.
3. Leave all settings as default (Framework Preset: *Other*) and click **Deploy**.
4. Vercel gives you a live URL (e.g. `malaysia-tax-relief-tracker.vercel.app`) and will auto-redeploy on every push to `main`.

**Option B — Vercel CLI:**
```bash
npm i -g vercel
vercel login
vercel --prod
```

## Pushing this repo to your GitHub account

From inside this folder:

```bash
git init                     # skip if already a git repo
git add .
git commit -m "Malaysia tax relief tracker"
git branch -M main
git remote add origin https://github.com/<your-username>/malaysia-tax-relief-tracker.git
git push -u origin main
```

(Create the empty repo first at [github.com/new](https://github.com/new) — name it `malaysia-tax-relief-tracker` and set it to **private** — then run the commands above.)

## Updating after changes

```bash
git add .
git commit -m "describe your change"
git push
```

Vercel (once connected to the repo) redeploys automatically on every push — no extra steps.

## Data & privacy

Everything runs client-side in your browser. Entries autosave to `localStorage` on whichever device/browser you're using; nothing is sent to a server. Receipts you attach are written to a folder on your own device, never uploaded. Calendar reminders are generated as local `.ics` files, not pushed to any account. The JSON backup/import feature is the supported way to move your data between devices.
