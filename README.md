# Still the Workshop of Europe? Germany's Industrial Transition, 1991–2024

A data-analysis project (Python + SQL) on how Germany's industry changed
over 34 years: the manufacturing share of GDP fell by a third, real
manufacturing output still grew by a third — and then, after 2018,
output started falling. Germany is compared with France, Italy, Poland,
the United Kingdom, Japan and the EU-27, and with 183 countries in a
cross-country test of whether income explains industrial weight at all.

**Data (100% real, published sources):** World Bank World Development
Indicators (WDI), fetched through the public World Bank API — see
`src/download_data.py`. Every number below is produced by
`src/analysis.py` / `sql/queries.sql` and stored in
`outputs/results.json` / `outputs/sql_results.md`; nothing is typed by
hand. "Manufacturing value added per industrial worker" is an
approximation: it divides manufacturing value added by industry
employment, because the World Bank does not publish manufacturing-only
employment.

## Key findings

1. **A shrinking share, a growing output — until 2018.** Manufacturing
   fell from **24.6% of GDP in 1991 to 18.0% in 2024** (−6.6 points,
   trend −0.08 points/year), yet real manufacturing value added rose
   **33.7%**, from **$531.7 bn to $710.8 bn** (constant 2015 US$).
   The share fell because services grew faster, not because
   manufacturing shrank.
2. **The benign phase ended in 2018.** Manufacturing value added
   peaked at **$754.6 bn in 2022** and has fallen **5.2% since 2018**
   (5.8% from its peak), with declines in four of the six years since
   2018. The largest single-year falls were **2009 (−19.9%)**, **2020
   (−8.3%)**, **1993 (−8.1%)** and **2024 (−4.3%)** — the 2024 fall
   matches the Destatis production index (−4.5% for the year).
3. **Employment adjusted far more than output.** Industry's share of
   employment fell from **38.4% to 26.3%** (−12.1 points) and
   industrial employment from **14.5 million to 11.1 million
   (−23.4%)**, while services rose from 57.9% to 72.6% of employment.
4. **Productivity rose — but its growth halved.** GDP per person
   employed rose **32.2%** ($93,525 → $123,669, PPP) and manufacturing
   value added per industrial worker rose **74.6%** ($36,589 →
   $63,900). But labour-productivity growth fell from **1.10% per
   year (1991–2010) to 0.51% per year (2010–2024)**.
5. **Still the most industrial large EU economy.** Germany's 18.0%
   manufacturing share in 2024 is 3.7 points above the EU-27 average
   (14.3%), and far above France (9.6%) and the United Kingdom (8.0%);
   only Japan (18.8%) is higher among the comparators. The gap to the
   EU has narrowed from 4.9 points in 1991.
6. **Income does not explain manufacturing.** Across **183 countries
   in 2023**, GDP per capita does not predict the manufacturing share
   (OLS slope 0.55, **R² = 0.008, p = 0.23**). Germany sits **6.7
   points above** its income-predicted share (18.9% actual vs 12.2%
   predicted) — its industrial weight is a matter of structure and
   policy, not income.
7. **The technology mix lags.** High-technology exports were **18.0%**
   of German manufactured exports in 2024 (up from 15.5% in 2007),
   below the United Kingdom (29.2%), China (26.3%), the United States
   (24.3%), France (23.1%) and the EU-27 average (20.6%).

## Repository

```
germany-industrial-transition/
├── README.md
├── requirements.txt
├── .gitignore
├── data/                  # WDI series (CSV) + analysis subset
│   ├── wb_*.csv           # raw downloads from the World Bank API
│   ├── analysis_subset.csv
│   └── sources.md         # per-series sources and notes
├── src/
│   ├── download_data.py   # fetch WDI via the public World Bank API
│   ├── analysis.py        # all headline numbers -> outputs/results.json
│   ├── build_db.py        # build industry.db, run SQL -> outputs/sql_results.md
│   └── figures.py         # 8 figures (300 dpi) -> outputs/figures/
├── sql/
│   └── queries.sql        # 9 documented queries
├── outputs/
│   ├── results.json
│   ├── germany_yearly.csv
│   ├── cross_country_2023.csv
│   ├── sql_results.md
│   └── figures/           # fig1..fig8 (PNG, 300 dpi)
└── paper/
    ├── paper_twocol.md    # journal paper, two-column version
    ├── paper.md           # journal paper, single-column version
    ├── paper_twocol.docx
    ├── paper.docx
    └── build_journal.py   # builds .docx + .pdf from the .md files
```

(The paper PDFs are kept out of this repository by the author's choice;
they are available on request.)

## Reproduce

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python src/download_data.py   # fetch WDI data into data/
python src/analysis.py        # -> outputs/results.json
python src/build_db.py        # -> industry.db, outputs/sql_results.md
python src/figures.py         # -> outputs/figures/ (8 PNGs, 300 dpi)
python paper/build_journal.py paper/paper_twocol.md --two-col
python paper/build_journal.py paper/paper.md
```

## Paper

"Still the Workshop of Europe? Germany's Industrial Transition,
1991–2024 — Manufacturing Value Added, Employment and Productivity"
(Rashed Rana, 2026). Two-column (7 pp.) and single-column (10 pp.)
versions in `paper/`. The paper includes an AI-assistance declaration;
the author reviews and decides on any journal submission.
