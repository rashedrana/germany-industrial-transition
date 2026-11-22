# Star Schema — Germany Industrial Transition (Power BI model)

```
                    DimCountry                      DimYear
                 (CountryKey PK)                 (YearKey PK)
                  Country, ISO_Code,              Year, Decade
                  Region                              |
                        |                              |
          +-------------+--------------+---------------+-----------+
          |                            |                           |
  FactCountryYear              FactIndicator            FactCountryYear_Full
  (CountryKey FK,              (CountryKey FK,           (CountryKey FK,
   YearKey FK,                  YearKey FK,               YearKey FK = 2023,
   ManufShare_Pct,              IndicatorKey FK,          ManufShare_Pct,
   IndustryShare_Pct,           Value)                    GDPperCapita_PPP2021)
   ManufVA_bnUSD,                     |
   IndustryVA_bnUSD,            DimIndicator
   EmpIndustry_Pct,            (IndicatorKey PK)
   EmpServices_Pct,             Indicator, IndicatorCode,
   EmpAgri_Pct,                 Category, Unit
   IndustrialEmployment_m,
   GDPperWorker_PPP,
   GDPperCapita_USD,
   HighTechExports_Pct,
   Unemployment_Pct,
   LabourForce_m, Population_m)
```

## Tables (CSVs in `data/`)
| Table | Grain | Rows | Contents |
|---|---|---|---|
| FactCountryYear | country × year | 340 | Wide table: the 10-country panel (1991–2024) with all headline measures incl. derived industrial employment |
| FactIndicator | country × year × indicator | 4,870 | Long table: every WDI indicator + 3 derived indicators for the panel |
| FactCountryYear_Full | country (2023) | 183 | Cross-section: manufacturing share + GDP per capita PPP for 183 countries, 2023 |
| FactIndicator_Full | country × year × indicator | 5,339 | Long table for all 261 dimension countries: the panel series + the 2023 cross-section indicators |
| DimCountry | country | 10 | Germany, France, Italy, Poland, United Kingdom, United States, China, Japan, European Union, World |
| DimCountry_Full | country | 261 | Every country/aggregate in the WDI extract, with World Bank region |
| DimYear | year | 34 | 1991–2024, with decade band |
| DimIndicator | indicator | 17 | 13 WDI indicators + GDP per capita PPP (2023 cross-section) + 3 derived indicators, with category and unit |

## Design notes
- Two fact shapes share the conformed DimCountry and DimYear dimensions: a wide
  country-year table for the KPI/chart measures, and a long indicator table so
  any WDI series can be sliced by DimIndicator[Category] — the standard way to
  combine headline measures with a flexible indicator bank.
- The 2023 cross-section (183 countries, regional aggregates removed — the
  paper's regression sample) lives in FactCountryYear_Full and also feeds two
  indicators (1 and 17) of FactIndicator_Full for every country in the raw
  2023 download (261 dimension rows).
- Derived indicators (keys 14–16) are computed in data prep exactly as in the
  paper: industrial employment = labour force × (1 − unemployment rate) ×
  industry employment share; manufacturing VA per industrial worker uses
  industry employment as a proxy for manufacturing employment and is labelled
  an approximation.
- Coverage gaps are real WDI gaps, kept as missing rows rather than filled:
  Poland's manufacturing share starts 1995, Japan's 1994, the US's 1997,
  China's 2004; high-technology exports start 2007; constant-price
  manufacturing VA for the US, China and the world exists for 2015 only in
  this vintage.
- Source data: `data/analysis_subset.csv` + `industry.db` of this repository
  (World Bank WDI, accessed November 2026) — the same dataset used in the
  research paper, so the dashboard and the paper always agree number for number.
