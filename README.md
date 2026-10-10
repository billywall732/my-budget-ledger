# My Budget Ledger

A single-file, no-build budget tracker for personal expenses and a wedding budget, backed by your own Google Drive — no server, no database, no account to create beyond your Gmail login.

**Live app:** https://billywall732.github.io/my-budget-ledger/

## Using the app

1. Open the live link above.
2. Click **Sync with Google Drive**.
3. Log in with your Gmail. If you see an "unverified app" warning, click **Advanced -> Go to My Budget App**.

Your data is stored in a single JSON file in your own Google Drive and is never seen by anyone else. The app only requests the `drive.file` scope, so it can only read/write the one file it creates for itself — it cannot see any of your other Drive files.

If you skip syncing, the app still works entirely offline using local browser storage; connecting Drive just adds backup/sync across devices.

## Features

- **Two trackers**: a personal monthly budget and a separate wedding budget, each with their own categories, subcategories, and history.
- **Dashboard**: spend-vs-budget by category, monthly trend chart, cash flow chart (income vs. expenses), and a monthly breakdown of where money went.
- **Expense & income log**: searchable/filterable transaction log with date, amount, category, and payment-method filters.
- **Payment methods & bank accounts**: track which card or account each transaction used, with running totals per account/card.
- **Subscriptions**: recurring expenses with billing cycles, tracked separately from one-off purchases.
- **Recurring income & salary tooling**: model paychecks (weekly/biweekly/semi-monthly/monthly) with pretax/tax deductions and see a net-pay breakdown.
- **Savings goals**: set targets and track progress toward them.
- **Vendors** (wedding tracker): tag expenses to vendors and see per-vendor totals.
- **Bulk import from Excel**: download an `.xlsx` template (with a Lists tab of your categories, cards, accounts, and vendors), fill it in, and upload it back. Every row is previewed with warnings and duplicate detection before anything is added.
- **CSV/JSON export & import**: back up or move your data outside of Drive sync.
- **Light/dark theme toggle** and a share button for sending the app link to others.

## Running it locally

This is a static single HTML file with no build step and no dependencies.

```
# from the repo root
python -m http.server 8080
# then open http://localhost:8080
```

Opening `index.html` directly from disk also works for everything except Google Drive sync, since Google requires an authorized origin (see below).

## Setting up your own Google Drive sync (optional, one-time)

The deployed app already has a configured Google OAuth client. If you fork this repo and deploy your own copy, you'll need your own client ID so Google will authorize your domain:

1. Go to https://console.cloud.google.com/ and create a project (free).
2. **APIs & Services -> Library** -> enable the **Google Drive API**.
3. **APIs & Services -> OAuth consent screen** -> set it up as "External", add yourself as a test user (or publish it — it's just for you).
4. **APIs & Services -> Credentials -> Create Credentials -> OAuth client ID** -> Application type "Web application".
5. Under "Authorized JavaScript origins" add the exact URL this page will be served from, e.g. `https://yourname.github.io` (no trailing slash, no path). Add `http://localhost:PORT` too if you test locally.
6. Copy the Client ID (ends in `.apps.googleusercontent.com`) and paste it into the `GOOGLE_CLIENT_ID` constant near the top of `index.html`.
7. Commit and push — GitHub Pages will pick it up automatically.

## Tech

Plain HTML/CSS/JavaScript in a single file (`index.html`), no framework, no build tooling, no package manager. Deployed via GitHub Pages.
