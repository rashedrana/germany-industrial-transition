# Germany Industrial Transition — Interactive Dashboard

The interactive companion to this repository's research paper
(*Still the Workshop of Europe? Germany's Industrial Transition, 1991–2024*).
Same data, same numbers — the paper explains the transition, this dashboard
lets you explore it.

## Web edition (live, no install)
Open `index.html` in a browser (or via GitHub Pages). Features:

- **Country buttons** — Germany plus France, Italy, Poland, the UK, the US,
  China, Japan, the EU and the world as the main view
- **Focus-year slider (1991–2024)** — every KPI card, the country comparison
  and the high-tech chart update to the selected year
- **Metric switch** — the trend chart flips between manufacturing share,
  manufacturing output and GDP per worker; per-country show/hide toggles
- **6 KPI cards** with change vs the start year (1991; high-tech exports vs
  2007, the first year published), an employment-mix stacked chart, an
  output-vs-jobs dual-axis chart, a productivity chart, and a 183-country
  scatter (manufacturing share vs income, 2023)
- Clicking a bar in the country comparison switches the whole dashboard to
  that country

## Power BI edition (build guide)
Everything needed to rebuild this as a native Power BI report sits in `powerbi/`:

- `powerbi/data/` — star-schema CSVs (4 fact tables, 4 dimensions; see
  `powerbi/model_diagram.md`). Two sizes: the paper's 10-country panel extract
  (340 country-years, 4,870 long fact rows), and the **full dataset — 261
  countries in the dimension, 5,339 long fact rows plus the 183-country 2023
  cross-section** (`*_Full.csv`), which the report is built on (default view:
  Germany). The source WDI extract is a focused panel + cross-section, so the
  full dataset is smaller than Project 1's global OWID extract — every row is
  a real published WDI value, nothing is padded.
- `powerbi/DAX_measures.md` — all measures, copy-paste ready (shares, output,
  employment, productivity, vs-1991 and YoY deltas, peak and rank measures)
- `powerbi/QUESTIONS.md` — 14 business questions, each solved with a different
  Power BI visual type (KPI cards, gauge, waterfall, treemap, scatter, map,
  ribbon, matrix…), with the verified answers
- `powerbi/layout_spec.md` — page-by-page layout in modern end-to-end project
  style: slicer header, KPI row, bookmark metric switching, drill-through
  country page, navigation buttons, exact color palette

On a Windows PC with Power BI Desktop: load the CSVs, set the relationships
in `model_diagram.md`, paste the DAX, and place the visuals per the layout spec.

## Data
World Bank World Development Indicators via `data/analysis_subset.csv` and
`industry.db` in this repository — 10 panel countries/aggregates × 34 years
(1991–2024), plus the 183-country 2023 cross-section used in the paper's
regression. Known WDI coverage gaps (e.g. China's manufacturing share starts
2004, high-tech exports start 2007) are kept as gaps, never filled.
