# Data Dictionary — Bank Marketing Campaign Dataset

> **Version:** 1.0 | **Prepared by:** Section C, Group 11 | **Last Updated:** April 2026  
> **Status:** ✅ Production-Ready (Post-Cleaning)

---

## Dataset Summary

| Item                        | Details                                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------------------- |
| **Dataset Name**            | Bank Marketing Campaign Dataset                                                                |
| **Source**                  | UCI Machine Learning Repository — Portuguese Banking Institution                               |
| **Raw File Name**           | `raw_dataset.csv`                                                                              |
| **Cleaned File Name**       | `cleaned_bank_data.csv`                                                                        |
| **Separator**               | Semicolon (`;`)                                                                                |
| **Last Updated**            | April 2026                                                                                     |
| **Granularity**             | One row per customer contact (a single phone call in a marketing campaign)                     |
| **Raw Shape**               | 41,188 rows × 21 columns                                                                       |
| **Clean Shape**             | 41,176 rows × 27 columns (after removing 12 duplicates; 6 derived columns added)               |
| **Target Variable**         | `subscription_flag` (binary: 1 = subscribed, 0 = did not subscribe)                            |
| **Overall Conversion Rate** | ~11% (highly imbalanced target)                                                                |
| **Pipeline Notebooks**      | `01_extraction.ipynb` → `02_cleaning.ipynb` → `03_eda.ipynb` → `04_statistical_analysis.ipynb` |

---

## Column Definitions

### 🧑 Customer Profile Columns

| Column Name         | Data Type | Description                                           | Example Value         | Used In                            | Cleaning Notes                                                                                                                                     |
| ------------------- | --------- | ----------------------------------------------------- | --------------------- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `age`               | int       | Customer's age in years at the time of contact        | `42`                  | EDA, Statistical Analysis, Tableau | No nulls. No type changes. Used as-is for binning into `age_group`.                                                                                |
| `job_type`          | category  | Customer's occupation / employment category           | `"admin."`            | EDA, KPI, Tableau                  | Renamed from `job`. Cast to `category` dtype. `"unknown"` standardised to `"Unknown"`. 11 distinct categories.                                     |
| `marital_status`    | category  | Customer's marital status                             | `"married"`           | EDA, Tableau                       | Renamed from `marital`. Cast to `category` dtype. Values: `married`, `single`, `divorced`, `Unknown`.                                              |
| `education_level`   | category  | Highest education level attained by the customer      | `"university.degree"` | EDA, KPI, Tableau                  | Renamed from `education`. `"illiterate"` merged into `"basic.4y"` (too few records to stand alone). `"unknown"` → `"Unknown"`. Cast to `category`. |
| `credit_default`    | object    | Whether the customer has credit in default            | `"no"`                | EDA                                | Renamed from `default`. Values: `yes`, `no`, `Unknown`. No imputation applied.                                                                     |
| `has_housing_loan`  | category  | Whether the customer holds a housing loan             | `"yes"`               | EDA, Tableau                       | Renamed from `housing`. Cast to `category`. Values: `yes`, `no`, `Unknown`.                                                                        |
| `has_personal_loan` | category  | Whether the customer holds a personal (consumer) loan | `"no"`                | EDA, Tableau                       | Renamed from `loan`. Cast to `category`. Values: `yes`, `no`, `Unknown`.                                                                           |

### 📞 Campaign Contact Columns

| Column Name             | Data Type | Description                                                             | Example Value | Used In                                 | Cleaning Notes                                                                                                                                                                                                             |
| ----------------------- | --------- | ----------------------------------------------------------------------- | ------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `contact_type`          | category  | Communication channel used to reach the customer                        | `"cellular"`  | EDA, KPI, Statistical Analysis, Tableau | Renamed from `contact`. Cast to `category`. Values: `cellular`, `telephone`. Chi-Square confirmed significant impact (p < 0.05).                                                                                           |
| `contact_month`         | category  | Month of the year when the last contact occurred                        | `"may"`       | EDA, KPI, Tableau                       | Renamed from `month`. Cast to `category`. 12 possible values (abbreviated month names).                                                                                                                                    |
| `contact_day`           | category  | Day of the week when the last contact occurred                          | `"mon"`       | EDA, Tableau                            | Renamed from `day_of_week`. Cast to `category`. Values: `mon`, `tue`, `wed`, `thu`, `fri`.                                                                                                                                 |
| `call_duration_sec`     | int       | Duration of the last phone call in seconds                              | `261`         | EDA, Statistical Analysis, Tableau      | Renamed from `duration`. No capping applied — long calls are genuine and highly predictive. **Note: This field is only known after the call ends; it should not be used as a pre-call predictor in production ML models.** |
| `num_contacts_campaign` | int       | Number of times this customer was contacted during the current campaign | `2`           | EDA, KPI, Statistical Analysis, Tableau | Renamed from `campaign`. **Capped at 10** using `.clip(upper=10)` to suppress extreme outliers. ANOVA confirmed significant impact (p < 0.05).                                                                             |

### 🔁 Previous Campaign History Columns

| Column Name                 | Data Type | Description                                                            | Example Value   | Used In                                 | Cleaning Notes                                                                                       |
| --------------------------- | --------- | ---------------------------------------------------------------------- | --------------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `num_previous_contacts`     | int       | Number of contacts made with this customer before the current campaign | `0`             | EDA, Statistical Analysis, Tableau      | Renamed from `previous`. No cleaning needed. Value of `0` means customer was never contacted before. |
| `previous_campaign_outcome` | category  | Outcome of the previous marketing campaign for this customer           | `"nonexistent"` | EDA, KPI, Statistical Analysis, Tableau | Renamed from `poutcome`. Cast to `category`. Values: `success`, `failure`, `nonexistent`, `Unknown`. |

### 🌍 Macroeconomic Context Columns

| Column Name                 | Data Type | Description                                                       | Example Value | Used In                                 | Cleaning Notes                                                                                                              |
| --------------------------- | --------- | ----------------------------------------------------------------- | ------------- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `employment_variation_rate` | float     | Quarterly employment variation rate (national economic indicator) | `1.1`         | EDA, Statistical Analysis, Tableau      | Renamed from `emp.var.rate`. No cleaning. Highly correlated with other macro variables.                                     |
| `consumer_price_index`      | float     | Monthly consumer price index (inflation indicator)                | `93.994`      | EDA, Statistical Analysis, Tableau      | Renamed from `cons.price.idx`. No cleaning. Part of the macro-economic cluster.                                             |
| `consumer_confidence_index` | float     | Monthly consumer confidence index (sentiment indicator)           | `-36.4`       | EDA, Statistical Analysis, Tableau      | Renamed from `cons.conf.idx`. No cleaning. Negative values are valid and expected.                                          |
| `interest_rate`             | float     | Daily 3-month EURIBOR interest rate (benchmark lending rate)      | `4.857`       | EDA, KPI, Statistical Analysis, Tableau | Renamed from `euribor3m`. No cleaning. Used to derive `rate_environment`. ANOVA confirmed significant impact (p < 0.05).    |
| `number_of_employees`       | int       | Number of bank employees (quarterly, national indicator)          | `5191`        | EDA, Tableau                            | Renamed from `nr.employed`. **Cast from float to int** for consistency. Highly correlated with `employment_variation_rate`. |

### 🎯 Target Column

| Column Name         | Data Type | Description                                                                                            | Example Value | Used In                            | Cleaning Notes                                                                                                                         |
| ------------------- | --------- | ------------------------------------------------------------------------------------------------------ | ------------- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `subscription_flag` | int       | **Primary target variable.** Whether the customer subscribed to a term deposit following this campaign | `1`           | KPI, Statistical Analysis, Tableau | Renamed from `subscribed`/`y`. Mapped from `yes/no` string → `1/0` binary integer. Original `subscribed` column dropped after mapping. |

---

## Derived Columns

All derived columns were created during `02_cleaning.ipynb` to support analysis, segmentation, and Tableau visualisation.

| Derived Column      | Source Column(s)                      | Logic / Transformation                                                                                                                                                 | Business Meaning                                                                                                                                                                                                                   |
| ------------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `age_group`         | `age`                                 | `pd.cut(age, bins=[0,25,35,50,65,100], labels=["1","2","3","4","5"])` → 1=Young (≤25), 2=Early Career (26–35), 3=Mid Career (36–50), 4=Senior (51–65), 5=Retired (65+) | Enables age-based segmentation in dashboards. Numeric labels (1–5) ensure correct sort order in Tableau. ANOVA confirmed age group significantly affects subscription (p < 0.05).                                                  |
| `call_intensity`    | `num_contacts_campaign`               | `1` if calls=1, `2` if calls=2, `4` if 3≤calls≤5, `6` if calls≥6                                                                                                       | Buckets raw call counts into interpretable contact pressure levels. Supports "optimal calling strategy" KPI analysis. More calls → lower conversion.                                                                               |
| `contacted_before`  | `num_previous_contacts`               | `"Yes"` if `num_previous_contacts > 0`, else `"No"`                                                                                                                    | Binary flag indicating whether the customer had any prior campaign interaction. Customers with prior contact have meaningfully higher conversion rates.                                                                            |
| `contact_recency`   | `days_since_last_contact` _(dropped)_ | `"Never Contacted"` if value=999, `"Recently Contacted"` if ≤7 days, `"Moderately Contacted"` if ≤30 days, `"Long Ago Contacted"` otherwise                            | Transforms the raw `pdays` column (which used 999 as a sentinel for "never contacted") into a human-readable recency category. The original numeric column was dropped after transformation.                                       |
| `rate_environment`  | `interest_rate`                       | `pd.cut(interest_rate, bins=[0,1.5,3.5,6], labels=["Low","Medium","High"])`                                                                                            | Classifies the macro interest rate regime at the time of contact. Low-rate environments correlate with significantly higher subscription rates (ANOVA, p < 0.05).                                                                  |
| `prev_success_flag` | `previous_campaign_outcome`           | `1` if `previous_campaign_outcome == "success"`, else `0`                                                                                                              | Binary flag for whether the customer subscribed in a prior campaign. **Strongest single predictor of current subscription** — customers with `prev_success_flag=1` are ~6x more likely to subscribe again (Chi-Square, p < 0.001). |
| `education_ordinal` | `education_level`                     | Ordinal map: `basic.4y`=1, `basic.6y`=2, `basic.9y`=3, `high.school`=4, `professional.course`=5, `university.degree`=6, `Unknown`=0                                    | Numeric encoding of education hierarchy. Used exclusively for correct sort ordering in Tableau — prevents unwanted alphabetical sorting of education categories.                                                                   |

---

## Data Quality Notes

### Missing Values

- **No true nulls exist** in either the raw or cleaned dataset (`df.isnull().sum()` returned all zeros).
- However, several categorical columns contain `"unknown"` (raw) / `"Unknown"` (cleaned) as a **placeholder for missing/unclassified information**. Affected columns: `job_type`, `marital_status`, `education_level`, `credit_default`, `has_housing_loan`, `has_personal_loan`.
- These `"Unknown"` values were **retained and not imputed** to avoid introducing bias. Analysts should be aware that `"Unknown"` is functionally a missing-value category, not a true category.

### Duplicate Records

- **12 exact duplicate rows** were identified in the raw dataset and removed during cleaning (`02_cleaning.ipynb`).
- Post-deduplication row count: **41,176 rows**.

### Outliers

- **`num_contacts_campaign`**: Extreme values (customers contacted 50+ times) existed in the raw data. These were **capped at 10** using `.clip(upper=10)`. This prevents outlier distortion in aggregations and charts without data loss.
- **`call_duration_sec`**: Statistically, many calls are "outliers" (very long calls). However, these are **genuine, high-value interactions** — they are strongly predictive of subscription and were intentionally retained.
- **`days_since_last_contact` (dropped)**: Raw values of `999` were used as a sentinel meaning "customer was never previously contacted." This column was fully replaced by `contact_recency` and then dropped to prevent misinterpretation.

### Target Class Imbalance

- The dataset is **heavily imbalanced**: approximately **88.7% of records are non-subscribers (0)** and only ~11.3% are subscribers (1).
- Any predictive modelling should address this imbalance (e.g., via SMOTE, class weighting, or threshold tuning). Dashboard KPIs should use **conversion rate** (%), not raw counts, to avoid misleading visuals.

### Macroeconomic Multicollinearity

- The five macro columns (`employment_variation_rate`, `consumer_price_index`, `consumer_confidence_index`, `interest_rate`, `number_of_employees`) are **highly intercorrelated** with each other (confirmed in the EDA correlation heatmap).
- For statistical modelling, include only one or use PCA. For Tableau dashboards, `rate_environment` (derived) is the recommended single macro KPI dimension.

### Leakage Warning

- **`call_duration_sec` is a post-call variable.** Its value is only known after the call has ended, making it unsuitable as a predictor in a real-time or pre-call prediction scenario. It is valid for retrospective analysis and dashboard KPIs, but must be **excluded from any prospective ML model** to avoid data leakage.

### Education Consolidation

- The `"illiterate"` education category had an extremely small sample size and was **merged into `"basic.4y"`** during cleaning to avoid sparse-category noise in analysis.

### Sentinel Value Removed

- The raw `pdays` column (renamed `days_since_last_contact`) used `999` as a sentinel for "never contacted." This non-standard encoding was resolved by converting to the categorical `contact_recency` column. The raw numeric column was then **dropped** from the dataset entirely.

---

## Statistical Testing Summary

The following tests were conducted in `04_statistical_analysis.ipynb` at significance level **α = 0.05**:

| Test # | Feature Tested      | Dashboard              | Test Type     | Result                   | Business Conclusion                                                                                    |
| ------ | ------------------- | ---------------------- | ------------- | ------------------------ | ------------------------------------------------------------------------------------------------------ |
| 1      | `age_group`         | Customer Profile       | One-Way ANOVA | ✅ Reject H₀ (p < 0.05)  | Age group significantly affects subscription. Young (≤25) and Senior (55+) convert at highest rates.   |
| 2      | `job_type`          | Customer Profile       | Chi-Square    | ✅ Reject H₀ (p < 0.05)  | Job type significantly influences conversion. Students and retired individuals lead; blue-collar lags. |
| 3      | `call_intensity`    | Campaign Effectiveness | One-Way ANOVA | ✅ Reject H₀ (p < 0.05)  | Number of calls significantly impacts success. Single-call contacts convert best.                      |
| 4      | `contact_type`      | Campaign Effectiveness | Chi-Square    | ✅ Reject H₀ (p < 0.05)  | Cellular outperforms telephone contact significantly.                                                  |
| 5      | `prev_success_flag` | Interaction History    | Chi-Square    | ✅ Reject H₀ (p ≪ 0.001) | **Strongest predictor.** Prior success customers are ~6× more likely to re-subscribe.                  |
| 6      | `rate_environment`  | Economic Impact        | One-Way ANOVA | ✅ Reject H₀ (p < 0.05)  | Low-rate environments yield significantly higher subscription rates.                                   |

---

_This data dictionary was generated from the complete pipeline notebooks: `01_extraction.ipynb`, `02_cleaning.ipynb`, `03_eda.ipynb`, and `04_statistical_analysis.ipynb`._
