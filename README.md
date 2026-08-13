# Malaysia Tax Relief Tracker

A single-file, offline-capable web app for tracking Malaysian personal income tax reliefs (LHDN/Hasil), rental & other income, and receipts across a Year of Assessment.

Everything — React, the Excel export engine (SheetJS), and the ZIP packer (JSZip) — is bundled inline in `index.html`. There is no build step and no server: it's one static file.

## What it does

- All LHDN individual relief categories with correct caps and subtopics, plus the automatic RM9,000 self relief.
- A **Household profile** toggle (Single/Married, No kids/Have kids) that auto-hides reliefs that don't apply — you can still switch any individual category back on.
- Rental income, loan interest and other side-income tracking.
- Receipts can be attached and saved directly to a folder you choose on your computer (Chrome/Edge only — uses the File System Access API).
- One-click export to a ZIP containing a consolidated Excel workbook (Overview / Detail / Other Income sheets) plus copies of your receipts.
- Autosaves to the browser's local storage as you type.
- **Backup & transfer**: download a `.json` snapshot of all your entries and settings, and import it on another device/browser to keep working there.

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

Everything runs client-side in your browser. Entries autosave to `localStorage` on whichever device/browser you're using; nothing is sent to a server. Receipts you attach are written to a folder on your own device, never uploaded. The JSON backup/import feature is the supported way to move your data between devices.
