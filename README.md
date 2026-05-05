# VIRESCO Bulk Liquid & DePak Reporting Portal

Standalone HTML reporting tool for VIRESCO AD LLC's two intake streams: Bulk Liquid (gallons, carbon-content method) and DePak Organics (tons, mass-based). Generates branded customer-specific or plant-wide PDF reports from Excel intake logs — entirely client-side, no backend.

## v2 — Enterprise Dashboard Rebuild (May 2026)

This release replaces v1's card-heavy layout with a PowerBI/Tableau-class dashboard:

- **Inter typography** with tabular numerals throughout — numbers right-align cleanly across all tables
- **Period-over-period deltas on every KPI** — automatically compares current period to prior comparable period (month vs prior month, quarter vs prior quarter, YTD vs same period last year)
- **Sparklines on every KPI tile** showing trajectory within the selected period
- **Tables instead of cards** for rankings and breakdowns — substantially more data per screen
- **Inline progress bars** in table cells for percentage values
- **Restrained palette** — full VIRESCO brand colors (green-700, green-500, yellow-500) but used with discipline; semantic green/red only for positive/negative deltas
- **No emojis anywhere** — replaced with disciplined typography and SVG marks
- **Tighter spacing rhythm** (4/8/16/24 px scale) and 1px borders instead of large rounded cards
- **PDF rebuild** — 5-page report matching the new aesthetic with embedded sparklines, period deltas, and methodology callouts

## Usage

1. Open `index.html` (or visit the deployed Vercel URL)
2. Upload the Bulk Liquid intake log (sheet `Intake Log`) and/or DePak intake log (sheet `Intake`)
3. Select the stream tab, customer/vendor filter, and reporting period
4. Browse five dashboard sections: Overview, Sustainability, Operations, Rankings, Intake Log
5. Click **Export PDF** to generate a branded report with full GHG methodology and embedded charts

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

## Deployment

```bash
cd viresco-portal
git init
git add .
git commit -m "v2 - enterprise dashboard rebuild"
git branch -M main
git remote add origin https://github.com/masoncolby/BulkLiquid-DePak-Reporting.git
git push -u origin main
```

Connect the GitHub repo to Vercel for auto-deploy. No build step required — `vercel.json` routes everything to `index.html`.

## Browser Requirements

Chrome, Edge, Safari, or Firefox — current versions. Uses XLSX.js, Chart.js, jsPDF, and jspdf-autotable from cdnjs.cloudflare.com. Inter font loaded from Google Fonts.
