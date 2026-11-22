# Power BI Layout Spec — Germany Industrial Transition Dashboard

Design language follows modern end-to-end Power BI project style (cf. "The Developer"
YouTube channel's end-to-end dashboard projects): a clean canvas, a slicer header,
rounded KPI cards, bookmark-driven metric switching and page-navigation buttons.
The web edition in `dashboard/index.html` is the visual reference — build the PBIX
to match it page for page.

## Global formatting
- Page size: 16:9 (1280 × 720)
- Background: #F7F6FA · Cards: white, subtle shadow, rounded-corner shape underlay
- Accent: purple #7C3AED · Secondary blue #0078D4
- Font: Segoe UI (titles bold 14–16 pt, KPI values bold 20–24 pt, body 10–11 pt)
- Country colors: Germany #7C3AED · France #0078D4 · Italy #059669 · Poland #DC2626 ·
  United Kingdom #D97706 · United States #0891B2 · China #DB2777 · Japan #65A30D ·
  European Union #64748B · World #A8A29E
- Sector colors: Agriculture #84CC16 · Industry #7C3AED · Services #0078D4

## Data model (Get Data → the CSVs in `powerbi/data/`)
Build on the **full dataset** (`FactIndicator_Full.csv`, `FactCountryYear_Full.csv`,
`DimCountry_Full.csv` + shared `DimYear.csv`, `DimIndicator.csv`): 261 countries in
the dimension, 5,339 long fact rows, 183-country 2023 cross-section. The focus files
(`FactCountryYear.csv`, `FactIndicator.csv`, `DimCountry.csv`) are the paper's
10-country panel extract — same model, numbers agree. Default the country slicer
to Germany.
Relationships (single direction, from dims to facts):
- DimCountry[CountryKey] → FactCountryYear[CountryKey], FactIndicator[CountryKey], FactCountryYear_Full[CountryKey]
- DimYear[YearKey] → FactCountryYear[YearKey], FactIndicator[YearKey], FactCountryYear_Full[YearKey]
- DimIndicator[IndicatorKey] → FactIndicator[IndicatorKey], FactIndicator_Full[IndicatorKey]
Hide all key columns. Paste all measures from `DAX_measures.md` into a `_Measures` table.

## Page 1 — Overview (the page from the video-style template)
Slicer header (top band, synced to all pages):
- Country: chiclet slicer (DimCountry[Country]), single select, Germany default
- Year: slider slicer (DimYear[Year]) + a card showing [Selected Year Label]
- Metric switch: three buttons (Manufacturing share / Manufacturing output /
  GDP per worker) wired to bookmarks that swap the main trend visual's measure
  (field parameter is the modern alternative — either is fine)

KPI card row (6 cards, each with its vs-1991 delta underneath):
1. Manufacturing Share % · 2. Manufacturing Output ($bn) · 3. Industrial
Employment (m) · 4. GDP per Worker (PPP $) · 5. High-Tech Export Share % ·
6. Industry Share %

Visuals:
- Main trend: line chart, axis DimYear[Year], values = bookmark-switched measure,
  legend DimCountry[Country] (all 10 panel countries shown, toggled by buttons)
- Right: clustered bar — Manufacturing Share % by DimCountry[Country] for the
  selected year, country colors, drill-through enabled to Page 3
- Gauge (small, under the KPI row): Manufacturing Share % vs the 1991 benchmark
- Bottom left: 100%-stacked column — employment shares (agriculture / industry /
  services) per year for the selected country
- Bottom right: dual-axis line — Manufacturing Output ($bn) vs Industrial
  Employment (m) for the selected country

## Page 2 — Structure & Jobs Deep-Dive
- Stacked column: value added by segment (manufacturing / other industry /
  non-industry, constant 2015 US$bn) per year — build from FactIndicator
- Waterfall: Δ manufacturing output by sub-period (1991–2000, 2000–08, 2008–09,
  2009–18, 2018–22, 2022–24) for the selected country
- Donut: GDP composition (manufacturing / other industry / non-industry),
  selected year
- Line: productivity pair — GDP per Worker (PPP $) and Manufacturing VA per
  Industrial Worker ($) for the selected country
- Horizontal bar: High-Tech Export Share % by country, selected year, sorted desc

## Page 3 — Country Comparison (drill-through target)
- Bar: manufacturing-share change (pp, first year → selected year) by country
- Ribbon: manufacturing-share rank by country across years
- Scatter: manufacturing share (y) vs GDP per capita PPP (x) from
  FactCountryYear_Full (2023, 183 countries), Germany highlighted
- Filled map: Manufacturing Share 2023 % by country (FactCountryYear_Full)
- Treemap: Manufacturing Output ($bn) by country, selected year
- Drill-through filter: DimCountry[Country]

## Page 4 — About & Method
- Text box: research question, headline findings (manufacturing share
  24.6% → 18.0%; real output +33.7% with a 2022 peak of $754.6bn; industrial
  jobs 14.5m → 11.1m; productivity growth halved; income does not predict
  manufacturing share, R² = 0.008)
- Method note: industrial employment and VA per worker are derived
  (labour force × (1 − unemployment) × industry employment share); industry
  employment proxies for manufacturing employment in the per-worker figure
- Data source: World Bank World Development Indicators (accessed Nov 2026)
- Links (buttons with web URL actions): GitHub project, portfolio site
- Note: companion research paper by Rashed Rana

## Navigation
Left vertical button bar on every page (home icon style): Overview · Structure ·
Countries · About — page navigator or buttons with "Page navigation" actions,
selected page highlighted in the accent color.

## Publish checklist
- [ ] All slicers synced across pages 1–3
- [ ] Bookmarks tested (metric switch)
- [ ] Drill-through from Page 1 comparison bar to Page 3 works
- [ ] Mobile layout: KPI cards 2 × 3, trend chart full width
- [ ] File saved as `Germany_Industrial_Transition.pbix` next to this spec
