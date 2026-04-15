# Receipt Tracker

LLC tax receipt tracker — captures receipt images, uses Claude AI to extract vendor, date, line items, category, and totals, then saves images to Google Drive and appends rows to a Google Sheet.

## Features
- 📸 Upload or drag-and-drop receipt photos / PDFs
- 🤖 AI extraction via Claude (Anthropic API)
- ☁️ Images saved to Google Drive (year subfolder auto-created)
- 📊 Data logged to "Receipts {year}" Google Sheet
- 🏷️ Category breakdown & summary view
- 💾 localStorage cache for offline access

## Setup
1. Open the app and go to **Settings**
2. Enter your [Anthropic API key](https://console.anthropic.com)
3. Click **Connect Google Account** (requires a Google OAuth Client ID from [Google Cloud Console](https://console.cloud.google.com))
4. The Google Drive folder ID is pre-configured

## Usage
- **Upload tab** — drop receipt images or PDFs
- **Review & Save** — verify/correct AI-extracted data, then save to Drive + Sheets
- **Ledger** — filter & browse all saved receipts
- **Summary** — totals by category for tax reporting
