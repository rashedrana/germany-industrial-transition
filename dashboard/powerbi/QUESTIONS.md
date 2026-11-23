# Business Questions & Chart Problem-Solving
## Germany Industrial Transition Dashboard — 19 questions, 19 visual types

Dataset: World Bank World Development Indicators (full extract in `powerbi/data/`):
**261 countries in the dimension · 10-country panel 1991–2024 · 183-country
cross-section for 2023 · 10,732 fact rows** across the four fact tables
(country × year wide table, country × year × indicator long table, and their
full-dataset counterparts). Every answer below was computed from the project
data and cross-checked against the research paper — the dashboard and the
paper agree number for number (e.g. Germany's manufacturing share 24.6% in
1991 → 18.0% in 2024 in both).

---

**Q1. Where does German industry stand in the selected year — at a glance?**
Visual: **KPI cards** (Manufacturing Share %, Manufacturing Output $bn,
Industrial Employment, GDP per Worker, High-Tech Export Share %, Industry
Share %) with vs-start-year deltas.
Answer (2024): manufacturing 18.0% of GDP (−6.6 pp vs 1991) · real output
$710.8bn (+33.7% vs 1991) · 11.12m industrial jobs (−23.4%) · GDP per worker
$123,669 (+32.2%) · high-tech exports 18.0% of manufactured exports
(+2.5 pp vs 2007) · industry overall 25.6% of GDP (−7.7 pp).

**Q2. How much of the 1991 industrial weight is left?**
Visual: **Gauge** — Manufacturing Share % against the 1991 benchmark (24.6%).
Answer: 18.0% in 2024 — Germany keeps about **73% of its 1991 manufacturing
share**. The share troughed at 17.3% in the 2009 crisis year and has moved
sideways since, even as output kept growing.

**Q3. How did the transition unfold — share down, output up?**
Visual: **Line chart** with bookmark metric switch (manufacturing share /
manufacturing output / GDP per worker), 10 panel countries + slicer.
Answer: The two lines tell opposite stories — share falls 24.6% → 18.0%
(1991→2024) while real manufacturing output rises $531.7bn → $710.8bn
(+33.7%), peaking at **$754.6bn in 2022** and slipping −5.8% since.

**Q4. How did the jobs mix change?**
Visual: **100% stacked column chart** (employment by sector, Germany).
Answer: Industry employed 38.4% of all workers in 1991, services 57.9% and
agriculture 3.6%. By 2024: industry 26.3%, services 72.6%, agriculture 1.1% —
roughly one job in four is now industrial, three in four are services.

**Q5. Which part of the economy actually shrank?**
Visual: **Stacked column chart** (value added in constant 2015 US$bn:
manufacturing / other industry / non-industry, Germany).
Answer: Manufacturing grew in absolute terms ($531.7bn → $710.8bn). The
shrinkage is in *other* industry (construction, mining, utilities):
$266.9bn → $144.2bn. Non-industry value added grew $1,686bn → $2,822bn.
Germany's GDP rose from $2,485bn to $3,677bn (constant 2015 US$).

**Q6. Which periods built — and destroyed — manufacturing output?**
Visual: **Waterfall chart** (Δ manufacturing value added by sub-period,
Germany, constant 2015 US$bn).
Answer: 1991–2000 +7.6 · 2000–2008 +92.1 · **2008–2009 −125.8** (the financial
crisis wiped out two decades of growth in one year, −19.9%) · 2009–2018
+244.0 (the long recovery) · 2018–2022 +5.0 · **2022–2024 −43.8** (the current
slide). Net 1991→2024: +179.1.

**Q7. Where does Germany rank by manufacturing share in 2024?**
Visual: **Clustered bar chart** (manufacturing share, panel countries) using
the `Country Rank by Manufacturing Share` DAX measure.
Answer: **#3 of the 9 panel countries with 2024 data** — China 24.8%,
Japan 18.8%, **Germany 18.0%**, Poland 16.1%, Italy 15.1%, World 14.9%,
EU 14.3%, France 9.6%, UK 8.0%. (The US series has no 2024 value in this
WDI vintage.)

**Q8. Who de-industrialised fastest?**
Visual: **Bar chart** of manufacturing-share change in percentage points
(first available year → 2024), sorted ascending.
Answer: The UK fell most (−8.2 pp since 1991), then China (−6.7 pp since
2004) and Germany (−6.6 pp since 1991). **Every** panel country lost share —
Poland least (−3.2 pp since 1995). De-industrialisation is the panel-wide
norm; Germany is mid-pack for speed but started highest in Europe.

**Q9. What does Germany's GDP look like in 2024?**
Visual: **Donut chart** (GDP composition, selected country + year).
Answer (Germany 2024): manufacturing 18.0%, other industry 7.6%,
everything else (services, agriculture, government) 74.4%.

**Q10. Where is the panel's manufacturing output concentrated?**
Visual: **Treemap** (manufacturing value added 2024, constant 2015 US$bn).
Answer: Japan $935.5bn is the panel's largest manufacturing economy in this
dataset, ahead of Germany $710.8bn, the UK $308.0bn, Italy $288.0bn,
France $268.7bn and Poland $113.7bn. (China and the US are excluded —
this WDI vintage carries their constant-price manufacturing VA for 2015
only; the EU aggregate, $2,368.6bn, is excluded to avoid double counting.)

**Q11. Does a rich country have to de-industrialise?**
Visual: **Scatter chart** — manufacturing share (y) vs GDP per capita PPP (x),
183 countries, 2023, Germany highlighted.
Answer: **No.** Across 183 countries, income explains almost none of the
variation in manufacturing share (R² = 0.008, paper's regression). Germany
(18.9% at $63,140 per capita in 2023) sits **+6.7 pp above** the share its
income would predict (12.2%); the median country is at 11.0%.

**Q12. Where in the world is manufacturing biggest?**
Visual: **Filled map** (manufacturing share of GDP by country, 2023).
Answer: Puerto Rico 43.7%, San Marino 31.8%, Ireland 29.8%, Eswatini 29.2%
and Cambodia 26.3% top the map — mostly small, specialised economies.
Germany's 18.9% (2023) is high for a large economy.

**Q13. How did the league table change since 1991?**
Visual: **Ribbon chart** (manufacturing-share rank by country across years).
Answer: In 1991 Germany was **#1 of the panel** (24.6%, ahead of the EU's
19.7% and Italy's 19.3%). By 2024 it is #3, overtaken by China (series
starts 2004 at 31.5%) and Japan — but it remains Europe's manufacturing
heavyweight, well clear of France (9.6%) and the UK (8.0%).

**Q14. What do the decades show — and what drove productivity?**
Visual: **Matrix with conditional formatting** (decade averages, Germany) +
**decomposition tree** (manufacturing VA per industrial worker → output and
employment effects).
Answer: Decade means — manufacturing share 21.0% (1991–99) → 19.7% (2000s)
→ 19.9% (2010s) → 18.5% (2020–24); average industrial jobs 13.24m → 11.54m
→ 11.26m → 11.28m; GDP per worker $101,728 → $114,640 → $119,758 → $123,700.
Manufacturing VA per industrial worker rose **+74.6%** ($36,589 → $63,900):
output +33.7% divided by a workforce −23.4% smaller. But economy-wide
productivity growth halved — GDP per worker grew 1.10%/yr (1991–2010)
vs 0.51%/yr (2010–2024).

---

## Retrofit questions (Q15–Q19, added 2026-11-23 — advanced chart types)

**Q15. How did the manufacturing league table actually move between the
benchmark years?**
Visual: **Bump chart** (manufacturing-share rank at 1991 · 2000 · 2010 ·
2024; ranked among panel countries with data in that year — 1991: 5,
2000: 9, 2010: 10, 2024: 9).
Answer: Germany starts **#1 in 1991** (24.6%), is passed by Japan by 2000
(**#2**), then by China by 2010 (**#3**), and holds **#3 in 2024**
(China 24.8%, Japan 18.8%, Germany 18.0%). The UK falls furthest —
#4 in 1991 to **last (#9) in 2024**; France slides #5 → #8. China enters
the chart at #1 in 2010 (its series starts 2004 at 31.5%) and never
gives the top spot back.

**Q16. Where in the country × year grid are the strong and weak
manufacturing eras?**
Visual: **Heatmap** (manufacturing share %, 10 panel countries × 34 years,
colour-scaled 7–32%).
Answer: The darkest cells cluster in each country's earliest years —
the whole panel has faded since the 1990s. **China is the darkest row**
throughout (peak **32.0% in 2006**; still the panel's highest at 24.8%
in 2024). The 2009 crisis column pales visibly for the European rows —
Germany 17.3%, Italy 13.7%, EU 13.8%, each at or near its row minimum.
The UK row fades most steadily of all, 16.2% → 8.0% with no recovery
column anywhere.

**Q17. Did any panel country actually gain manufacturing share?**
Visual: **Diverging bar chart** (change in manufacturing share,
first available year → latest, percentage points, diverging from zero).
Answer: **No — all ten bars point left.** UK −8.2 pp (16.2% → 8.0%,
1991–2024) lost most, then China −6.7 pp (2004–2024), **Germany −6.6 pp
(24.6% → 18.0%)**, France −6.2 pp, US −5.6 pp (1997–2021), EU −5.4 pp,
Japan −4.6 pp (1994–2024), World −4.3 pp (1997–2024), Italy −4.2 pp and
Poland −3.2 pp (1995–2024) least. Losing share is universal; only the
speed differs.

**Q18. Same transition, or ten different ones?**
Visual: **Small multiples** (one manufacturing-share trend panel per
panel country, 1991–2024, uniform 0–35% scale).
Answer: Ten different shapes. Germany, Italy and the EU fall steeply in
the 1990s then plateau; France slides almost monotonically (15.7% →
9.6%); the UK's line is the steepest continuous descent; China draws a
high arc peaking at 32.0% in 2006; Japan steps down gently from the
panel's highest starting plateau (23.4% in 1994); **Poland is the only
clear U-turn** — dipping to 14.1% in 2002, then recovering to 16.1% by
2024. The US panel is the shortest (1997–2021).

**Q19. What does the 2023 manufacturing map of Europe look like?**
Visual: **Tile map** (one square per country, position ≈ geography,
colour = manufacturing share of GDP, 2023; 43 European countries from
the 183-country cross-section).
Answer: **San Marino 31.8%** and **Ireland 29.8%** top the map, but the
structural story is the Central-European belt: Czechia 20.4%, Slovenia
19.5%, Switzerland 19.4%, **Germany 18.9%**, Poland 17.5%, Hungary 17.2%
and Slovakia 16.9% form a contiguous high-share core — Germany ranks
**#8 of the 43 European countries mapped**. The west and north-west are
pale (France 9.8%, UK 8.2%, Norway 5.6%); the lowest tiles are
Montenegro 2.9%, Andorra 3.4% and Luxembourg 4.1%.

---

Coverage checklist (Power BI visual palette): KPI card ✓ · gauge ✓ · line ✓ ·
100% stacked column ✓ · stacked column ✓ · waterfall ✓ · clustered bar ✓ ·
bar (change) ✓ · donut ✓ · treemap ✓ · scatter ✓ · filled map ✓ · ribbon ✓ ·
matrix + decomposition tree ✓ — plus slicers, bookmarks and drill-through
(layout spec). Retrofit additions: bump chart ✓ · heatmap (matrix with
colour scale) ✓ · diverging bar ✓ · small multiples (small-multiple line
panels) ✓ · tile map ✓.
