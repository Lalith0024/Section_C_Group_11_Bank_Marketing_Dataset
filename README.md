# Bank Marketing Campaign Analytics

> Newton School of Technology | Data Visualization & Analytics Capstone  
> Section C, Group 11

This project analyzes a Portuguese bank's direct marketing campaign to understand what drives term deposit subscriptions. The work combines Python-based ETL, exploratory analysis, statistical validation, and a four-page Tableau dashboard suite built for business decision-making.

The core question is:

> Which customer segments, campaign strategies, interaction patterns, and macroeconomic conditions most strongly influence whether a customer subscribes to a term deposit?

---

## Project Highlights

| Metric | Value |
|---|---:|
| Raw records | 41,188 |
| Cleaned records | 41,176 |
| Cleaned columns | 27 |
| Successful subscriptions | 4,639 |
| Overall conversion rate | 11.27% |
| Tableau dashboards | 4 |
| Statistical tests validated | 6 |

The dashboard story shows that conversion is not random. Prior campaign success, contact channel, call pressure, customer profile, and interest-rate environment all materially affect subscription outcomes.

---

## Dashboard Suite

The Tableau workbook is available at [`tableau/Bank Marketing Dataset.twbx`](tableau/Bank%20Marketing%20Dataset.twbx), with dashboard screenshots stored in [`tableau/screenshots/`](tableau/screenshots/).

### 1. Customer Profile

![Customer Profile](tableau/screenshots/Customer%20Profile.png)

This dashboard profiles the customer base by geography, job type, education, marital status, and loan ownership. It provides the top-level business context before moving into campaign performance.

**Key takeaways**

- Total customer contacts after cleaning: **41,176**.
- Overall success rate: **11.27%**, confirming the campaign is a low-conversion, high-volume funnel.
- Admin, blue-collar, and technician customers make up the largest contacted groups.
- Students and retired customers have the strongest conversion rates, while blue-collar customers lag.
- The dashboard includes filters for contact date, job type, education, and marital status.

### 2. Campaign Effectiveness

![Campaign Effectiveness](tableau/screenshots/Campain%20Effectiveness.png)

This dashboard evaluates how the bank's outreach strategy affects conversion. It focuses on call intensity, contact channel, campaign month, previous outcome, and recency.

**Key takeaways**

- One-call campaigns convert at **13.04%**, while high-intensity campaigns fall to **5.49%**.
- Cellular outreach converts at **14.74%**, compared with **5.23%** for telephone.
- March, December, September, and October show the strongest monthly conversion patterns.
- Recent and moderately recent contacts convert far better than customers who were never previously contacted.
- The dashboard supports a campaign strategy shift from volume-heavy calling to targeted, high-quality outreach.

### 3. Interaction History

![Interaction History](tableau/screenshots/Interaction%20History.png)

This dashboard highlights how customer relationship history changes subscription likelihood. It is the strongest evidence that prior engagement should guide future targeting.

**Key takeaways**

- Previously contacted customers: **5,625**.
- Prior successful customers: **1,373**.
- Customers with prior campaign success convert at **65.11%**.
- Cold contacts convert at only **8.83%**.
- Previous campaign success is the strongest single driver identified in the project.

### 4. Economic Impact

![Economic Impact](tableau/screenshots/Economic%20Impact.png)

This dashboard connects campaign outcomes to macroeconomic conditions such as interest rate, employment variation, consumer confidence, and the derived rate environment.

**Key takeaways**

- Low-rate environments convert at **24.02%**.
- High-rate environments convert at only **4.83%**.
- The medium-rate segment is small but shows very high conversion, requiring careful interpretation because of lower sample size.
- Average interest rate in the cleaned dataset is **3.62%**.
- The dashboard supports timing campaigns around favorable rate conditions instead of treating every macro period equally.

---

## Business Problem

The bank needs to improve the efficiency of its term deposit marketing campaigns. Historically, the campaign relied on broad phone outreach, but the dataset shows that many contacts do not convert and repeated calling can reduce success. The business goal is to identify the customers, channels, timing, and economic conditions that produce higher conversion so campaign teams can prioritize effort more intelligently.

**Decision supported:**  
Help marketing managers decide whom to contact, how often to contact them, which channel to use, and when campaign timing is most favorable.

---

## Dataset

| Attribute | Details |
|---|---|
| Dataset | Bank Marketing Campaign Dataset |
| Source | UCI Machine Learning Repository |
| Institution | Portuguese banking institution |
| Raw file | [`data/raw/raw_dataset.csv`](data/raw/raw_dataset.csv) |
| Cleaned file | [`data/processed/cleaned_bank_data.csv`](data/processed/cleaned_bank_data.csv) |
| Raw shape | 41,188 rows x 21 columns |
| Clean shape | 41,176 rows x 27 columns |
| Target variable | `subscription_flag` |
| Granularity | One row per customer campaign contact |

For detailed column definitions, cleaning notes, derived variables, and statistical test mapping, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## Data Preparation

The cleaned dataset was produced through a structured notebook pipeline:

1. [`01_extraction.ipynb`](notebooks/01_extraction.ipynb) loads and audits the raw campaign dataset.
2. [`02_cleaning.ipynb`](notebooks/02_cleaning.ipynb) standardizes fields, removes duplicates, handles sentinel values, and creates derived columns.
3. [`03_eda.ipynb`](notebooks/03_eda.ipynb) explores customer, campaign, history, and economic patterns.
4. [`04_statistical_analysis.ipynb`](notebooks/04_statistical_analysis.ipynb) validates dashboard findings with ANOVA, Chi-Square tests, correlation, and logistic regression.
5. [`05_final_load_prep.ipynb`](notebooks/05_final_load_prep.ipynb) prepares the final Tableau-ready extract.

Key cleaning decisions:

- Removed **12 exact duplicate records**.
- Renamed raw fields into business-readable column names.
- Converted the target variable into binary `subscription_flag`.
- Replaced the raw `pdays` sentinel value with `contact_recency`.
- Capped extreme `num_contacts_campaign` values at 10.
- Created dashboard-ready features including `age_group`, `call_intensity`, `contacted_before`, `rate_environment`, and `prev_success_flag`.
- Retained `Unknown` categorical values rather than imputing them to avoid bias.

---

## KPI Framework

| KPI | Definition | Business Use |
|---|---|---|
| Conversion Rate | Successful subscriptions / total contacted customers | Primary campaign success metric |
| Successful Customers | Count of customers with `subscription_flag = 1` | Measures campaign output |
| Contact Channel Conversion | Conversion rate by `contact_type` | Compares cellular vs telephone performance |
| Call Intensity Conversion | Conversion rate by `call_intensity` | Finds when additional outreach becomes counterproductive |
| Prior Success Conversion | Conversion rate where previous campaign outcome was successful | Identifies high-value retargeting group |
| Cold Contact Conversion | Conversion rate for customers with no previous campaign success | Benchmarks new-customer outreach |
| Rate Environment Conversion | Conversion rate by low, medium, or high interest-rate environment | Connects marketing timing to macroeconomic context |

---

## Statistical Validation

The project validates dashboard patterns using formal tests at alpha = 0.05.

| Question | Feature | Test | Result | Dashboard |
|---|---|---|---|---|
| Do age groups convert differently? | `age_group` | ANOVA | Significant | Customer Profile |
| Is job type associated with subscription? | `job_type` | Chi-Square | Significant | Customer Profile |
| Does call intensity affect conversion? | `call_intensity` | ANOVA | Significant | Campaign Effectiveness |
| Does contact channel matter? | `contact_type` | Chi-Square | Significant | Campaign Effectiveness |
| Does prior campaign success predict current success? | `prev_success_flag` | Chi-Square | Highly significant | Interaction History |
| Does rate environment affect subscription? | `rate_environment` | ANOVA | Significant | Economic Impact |

The statistical analysis confirms that the Tableau insights are not only visual patterns: customer demographics, campaign strategy, relationship history, and macroeconomic conditions all measurably influence subscription outcomes.

---

## Key Insights

1. Prior campaign success is the strongest conversion signal: prior-success customers convert at **65.11%** versus **9.41%** for others.
2. Cold outreach is inefficient: customers with no previous campaign outcome convert at only **8.83%**.
3. Recency matters: recently contacted customers convert at **65.76%**, while never-contacted customers convert at **9.26%**.
4. Cellular is the better channel: **14.74%** conversion versus **5.23%** for telephone.
5. More calls do not mean more success: conversion declines from **13.04%** for one-call customers to **5.49%** for high-intensity contacts.
6. Students and retired customers are high-value segments, converting at **31.43%** and **25.26%** respectively.
7. Blue-collar customers form a large contacted group but convert at only **6.90%**, suggesting a need for different messaging or lower prioritization.
8. Low-rate environments are much more favorable than high-rate periods: **24.02%** conversion versus **4.83%**.
9. Macroeconomic variables are highly related, so `rate_environment` is the clearest Tableau dimension for business interpretation.
10. Call duration is strongly associated with conversion but should be treated as a post-call diagnostic, not a pre-call targeting variable.

---

## Recommendations

| # | Recommendation | Evidence | Expected Impact |
|---|---|---|---|
| 1 | Prioritize customers with previous campaign success and recent contact history. | Prior-success conversion is **65.11%**; recently contacted conversion is **65.76%**. | Higher conversion with fewer wasted calls. |
| 2 | Shift campaign contact strategy toward cellular outreach. | Cellular conversion is **14.74%** vs **5.23%** for telephone. | Better channel efficiency and higher subscription yield. |
| 3 | Limit repeated calling and avoid over-contacting customers. | Conversion drops from **13.04%** at one call to **5.49%** at high intensity. | Lower customer fatigue and better agent productivity. |
| 4 | Segment campaign messaging for students and retired customers. | Students convert at **31.43%** and retired customers at **25.26%**. | Stronger targeting of naturally receptive groups. |
| 5 | Time aggressive campaigns during low-rate environments. | Low-rate conversion is **24.02%** vs **4.83%** in high-rate conditions. | Improved campaign timing and budget allocation. |

---

## Repository Structure

```text
Section_C_Group_11_Bank_Marketing_Dataset/
|
|-- README.md
|
|-- data/
|   |-- raw/
|   |   `-- raw_dataset.csv
|   `-- processed/
|       `-- cleaned_bank_data.csv
|
|-- docs/
|   `-- data_dictionary.md
|
|-- notebooks/
|   |-- 01_extraction.ipynb
|   |-- 02_cleaning.ipynb
|   |-- 03_eda.ipynb
|   |-- 04_statistical_analysis.ipynb
|   `-- 05_final_load_prep.ipynb
|
|-- scripts/
|   `-- etl_pipeline.py
|
|-- tableau/
|   |-- Bank Marketing Dataset.twbx
|   |-- dashboard_links.md
|   `-- screenshots/
|       |-- Customer Profile.png
|       |-- Campain Effectiveness.png
|       |-- Interaction History.png
|       `-- Economic Impact.png
|
|-- reports/
|   |-- README.md
|   |-- project_report_template.md
|   `-- presentation_outline.md
|
|-- DVA-oriented-Resume/
`-- DVA-focused-Portfolio/
```

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python | Data extraction, cleaning, EDA, statistical analysis |
| Jupyter Notebook | Reproducible analytical workflow |
| Tableau | Interactive dashboard design and presentation |
| GitHub | Version control and project collaboration |
| CSV | Raw and processed data storage |

Recommended Python libraries are listed in [`requirements.txt`](requirements.txt).

---

## How To Run Locally

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

Open the notebooks in numeric order from `01_extraction.ipynb` through `05_final_load_prep.ipynb`. Open the Tableau workbook from [`tableau/Bank Marketing Dataset.twbx`](tableau/Bank%20Marketing%20Dataset.twbx) to inspect or modify the dashboard suite.

---

## Current Deliverables

- Cleaned dataset: [`data/processed/cleaned_bank_data.csv`](data/processed/cleaned_bank_data.csv)
- Data dictionary: [`docs/data_dictionary.md`](docs/data_dictionary.md)
- Analysis notebooks: [`notebooks/`](notebooks/)
- Tableau workbook: [`tableau/Bank Marketing Dataset.twbx`](tableau/Bank%20Marketing%20Dataset.twbx)
- Dashboard screenshots: [`tableau/screenshots/`](tableau/screenshots/)
- Report support files: [`reports/`](reports/)

---

## Conclusion

The campaign should move from broad outreach to evidence-led targeting. The strongest opportunities are retargeting customers with successful prior outcomes, using cellular contact, reducing overcalling, prioritizing high-conversion customer segments, and timing campaigns around favorable interest-rate environments.

Together, the Python analysis and Tableau dashboards provide a complete decision framework for improving term deposit campaign conversion.
