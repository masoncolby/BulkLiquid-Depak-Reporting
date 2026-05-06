# VIRESCO Bulk Liquid & DePak Reporting Portal

Standalone HTML reporting tool for VIRESCO AD LLC's two intake streams: Bulk Liquid (gallons, carbon-content method) and DePak Organics (tons, mass-based). Generates branded customer-specific or plant-wide PDF reports from Excel intake logs — entirely client-side, no backend.

## v4 — Consulting-Grade PDF Rebuild (May 2026)

The PDF report has been completely redesigned to match the visual quality and structural rigor of VIRESCO's premium client deliverables (e.g., the Denali Target Midwest Organics report).

- **Dark green page header bars** (green-900) on every page with VIRESCO branding, stream identifier, and page numbering — replaces the thin top accent
- **Yellow accent line** below each header bar for brand pop
- **Section title bars** with green-700 background, white headline, and subtitle on the right (e.g., "EXECUTIVE SUMMARY  /  Apr 2026")
- **Big-number sustainability callouts** — 4 visual anchor tiles showing real-world equivalencies (cars off road, trees planted, homes powered, miles avoided) with colored accent stripes
- **Auto-generated narrative paragraphs** below each section title — pulls real numbers from the data and writes contextual prose so reports read like consulting deliverables, not raw exports
- **Color-coded KPI tiles** in 3x2 grid with green stripes for primary metrics and yellow stripes for operational metrics
- **Footer with confidentiality marking, customer name, and report ID** on every page
- **Encoding fixes** — CO2e, m3, CH4 now render cleanly (jsPDF Helvetica WinAnsi limitation worked around)
- **Smart delta suppression** — when the prior period has no/minimal data, KPIs show "first reporting period" instead of misleading huge percentages
- **About-this-report block** at the end with VIRESCO facility context

## v3.1 — Vendor Merge Tool

- Merge similar vendor/customer names ("Cady" / "cady", "Northstar Recycling" / "North Star Recycling") via inline merge buttons in the warning banner
- Choose canonical name (radio: A | B | custom text input)
- Active merge rules display as chips at the bottom of the banner with one-click undo
- Session-only — merges reset on page reload

## v3 — Multi-File Consolidated Reporting

- Multiple files per stream — upload bulk-2024.xlsx, bulk-2025.xlsx, bulk-2026.xlsx and they merge automatically
- Auto-deduplication by date + vendor/customer + volume
- File list with per-file remove buttons
- Lifetime period — spans earliest to latest record across all loaded files
- Daily volume chart auto-buckets to monthly when range exceeds 1 year
- Name similarity detection (Levenshtein-based)

## v2 — Enterprise Dashboard Foundation

- Inter typography with tabular numerals
- Period-over-period deltas on every KPI
- Sparklines on every KPI tile
- Tables for rankings/breakdowns instead of cards
- Restrained palette using full VIRESCO brand colors

## Usage

1. Open `index.html` (or visit the deployed Vercel URL)
2. For each stream, click "+ Add Bulk Liquid file" or "+ Add DePak file" — multi-select supported in the picker
3. Click **Continue to Dashboard** when ready
4. If the warning banner appears with similar names, click **Merge ->** and choose canonical
5. Select stream tab, customer/vendor filter, and reporting period (Month / Quarter / YTD / Annual / Lifetime / Custom)
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
- Density: 8.34 lbs/gal | Carbon content: 10,000 ppm
- Biogas: lbs C x 23 SCF | Energy: lbs C x 1.34 kWh
- GHG: gal x 8.34 / 2,000 x 0.62
- ReNutrient digestate: gallons x 95%

**Universal equivalencies:**
- Households powered: MMBtu / 32.7
- Cars off road: Tons CO2e / 0.383
- Tree-years: Tons CO2e / 0.06
- Vehicle miles avoided: Tons CO2e x 2,481

## Deployment

```bash
cd viresco-portal
git add .
git commit -m "v4 - consulting-grade PDF rebuild"
git push
```

Connect the GitHub repo to Vercel for auto-deploy. No build step required.

## Browser Requirements

Chrome, Edge, Safari, or Firefox — current versions. Uses XLSX.js, Chart.js, jsPDF, and jspdf-autotable from cdnjs.cloudflare.com. Inter font loaded from Google Fonts.
