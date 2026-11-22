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
