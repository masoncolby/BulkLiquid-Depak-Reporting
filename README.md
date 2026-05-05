# VIRESCO Bulk Liquid & DePak Reporting Portal

Standalone HTML reporting tool for VIRESCO AD LLC's two intake streams: Bulk Liquid (gallons, carbon-content method) and DePak Organics (tons, mass-based). Generates branded customer-specific or plant-wide PDF reports from Excel intake logs — entirely client-side, no backend.

## v3 — Multi-File Consolidated Reporting (May 2026)

This release adds the ability to consolidate intake logs across multiple years into single lifetime reports.

- **Multiple files per stream** — upload bulk-2024.xlsx, bulk-2025.xlsx, bulk-2026.xlsx (and so on); the portal merges them into one dataset
- **Automatic deduplication** — records are deduped by date + vendor/customer + volume (case-insensitive), so accidentally uploading overlapping files won't double-count
- **File list with remove buttons** — see what's loaded, remove a file with a single click, dataset updates immediately
- **Lifetime period** — new period button that spans the earliest to latest record across all loaded files. Daily volume chart auto-buckets to monthly when range exceeds 1 year. No period-over-period delta on Lifetime view (nothing to compare to); KPIs show "all-time total" instead.
- **Name similarity detection** — after upload, the portal scans vendor/customer names for likely duplicates ("Monster Energy" vs "Monster CS", "Pepsi-Cola" vs "Pepsi Cola") and shows a dismissible warning banner with the candidates. The portal won't auto-merge them — that's intentionally a manual decision — but you'll know to clean source files if needed.

## v2 — Enterprise Dashboard Foundation (May 2026)

- Inter typography with tabular numerals throughout
- Period-over-period deltas on every KPI (month vs prior month, quarter vs prior quarter, YTD vs same period last year, annual vs prior year, custom vs equal-length window before)
- Sparklines on every KPI tile showing trajectory within the selected period
- Tables for rankings/breakdowns instead of cards (more data density)
- Inline progress bars in table cells for percentage values
- Restrained palette — full VIRESCO brand colors used with discipline; semantic green/red only for delta indicators
- No emojis anywhere
- 5-page PDF report matching the dashboard aesthetic

## Usage

1. Open `index.html` (or visit the deployed Vercel URL)
2. For each stream, click "+ Add Bulk Liquid file" or "+ Add DePak file" — you can select multiple files at once. Repeat to add more.
3. Click **Continue to Dashboard** when ready
4. Select stream tab, customer/vendor filter, and reporting period (Month / Quarter / YTD / Annual / **Lifetime** / Custom)
5. Browse five dashboard sections: Overview, Sustainability, Operations, Rankings, Intake Log
6. Click **Export PDF** to generate a branded report

## File Structure

| File | Purpose |
|------|---------|
| `index.html` | Single-file portal (HTML + CSS + JS + PDF generator inlined) |
| `vercel.json` | Vercel routing config for static deployment |
| `README.md` | This file |

## Sustainability Factor Sheet

**DePak (mass-based):**
- GHG: tons x 0.62 Tons CO2e/ton (CARB ANDOC)
- Biogas: tons x 100 m3 x 73% digester efficiency
- Energy: m3 x 4 kWh/m3 (40% genset baked in)
- NPK: tons x 15 kg

**Bulk Liquid (carbon-content based):**
- Density: 8.34 lbs/gal · Carbon content: 20,000 ppm
- Biogas: lbs C x 23 SCF · Energy: lbs C x 1.34 kWh
- GHG: gal x 8.34 / 2,000 x 0.62 (mass-based, unified across both streams)
- ReNutrient digestate: gallons x 95%

**Universal equivalencies:**
- Households powered: MMBtu / 32.7
- Cars off road: Tons CO2e / 0.383
- Tree-years: Tons CO2e / 0.06
- Vehicle miles avoided: Tons CO2e x 2,481

## Required Excel Columns

**Bulk Liquid** — `Date`, `Vendor`, `Gallons` (required); `Time`, `Strength`, `Tank`, `Driver`, `Ticket #`, `Special Notes` (optional)

**DePak** — `Date`, `Customer`, `Tons` (required); `Time`, `Product`, `Weight (lbs)`, `Quantity`, `Units`, `Truck #`, `Trucking Company`, `Customer BOL#` (optional)

## Deduplication Rules

When you upload multiple files for the same stream, records are merged with this dedup key:
- **Bulk Liquid:** `YYYY-MM-DD | vendor (lowercased) | rounded gallons`
- **DePak:** `YYYY-MM-DD | customer (lowercased) | tons (2 decimals)`

If two records collide on this key, the second one is dropped silently. The merge summary line tells you how many duplicates were removed. Records that legitimately differ (different gallons, different vendor, etc.) are kept independently — no fuzzy matching, only exact-key matching.

## Deployment

```bash
cd viresco-portal
git init
git add .
git commit -m "v3 - multi-file lifetime reporting"
git branch -M main
git remote add origin https://github.com/masoncolby/BulkLiquid-DePak-Reporting.git
git push -u origin main
```

Connect the GitHub repo to Vercel for auto-deploy. No build step required — `vercel.json` routes everything to `index.html`.

## Browser Requirements

Chrome, Edge, Safari, or Firefox — current versions. Uses XLSX.js, Chart.js, jsPDF, and jspdf-autotable from cdnjs.cloudflare.com. Inter font loaded from Google Fonts.
