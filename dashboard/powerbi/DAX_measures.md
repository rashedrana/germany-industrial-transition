# DAX Measures — Germany Industrial Transition (Power BI)

Model tables: `FactCountryYear`, `FactIndicator`, `FactCountryYear_Full`,
`DimCountry`, `DimYear`, `DimIndicator`.
Create a dedicated measure table (`_Measures`) in Power BI and paste these in one by one.
All measures respond to the Year and Country slicers unless stated otherwise.
`FactCountryYear` holds one row per country × year, so AVERAGE returns the
selected country-year value under the slicers (same pattern as Project 1).

---

## Core structure measures (FactCountryYear)

```dax
Manufacturing Share % =
AVERAGE ( FactCountryYear[ManufShare_Pct] )
```

```dax
Industry Share % =
AVERAGE ( FactCountryYear[IndustryShare_Pct] )
```

```dax
Other Industry Share (pp) =
[Industry Share %] - [Manufacturing Share %]
```

```dax
Non-Industry Share % =
100 - [Industry Share %]
```

```dax
Manufacturing Output ($bn) =
AVERAGE ( FactCountryYear[ManufVA_bnUSD] )
```

```dax
Industry Output ($bn) =
AVERAGE ( FactCountryYear[IndustryVA_bnUSD] )
```

## Employment & productivity measures

```dax
Industrial Employment (m) =
AVERAGE ( FactCountryYear[IndustrialEmployment_m] )
```

```dax
Industry Employment Share % =
AVERAGE ( FactCountryYear[EmpIndustry_Pct] )
```

```dax
Services Employment Share % =
AVERAGE ( FactCountryYear[EmpServices_Pct] )
```

```dax
GDP per Worker (PPP $) =
AVERAGE ( FactCountryYear[GDPperWorker_PPP] )
```

```dax
Manufacturing VA per Industrial Worker ($) =
DIVIDE ( [Manufacturing Output ($bn)] * 1E9, [Industrial Employment (m)] * 1E6 )
```

```dax
High-Tech Export Share % =
AVERAGE ( FactCountryYear[HighTechExports_Pct] )
```

```dax
Unemployment % =
AVERAGE ( FactCountryYear[Unemployment_Pct] )
```

## KPI / time-intelligence measures (vs start year 1991)

```dax
Manufacturing Share % (1991) =
CALCULATE ( [Manufacturing Share %], DimYear[Year] = 1991 )
```

```dax
Manufacturing Share Change vs 1991 (pp) =
[Manufacturing Share %] - [Manufacturing Share % (1991)]
```

```dax
Manufacturing Share vs 1991 Benchmark % =
DIVIDE ( [Manufacturing Share %], [Manufacturing Share % (1991)] ) * 100
```

```dax
Manufacturing Output Change vs 1991 % =
VAR Base =
    CALCULATE ( [Manufacturing Output ($bn)], DimYear[Year] = 1991 )
RETURN
    DIVIDE ( [Manufacturing Output ($bn)] - Base, Base ) * 100
```

```dax
Industrial Employment Change vs 1991 % =
VAR Base =
    CALCULATE ( [Industrial Employment (m)], DimYear[Year] = 1991 )
RETURN
    DIVIDE ( [Industrial Employment (m)] - Base, Base ) * 100
```

```dax
Manufacturing Output YoY % =
VAR ThisYear = [Manufacturing Output ($bn)]
VAR PrevYear =
    CALCULATE (
        [Manufacturing Output ($bn)],
        DimYear[Year] = MAX ( DimYear[Year] ) - 1
    )
RETURN
    DIVIDE ( ThisYear - PrevYear, PrevYear ) * 100
```

```dax
Peak Manufacturing Output ($bn) =
MAXX ( ALL ( DimYear ), [Manufacturing Output ($bn)] )
```

```dax
Change from Peak % =
DIVIDE (
    [Manufacturing Output ($bn)] - [Peak Manufacturing Output ($bn)],
    [Peak Manufacturing Output ($bn)]
) * 100
```

## Long-table measure (FactIndicator — any indicator via slicer)

```dax
Indicator Value =
AVERAGE ( FactIndicator[Value] )
```

## Cross-section measures (FactCountryYear_Full, 2023, 183 countries)

```dax
Manufacturing Share 2023 % =
AVERAGE ( FactCountryYear_Full[ManufShare_Pct] )
```

```dax
Country Rank by Manufacturing Share =
RANKX (
    ALL ( DimCountry[Country] ),
    [Manufacturing Share %],
    ,
    DESC,
    DENSE
)
```

```dax
Countries Above Germany (2023) =
VAR DEU =
    CALCULATE ( [Manufacturing Share 2023 %], DimCountry[ISO_Code] = "DEU" )
RETURN
    COUNTROWS ( FILTER ( ALL ( DimCountry ), [Manufacturing Share 2023 %] > DEU ) )
```

## Display helpers

```dax
Selected Country =
SELECTEDVALUE ( DimCountry[Country], "All countries" )
```

```dax
Selected Year Label =
"Focus year: " & SELECTEDVALUE ( DimYear[Year], 2024 )
```

## Retrofit measures (Q15–Q19, added 2026-11-23)

For the bump chart (Q15), each snapshot year (1991 / 2000 / 2010 / 2024)
gets its own rank measure so the four rank columns sit side by side;
ranks are computed only over panel countries that have a value that year.

```dax
Manufacturing Share Rank 1991 =
VAR Y = 1991
RETURN
    RANKX (
        FILTER ( ALL ( DimCountry ), NOT ISBLANK ( CALCULATE ( [Manufacturing Share %], DimYear[Year] = Y ) ) ),
        CALCULATE ( [Manufacturing Share %], DimYear[Year] = Y ),
        ,
        DESC,
        DENSE
    )
```
(Duplicate as `... Rank 2000`, `... Rank 2010`, `... Rank 2024` with
Y = 2000 / 2010 / 2024.)

For the diverging bar (Q17) — first and last available values per country,
so series that start late (China 2004, Poland 1995) or end early
(US 2021) are compared over their own span:

```dax
Manufacturing Share % (First Available) =
VAR FirstYear =
    CALCULATE ( MIN ( FactCountryYear[YearKey] ), NOT ISBLANK ( FactCountryYear[ManufShare_Pct] ) )
RETURN
    CALCULATE ( [Manufacturing Share %], DimYear[Year] = FirstYear )
```

```dax
Manufacturing Share % (Last Available) =
VAR LastYear =
    CALCULATE ( MAX ( FactCountryYear[YearKey] ), NOT ISBLANK ( FactCountryYear[ManufShare_Pct] ) )
RETURN
    CALCULATE ( [Manufacturing Share %], DimYear[Year] = LastYear )
```

```dax
Manufacturing Share Change First to Last (pp) =
[Manufacturing Share % (Last Available)] - [Manufacturing Share % (First Available)]
```

For the heatmap (Q16), anchor the matrix colour scale to the panel's
observed range so the conditional formatting is stable across slicers:

```dax
Panel Min Manufacturing Share % =
MINX ( ALL ( FactCountryYear ), FactCountryYear[ManufShare_Pct] )
```

```dax
Panel Max Manufacturing Share % =
MAXX ( ALL ( FactCountryYear ), FactCountryYear[ManufShare_Pct] )
```

For the tile map (Q19), reuse `Manufacturing Share 2023 %` (above) on a
shape/tile visual filtered to European countries
(`DimCountry_Full[Region] = "Europe & Central Asia"`, Central Asian
states removed in the visual filter) — no new measure needed. The small
multiples (Q18) reuse `Manufacturing Share %` with the country field in
the small-multiples well of a line chart, y-axis fixed 0–35.

---

Notes
- KPI cards on Page 1 use: Manufacturing Share %, Manufacturing Output ($bn),
  Industrial Employment (m), GDP per Worker (PPP $), High-Tech Export Share %,
  Industry Share % — each with its "vs 1991" companion measure as the delta
  visual (high-tech exports use 2007 as the base — the series starts then).
- The gauge on Page 1 uses [Manufacturing Share %] with the maximum set to
  [Manufacturing Share % (1991)] so it reads as "share of the 1991 level kept".
- `Manufacturing VA per Industrial Worker` is an approximation: industry
  employment proxies for manufacturing employment (see the paper's methods).
