# Project Structure
## Powering the Grid: A Statistical Analysis of Bulgarian Day-Ahead Electricity Prices (2015–2025)

---

## Research Questions

1. *How strongly do weather variables correlate with real (inflation-adjusted) Bulgarian day-ahead electricity prices?*
2. *Which weather variable has the strongest independent relationship with price?*
3. *Did the 2021-2022 energy crisis fundamentally alter the weather-price relationship, and if so, how?*

**Important:** This analysis identifies statistical associations only. No causal claims are made.

---

## Data Sources

- **Source 1: Ember** — daily day-ahead electricity prices for Bulgaria (nominal €/MWh), 2015–2025
  - License: CC-BY-4.0
  - URL: ember-energy.org/data/european-wholesale-electricity-price-data

- **Source 2: Open-Meteo** — hourly weather for Sofia, aggregated to daily, 2015–2025
  - Variables: temperature, precipitation, wind speed, cloud cover, snowfall
  - URL: open-meteo.com/en/docs/historical-weather-api

- **Source 3: Eurostat** — HICP inflation index for Bulgaria, monthly, base year 2015=100
  - Used to deflate nominal prices to real prices
  - URL: ec.europa.eu/eurostat/databrowser/view/PRC_HICP_MIDX

---

## Market Periods

- **2015–2019** — stable pre-crisis baseline
- **2020** — COVID-suppressed demand (abnormally low prices)
- **2021–2022** — energy crisis (gas price spike, Russia-Ukraine war, extreme price volatility)
- **2023–2025** — post-crisis normalization

---

## Notebook Structure

### 1. Introduction
- Real-world significance of electricity pricing
- The Bulgarian energy market context
- Literature review — 3 academic citations
- Research questions — stated explicitly and numbered
- What this analysis can and cannot claim (correlation vs causation)

### 2. Data Sources
- Source 1: Ember — methodology, license, citation
- Source 2: Open-Meteo — variables, location, methodology
- Source 3: Eurostat HICP — methodology, why deflation is necessary
- Why these sources are independent and appropriate

### 3. Data Loading & Preprocessing
- Load all three CSVs
- Inspect shape, dtypes, missing values for each
- Handle missing values — document decisions
- **Price deflation:**
    - Load Eurostat HICP monthly index (base 2015=100)
    - Map monthly index to daily prices
    - Compute real price: Real Price = Nominal Price × (HICP_base / HICP_t)
    - Document the deflation formula explicitly
    - Compare nominal vs real price series — show what inflation was hiding
- Aggregate Open-Meteo hourly weather to daily
- Align date ranges across all three sources (2015–2025)
- Merge on date
- Feature engineering — extract season, month, year, market period label

### 4. Exploratory Data Analysis
- Nominal vs real price comparison — time series overlay
- Real price distribution — histogram, boxplot, summary statistics
- Time series plot of real price 2015–2025 (annotate market periods)
- Each weather variable — distribution and time series
- Real price by season — boxplots
- Real price by market period — boxplots
- Correlation matrix heatmap — preliminary overview

### 5. Distribution Analysis
- Formal normality testing of real price — explain the math
- Skewness and kurtosis
- Justify choice of Spearman over Pearson based on results

### 6. Correlation Analysis
- Spearman correlation — each weather variable vs real price
- Confidence intervals on each correlation coefficient
- Significance testing — H₀: ρ = 0 for each variable
- Ranked comparison of all variables by correlation strength
- Report both p-value and effect size — discuss the distinction

### 7. Partial Correlation Analysis
- Explain the method and why it is needed
- Compute partial correlations controlling for intercorrelations
- Compare to simple correlations — what changed and why

### 8. Stratified Correlation Analysis

#### 8.1 By Season
- Re-run correlations separately per season
- Compare strength across seasons
- Discuss which relationships are season-dependent

#### 8.2 By Market Period
- Define four periods explicitly:
    - 2015–2019 — stable baseline
    - 2020 — COVID shock
    - 2021–2022 — energy crisis
    - 2023–2025 — post-crisis normalization
- Re-run correlations separately per period
- Compare weather-price relationship strength across periods
- Discuss: did the crisis decouple weather from price?

### 9. Extreme Weather Analysis
- Define extreme days statistically — explain threshold choice
- Mann-Whitney U test — extreme vs normal days
- Bonferroni correction for multiple comparisons
- Interpret results in context

### 10. Discussion
- Answer each research question directly
- What do findings reveal about the Bulgarian market specifically
- Limitations — autocorrelation, observational data, single location weather, nominal vs real price assumptions
- What future analysis could add

### 11. Conclusion
- Key findings summarized
- Methodological contributions
- Broader implications

### References
- All data sources with URLs and licenses
- All academic papers cited

---

## Methods Summary

| Method | Purpose | Section |
|---|---|---|
| Price deflation (HICP) | Remove inflation from nominal prices | 3 |
| Normality testing | Justify correlation method choice | 5 |
| Spearman correlation + significance test | Quantify weather-price relationships | 6 |
| Confidence intervals | Bound the correlation estimates | 6 |
| Partial correlation | Isolate independent contributions | 7 |
| Stratified correlation by season | Season-dependence of relationships | 8.1 |
| Stratified correlation by market period | Crisis impact on relationships | 8.2 |
| Mann-Whitney U test | Extreme weather price impact | 9 |
| Bonferroni correction | Control for multiple comparisons | 9 |

---

## Key Methodological Notes

- Prices are deflated to real terms using Eurostat HICP before any analysis.
- Autocorrelation is a known limitation — prices are not independent day-to-day. State this explicitly.
- Correlation does not imply causation — state this explicitly in the introduction.
- The 2021-2022 crisis period is treated as a natural experiment, not excluded.
- Effect size (r) and statistical significance (p-value) are always reported together.
- Inflation adjustment formula: Real Price = Nominal Price × (HICP_base / HICP_t)
