# Malaysia Tax & Finance Tracker

A single-file, offline-capable web app for tracking Malaysian personal income tax reliefs (LHDN/Hasil) **and** your everyday personal finances — bills, passive income, investment contributions and portfolio — across a Year of Assessment.

Everything — React, the Excel export engine (SheetJS), and the ZIP packer (JSZip) — is bundled inline in `index.html`. There is no build step and no server: it's one static file.

## What it does

**Tax relief**
- All LHDN individual relief categories with correct caps and subtopics, plus the automatic RM9,000 self relief.
- A **Household profile** toggle (Single/Married, No kids/Have kids) that auto-hides reliefs that don't apply — you can still switch any individual category back on.
- **Rental income & loan interest** is now recurring-capable, just like a bill: give it a start date and an "Ends" condition, and it expands into a monthly/annual schedule with its own per-occurrence receipts and `.ics` reminders. Rental income also automatically counts toward the Personal Finance "Passive income" totals — and shows up as its own read-only line inside the Passive income card on the Bills tab, with an "Edit" shortcut back to the Tax tab, so it's visibly included, not just silently folded into the number.
- **Education & medical insurance** is also recurring-capable. For a policy that bundles medical/critical-illness cover into a life or personal-accident policy, fill in both the medical premium and the "Life portion" field — the app then counts only 60% of the medical premium toward this relief, per LHDN's treatment of CI riders on life/PA policies (the life portion itself isn't auto-added anywhere; log it separately under "EPF & life insurance" if you want that relief too). Leave the life portion blank for a stand-alone medical policy and the full premium counts, as before.
- Other side-income tracking (freelance, business income, etc.).

**Personal finance**
- Recurring **bills** (housing, utilities, insurance, subscriptions, loans) and **passive income**, each with monthly, annually or one-time frequency, kept in their own tab and their own dashboard section — separate from investments throughout, so payments and investing never get blended together in the totals.
- **Investment contributions** (EPF top-ups, PRS, unit trust, brokerage transfers) and an **investment portfolio** snapshot (current value vs. cost basis, gain/loss).
- Recurring items can be given a **start date and an end condition** — never-ending, on a specific end date, or after a set number of occurrences (e.g. "12 months" or "5 years") — and each one has an expandable **Items & receipts** list showing every individual occurrence it generates: month + year for monthly items, year alone for annual ones.
- **Per-occurrence receipts**: each individual occurrence of a recurring bill or investment contribution — not just the entry as a whole — gets its own "Attach"/"Replace" button and its own "No receipt" checkbox. Attach January's rent receipt without it applying to February, or mark a single month exempt without exempting the whole series.
- **YTD vs. year-end figures, calculated correctly**: the Dashboard shows Payments and Investments as two separate sections, each with a "Paid/Contributed YTD" figure (occurrences that have actually happened so far this year) and an "Expected year-end" figure (what the full year adds up to once every remaining occurrence lands), plus a per-item breakdown so you can see which entries are driving the totals. Every occurrence is scoped strictly to the calendar year it falls in — an item logged for next year or carried over from last year is counted in that year, not lumped into the current one.
- Any bill or investment contribution can be **optionally linked to a tax relief category** (e.g. a life-insurance bill linked to "EPF & life insurance"). Linking auto-creates the matching entries inside that tax relief category — broken down the same way, one per occurrence within the selected assessment year — and stays in sync automatically: edit the bill's amount, dates or link and the tax relief side updates immediately, with nothing to duplicate by hand.
- A **Dashboard** tab with monthly expenditure, passive income (incl. rental), investing and net cash flow, plus a rolling list of upcoming payments. A dedicated "Expected monthly expenditure" vs. "Expected monthly income" pair of cards gives a quick side-by-side read before the detailed breakdown. Only entries currently within their active start/end window count toward these totals — a bill that's already ended won't inflate your monthly expenditure.
- **Amounts-hidden privacy toggle** (the eye icon in the header) masks headline totals with `•` — handy over someone's shoulder. Editable entry values in the tables stay visible so you can keep working.
- **Payment reminders**: every bill and investment row with a payment date has a "📅 .ics" button that downloads a calendar file (with a repeating rule for monthly/annual items and a 1-day-before alert) — double-click to add it to Outlook/Apple Calendar, or import it into Google Calendar via Settings → Import. This is the tracker's actual reminder mechanism: it's a static file with no server, so it can't open a live Google OAuth connection from inside itself. If you'd rather have entries appear on your Google Calendar automatically, the cleanest route is Google Calendar's own "Subscribe from URL"/Import flow using the downloaded `.ics` files.
- A missing-receipt warning surfaces on the Dashboard and Data tab any time an occurrence has an amount but no attached receipt — flagged per occurrence, so a monthly bill with one missing month shows just that month, not the whole series. Any occurrence that genuinely doesn't need one (cash purchase, no receipt kept, etc.) can be marked **"No receipt"** — either via the checkbox on that occurrence's own row, or with the one-click "No receipt needed" button in the warning list itself — which removes it from the alert until you uncheck it again.
- Assessment/calendar years covered by the app run **2023 through 2028**.

**Shared**
- Receipts can be attached and saved directly to a folder you choose on your computer (Chrome/Edge only — uses the File System Access API).
- One-click export to a ZIP containing a consolidated Excel workbook (Overview, Tax Relief Detail, Other Income, Bills, Passive Income, Investment Contributions, Investment Portfolio sheets) plus copies of your receipts. The Bills and Investment Contributions sheets list one row per occurrence for the current year (not one row per entry), each marked Paid/occurred or Upcoming, with paid-YTD and expected-year-end totals rolled up at the bottom and echoed on the Overview sheet.
- Autosaves to the browser's local storage as you type.
- **Backup & transfer**: download a single `.json` snapshot covering everything — tax relief, bills, income, investments and portfolio — and import it on another device/browser to keep working there.
- Entry name fields and table text are sized for phones (16px, which also stops iOS Safari's auto-zoom-on-focus) so they stay readable on mobile.

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
