# County-Level Socioeconomic, Environmental, Behavioral & Disease Prevalence Data (USA, 2019–2023)

**Creators:** Maksim I. Pirozhkov, Pavel A. Vedeneev  
**Contact:** makspirozhkov2005@gmail.com  
**Data version:** 1.0  
**Last updated:** 2025-06-14

---

## 1. Overview

This dataset provides annual, county‑level measurements for **5 air pollutants**, **3 behavioral risk factors**, **4 socioeconomic indicators**, and **6 chronic disease prevalences** across 3,144 unique U.S. counties for the years 2019–2023.  

Missing pollution values have been imputed using **inverse distance weighting (IDW)**. A small number of missing values remain for `median_household_income` (4 rows) and `CO` (2 rows). The dataset includes **disease outcomes** (CDC PLACES) directly, so it can be used for supervised learning without external merging.

---

## 2. File information

| File | Format | Rows | Columns | Size |
|------|--------|------|---------|------|
| `merged_full_df_cleaned.csv` | CSV (UTF‑8) | 15,404 | 20 | 3.06 MB |

**Rows per year:**
- 2019: 3,112
- 2020: 3,133
- 2021: 3,068
- 2022: 3,135
- 2023: 2,956

**Unique counties (FIPS):** 3,144  

**Missing values:**  
- `median_household_income`: 4 missing  
- `CO`: 2 missing  
All other columns have complete data.

---

## 3. Data sources & processing

### Sources
- **Air pollutants (CO, NO₂, PM₂.₅, PM₁₀, O₃)** – EPA Air Quality System (AQS), annual averages.
- **Behavioral risk factors (smoking, obesity, low physical activity)** – CDC PLACES 2023 (age‑adjusted %).
- **Socioeconomic indicators** – ACS 5‑year estimates (2019–2023).
- **Disease outcomes (cancer, asthma, CHD, COPD, diabetes, stroke)** – CDC PLACES 2023 (age‑adjusted %).

### Preprocessing steps
1. Removed rows with invalid FIPS codes or missing centroid coordinates.
2. Imputed missing pollutant concentrations using **IDW** (k=5, power=2) based on county centroids.
3. Merged all sources by `FIPS` and `year`.
4. Retained all rows where at least the disease outcome and key features were available (some years have fewer counties due to PLACES or ACS coverage).

> **Note:** A small number of missing values persist in `median_household_income` (4 rows) and `CO` (2 rows). These are left as `NaN` – you may choose to drop or impute them further.

---

## 4. Data dictionary

| Variable name | Description | Data type | Unit / range | Missing |
|---------------|-------------|-----------|--------------|---------|
| `FIPS` | 5‑digit county FIPS code | integer | 1001 – 56045 | 0 |
| `year` | Calendar year | integer | 2019 – 2023 | 0 |
| `median_household_income` | Median annual household income (USD) | float | 17,109 – 178,707 | 4 |
| `poverty_rate` | Proportion of population below poverty line | float | 0.00 – 0.59 (0–59%) | 0 |
| `bachelor_plus_rate` | Proportion with bachelor’s degree or higher | float | 0.00 – 0.80 (0–80%) | 0 |
| `unemployment_rate_acs` | Unemployment rate (proportion) | float | 0.00 – 0.32 (0–32%) | 0 |
| `CO` | Annual mean carbon monoxide concentration | float | 0.02 – 1.29 ppm | 2 |
| `NO2` | Annual mean nitrogen dioxide concentration | float | 0.24 – 21.97 ppb | 0 |
| `PM2_5` | Annual mean fine particulate matter | float | 0.92 – 31.29 µg/m³ | 0 |
| `PM10` | Annual mean coarse particulate matter | float | 4.40 – 65.11 µg/m³ | 0 |
| `O3` | Annual mean ozone concentration | float | 0.01 – 0.05 ppm | 0 |
| `prcCancer_non_skin` | Age‑adjusted prevalence of cancer (excluding skin) | float | 2.70 – 17.80 % | 0 |
| `prcAsthma` | Age‑adjusted prevalence of asthma | float | 7.10 – 16.10 % | 0 |
| `prcCoronaryHeartDisease` | Age‑adjusted prevalence of coronary heart disease | float | 3 – 15 % | 0 |
| `prcCOPD` | Age‑adjusted prevalence of COPD | float | 3.20 – 19.80 % | 0 |
| `prcSmoking` | Age‑adjusted prevalence of current smoking | float | 5.20 – 45.70 % | 0 |
| `prcDiabetes` | Age‑adjusted prevalence of diabetes | float | 4.90 – 27.10 % | 0 |
| `prcLPA` | Age‑adjusted prevalence of low physical activity (no leisure‑time PA) | float | 10.20 – 52.70 % | 0 |
| `prcObesity` | Age‑adjusted prevalence of obesity (BMI ≥ 30) | float | 15.20 – 52.90 % | 0 |
| `prcStroke` | Age‑adjusted prevalence of stroke | float | 1.70 – 8.90 % | 0 |

**Notes:**  
- `poverty_rate`, `bachelor_plus_rate`, `unemployment_rate_acs` are proportions (multiply by 100 to obtain percent).  
- Disease prevalence columns (`prc*`) are already percentages.  
- Missing values are represented as `NaN`. Rows with missing `median_household_income` (4 rows) or `CO` (2 rows) can be dropped or imputed.

**Note:** All percentages are age‑adjusted as provided by CDC PLACES.

---

## 5. Known gaps & limitations

- **Missing income & CO values:** 4 counties lack `median_household_income`; 2 counties lack `CO`. You may need to drop or impute these 6 rows.
- **Incomplete temporal coverage:** 2023 has the fewest rows (2,956 vs. ~3,100 in other years). Some FIPS codes are missing from 2023 entirely.
- **IDW imputation uncertainty:** Imputed pollution values have no confidence intervals and may be biased for remote counties.
- **Static graph assumption:** The dataset does not include dynamic spatial connectivity (e.g., changing roads or mobility).
- **No external validation:** Use with caution for counties with sparse monitor coverage.

---

## 6. Usage & license

**License:** Creative Commons Attribution 4.0 International (CC BY 4.0)  

**Recommended citation:**  
> Pirozhkov, M. I., & Vedeneev, P. A. (2025). *County‑Level Socioeconomic, Environmental, Behavioral & Disease Prevalence Data (USA, 2019–2023)* [Data set]. GitHub. https://github.com/M007AX/FMEPID2026

**Related paper:**  
M.I. Pirozhkov, P.A. Vedeneev. “A Graph Attention Transformer Framework for Integrating Socioeconomic and Environmental Data in Disease Prevalence Modeling.” *AIME* (2026).

---

## 7. Example use (Python)

```python
import pandas as pd

df = pd.read_csv("merged_full_df_cleaned.csv")

# Check missing income rows
missing_income = df[df['median_household_income'].isna()]
print(missing_income[['FIPS','year']])

# Train on 2019–2022, test on 2023
train = df[df['year'] != 2023]
test = df[df['year'] == 2023]

# Drop rows with missing CO or income if needed
df_clean = df.dropna(subset=['CO', 'median_household_income'])


## 8. Contact

For questions, please open an issue on GitHub or email Maksim Pirozhkov at makspirozhkov2005@gmail.com.
