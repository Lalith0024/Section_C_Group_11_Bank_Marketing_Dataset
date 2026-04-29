# Ankit Raj Singh — DVA-Focused Portfolio

**Live Portfolio:** [dva-portfolio-omega.vercel.app](http://dva-portfolio-omega.vercel.app)

## Full-Stack Developer & Data Visualization Analyst

I build AI-powered dashboards and data-driven systems using Next.js, TypeScript, Python, and Google Sheets/Tableau, turning complex datasets into clear, actionable insights. Alongside full-stack development, I work extensively with spreadsheets, pivot tables, and exploratory data analysis (EDA) to clean, analyze, and visualize data for decision-making.

---

## Portfolio Projects

---

### 1. Bank Marketing Campaign Analysis

**Repository:** [Section\_C\_Group\_11\_Bank\_Marketing\_Dataset](https://github.com/Lalith0024/Section_C_Group_11_Bank_Marketing_Dataset)

This banking analytics project studies a Portuguese banking institution's direct marketing campaign data to understand which customer groups are more likely to subscribe to a term deposit.

---

#### Problem Statement & Stakeholder Context

The bank needed to improve the efficiency of its term deposit marketing campaigns. Historically, the campaign relied on broad phone outreach, but the dataset revealed that many contacts do not convert and repeated calling can reduce success. The goal was to identify the customers, channels, timing, and economic conditions that produce higher conversion so campaign teams can prioritize effort more intelligently.

**Decision supported:** Help marketing managers decide whom to contact, how often, which channel to use, and when campaign timing is most favorable.

---

#### Dataset Source & Scope

| Attribute | Details |
|-----------|---------|
| Dataset | Bank Marketing Campaign Dataset |
| Source | UCI Machine Learning Repository |
| Institution | Portuguese banking institution |
| Raw shape | 41,188 rows × 21 columns |
| Cleaned shape | 41,176 rows × 27 columns |
| Successful subscriptions | 4,639 |
| Overall conversion rate | 11.27% |
| Target variable | `subscription_flag` |
| Granularity | One row per customer campaign contact |

---

#### Data Cleaning & Transformation Summary

The cleaned dataset was produced through a structured 5-notebook pipeline:

- `01_extraction.ipynb` — loads and audits the raw campaign dataset
- `02_cleaning.ipynb` — standardizes fields, removes duplicates, handles sentinel values, creates derived columns
- `03_eda.ipynb` — explores customer, campaign, history, and economic patterns
- `04_statistical_analysis.ipynb` — validates dashboard findings with ANOVA, Chi-Square, correlation, and logistic regression
- `05_final_load_prep.ipynb` — prepares the final Tableau-ready extract

Key cleaning decisions included removing 12 exact duplicate records, renaming raw fields into business-readable names, converting the target variable into binary `subscription_flag`, replacing the raw `pdays` sentinel with `contact_recency`, capping extreme `num_contacts_campaign` values at 10, and creating dashboard-ready features such as `age_group`, `call_intensity`, `contacted_before`, `rate_environment`, and `prev_success_flag`.

---

#### KPI Framework

| KPI | Definition | Business Use |
|-----|-----------|--------------|
| Conversion Rate | Successful subscriptions / total contacted | Primary campaign success metric |
| Successful Customers | Count where `subscription_flag = 1` | Measures campaign output |
| Contact Channel Conversion | Conversion rate by `contact_type` | Cellular vs telephone performance |
| Call Intensity Conversion | Conversion rate by `call_intensity` | Finds when additional outreach backfires |
| Prior Success Conversion | Rate where previous outcome was successful | Identifies high-value retargeting group |
| Cold Contact Conversion | Rate for customers with no prior success | Benchmarks new-customer outreach |
| Rate Environment Conversion | Rate by low, medium, or high interest-rate period | Connects timing to macroeconomic context |

---

#### Dashboard Screenshots & Links

The Tableau workbook covers four dashboards — Customer Profile, Campaign Effectiveness, Interaction History, and Economic Impact.

**Dashboard 1 — Customer Profile**

Profiles the customer base by job type, education, marital status, and loan ownership.

- Total contacts after cleaning: **41,176**
- Overall success rate: **11.27%**
- Students and retired customers show the strongest conversion rates; blue-collar customers lag.

**Dashboard 2 — Campaign Effectiveness**

Evaluates how outreach strategy affects conversion by call intensity, channel, month, and previous outcome.

- One-call campaigns convert at **13.04%**; high-intensity campaigns fall to **5.49%**
- Cellular outreach converts at **14.74%** vs **5.23%** for telephone
- March, December, September, and October show the strongest monthly patterns

**Dashboard 3 — Interaction History**

Shows how customer relationship history changes subscription likelihood.

- Customers with prior campaign success convert at **65.11%**
- Cold contacts convert at only **8.83%**
- Prior campaign success is the single strongest driver identified in the project

**Dashboard 4 — Economic Impact**

Connects campaign outcomes to macroeconomic conditions such as interest rate and consumer confidence.

- Low-rate environments convert at **24.02%**
- High-rate environments convert at only **4.83%**
- Average interest rate in the cleaned dataset: **3.62%**

---

#### Key Insights

1. Prior campaign success is the strongest signal: prior-success customers convert at **65.11%** vs **9.41%** for others.
2. Cold outreach is inefficient: customers with no prior outcome convert at only **8.83%**.
3. Recency matters: recently contacted customers convert at **65.76%**; never-contacted at **9.26%**.
4. Cellular is the better channel: **14.74%** vs **5.23%** for telephone.
5. More calls do not mean more success: conversion declines from **13.04%** (one call) to **5.49%** (high intensity).
6. Students and retired customers are high-value segments, converting at **31.43%** and **25.26%** respectively.
7. Blue-collar customers are a large group but convert at only **6.90%**.
8. Low-rate environments are far more favorable: **24.02%** vs **4.83%** in high-rate conditions.

---

#### Recommendations & Expected Impact

| # | Recommendation | Evidence | Expected Impact |
|---|---------------|---------|----------------|
| 1 | Prioritize customers with prior campaign success and recent contact history | Prior-success: 65.11%; recent: 65.76% | Higher conversion with fewer wasted calls |
| 2 | Shift to cellular outreach | 14.74% vs 5.23% for telephone | Better channel efficiency |
| 3 | Limit repeated calling | Drops from 13.04% at one call to 5.49% at high intensity | Lower customer fatigue, better agent productivity |
| 4 | Segment messaging for students and retired customers | 31.43% and 25.26% conversion | Stronger targeting of receptive groups |
| 5 | Time campaigns during low-rate environments | 24.02% vs 4.83% in high-rate periods | Improved budget allocation |

---

### 2. Steam Games Analytics — Engagement, Popularity & Market Structure

**Repository:** [SectionC\_Group2\_SteamGames](https://github.com/mahir-m01/SectionC_Group2_SteamGames)

This gaming analytics project analyzes how pricing models, content structure, platform accessibility, and user feedback influence game popularity, engagement, and player satisfaction on the Steam marketplace.

---

#### Problem Statement & Stakeholder Context

Steam hosts over 90,000 games, yet only a tiny fraction achieve meaningful player engagement. Publishers, indie developers, and platform strategists need to understand what actually drives discoverability, concurrency, and sentiment to make informed decisions on pricing, content depth, and platform targeting.

**Decision supported:** Help game developers and publishers understand which factors — pricing model, content richness, platform availability, recommendation volume — most strongly predict high player engagement and market success.

---

#### Dataset Source & Scope

| Attribute | Details |
|-----------|---------|
| Dataset | Steam Games Metadata |
| Source | Kaggle (Steam Games Dataset) |
| Original size | ~90,000 games |
| Analysis sample | ~7,000 games |
| Sampling method | Top titles by descending Peak CCU (concurrent users) |
| Granularity | One row per game |

---

#### Data Cleaning & Transformation Summary

All cleaning and preparation was executed in **Google Sheets** as required by the project brief.

Key steps included removing non-analytical text, media, and metadata-heavy columns; standardizing data types (dates, currency, percentages, numeric fields); converting array-based fields (languages, genres, platforms) into count metrics; normalizing platform indicators into Boolean format; standardizing column naming; and filling missing playtime values using column averages (applied cautiously in analysis).

**Feature Engineering Highlights:**

- **Total Reviews** = Positive + Negative
- **Weighted Sentiment** = % Positive × ln(Total Reviews + 1)
- **Pricing Type** — Free-to-Play / Paid
- **Platform Count** — Windows + Mac + Linux
- **Popularity Tier** — based on Peak CCU thresholds (Niche, Mid-Tier, Blockbuster)
- **Discount Impact** = Discount × Peak CCU
- **Price Band, DLC Band, Recommendation Band**

---

#### KPI Framework

| KPI | Definition | Business Use |
|-----|-----------|--------------|
| Average Peak CCU | Mean concurrent users by segment | Core engagement benchmark |
| Weighted Sentiment Score | % Positive × ln(Total Reviews + 1) | Quality-adjusted satisfaction metric |
| Market Concentration (Niche %) | Share of games below CCU threshold | Reveals platform skew and discovery difficulty |
| Pricing Type Engagement Ratio | Peak CCU for F2P vs Paid | Informs monetization strategy |
| Recommendation-CCU Correlation | Relationship between review volume and concurrency | Measures organic discovery effect |
| Content Depth Score | Genre count + DLC count + platform count | Proxy for game richness |
| Discount Impact Score | Discount × Peak CCU | Measures promotion effectiveness |

---

#### Dashboard Screenshots & Links

The analysis is delivered through an interactive Google Sheets dashboard featuring KPI cards for engagement, satisfaction, market structure, and discovery; charts segmented by Pricing Type, Popularity Tier, Content Depth, and Social Proof; slicers for dynamic filtering; and a dark theme inspired by the Steam desktop interface.

**[Preview Live Dashboard](https://docs.google.com/spreadsheets/d/1DPXJUWniEpypmdDw2xhfW4OhgbheznYVoae_yg3CmW4/edit?gid=293185833#gid=293185833&fvid=240560395)**

---

#### Key Insights

1. Free-to-Play games show **~5.7× higher average Peak CCU** than Paid titles.
2. Over **93% of games are classified as Niche**, with very low concurrent users — discoverability is the central challenge on Steam.
3. A small number of Blockbuster titles generate a **disproportionate share of total engagement** across the platform.
4. Higher recommendation volumes strongly correlate with higher Peak CCU, confirming an **organic discovery flywheel**.
5. Multi-genre and content-rich games tend to achieve higher player concurrency.
6. Higher-priced games generally show **stronger sentiment** scores, though not broader reach.

---

#### Recommendations & Expected Impact

| # | Recommendation | Evidence | Expected Impact |
|---|---------------|---------|----------------|
| 1 | Adopt Free-to-Play or freemium pricing for broader audience reach | F2P games average 5.7× higher Peak CCU | Significantly wider top-of-funnel engagement |
| 2 | Invest in review volume growth through community and launch campaigns | Recommendation volume correlates strongly with CCU | Triggers organic discovery; improves ranking signals |
| 3 | Expand platform availability (Mac + Linux) to increase addressable audience | Multi-platform games show higher content depth scores | Incremental CCU gains at low marginal cost |
| 4 | Develop richer content (DLCs, multi-genre appeal) to boost content depth scores | Content-rich games achieve higher concurrency | Longer engagement cycles and higher sentiment |
| 5 | Use targeted discounts strategically for mid-tier games rather than blockbusters | Discount Impact analysis reveals diminishing returns at top tier | Better ROI on promotional spending |

---

## Tools Used

| Category | Tools |
|----------|-------|
| Languages | Python, SQL, HTML, CSS, JavaScript |
| Frameworks | Next.js, React |
| Data Tools | Pandas, Google Sheets, Tableau, Jupyter Notebook |
| Deployment | GitHub, Vercel |

---

## Portfolio Structure

```
dva-portfolio-omega/
├── src/               # Next.js source code (components, pages, styles)
├── public/            # Static assets (images, dashboard screenshots)
└── next.config.ts     # Configuration for the Next.js application
```

---

## Purpose

This portfolio was created to convert DVA capstone work into a clear, accessible, and deployable case-study format. It is designed so evaluators, mentors, recruiters, or teammates can quickly understand the business problem, data work, dashboard output, insights, and impact of each project.

---

*Newton School of Technology — Data Visualization & Analytics | Capstone Portfolio*
