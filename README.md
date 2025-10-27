# Megaline Plans - Statistical Data Analysis

## Objective
Evaluate which cellular plan (**Surf** vs **Ultimate**) delivers higher **monthly revenue** and how user behavior (calls, messages, internet) differs between plans using **statistical inference**.

## Datasets
- `megaline_users.csv` - demographics, city, plan, registration/churn dates  
- `megaline_calls.csv` - per-call usage with duration (sec/min) and date  
- `megaline_messages.csv` - message counts by date  
- `megaline_internet.csv` - internet traffic (MB) by date  
- `megaline_plans.csv` - plan terms & pricing

### Plan Parameters
| Plan | Monthly Fee (USD) | Minutes Included | Messages Included | Data Included (MB) | Overages: per min (USD) | per message (USD) | per GB (USD) |
|---|---:|---:|---:|---:|---:|---:|---:|
| Surf | 20 | 500 | 50 | 15,360 | 0.03 | 0.03 | 10 |
| Ultimate | 70 | 3,000 | 1,000 | 30,720 | 0.01 | 0.01 | 7 |

---

## Methodology

### 1. Data Cleaning & Standardization
- Standardized column names and types; validated date fields
- Removed duplicates; handled missing values conservatively (documented imputation/exclusion rules in notebook)

### 2. Feature Engineering (Monthly Aggregation)
- **Calls:** aggregated minutes per user-month (rounded up per-minute billing where specified)
- **Messages:** counted SMS per user-month
- **Internet:** converted MB → GB and aggregated per user-month
- **Revenue** per user-month = monthly fee + overages on minutes/SMS/GB based on plan rules

### 3. Exploratory Data Analysis (EDA)
- Distribution checks for minutes, SMS, GB, and revenue by plan
- Seasonal effects and cohort tenure checks (reg_date, churn)
- City-level cut (NYC metro vs other MSAs) for sensitivity analysis

### 4. Hypothesis Testing
- **H1:** Mean monthly revenue differs between Ultimate and Surf plans
  - Test: **Two-sample Welch's t-test** (unequal variances), α = 0.05
  - Assumptions: CLT justified by large n; inspected skew/outliers; reported effect size (Cohen's d)
- **H2:** Usage behavior differs by plan (minutes/SMS/GB)
  - Tests: t-tests for means; Levene for variance heterogeneity where relevant
- **H3 (optional):** NYC metro vs. other regions differ in revenue for the same plan
  - Test: two-sample t-test; reported 95% CI of mean difference

### 5. Validation & Robustness
- Sensitivity to outliers via trimmed means / winsorization checks
- Re-ran tests on 2018-2019 overlapping active months only (reduce survivorship bias)
- Confirmed conclusions stable to reasonable preprocessing choices

---

## Key Metrics Reported
- Mean ± SD of monthly revenue by plan  
- 95% confidence intervals of mean differences  
- t-statistics, p-values (Welch's) and **Cohen's d** effect sizes  
- Plan-level usage profiles (minutes, SMS, GB)

> **Note:** Numerical results, figures, and tables are in the notebook; the README focuses on method and interpretation.

---

## Tech Stack
**Python** · pandas · numpy · matplotlib · seaborn · **scipy.stats** (Welch's t-test, Levene)

---

## Interpretation Guide
- **Statistical significance (p < 0.05):** the mean difference is unlikely due to sampling noise
- **Practical significance:** rely on **effect size** (|d| ≥ 0.2 small, 0.5 medium, 0.8 large) and revenue deltas to judge business value
- **Plan decisioning:** combine revenue findings with usage behavior (minutes/SMS/GB) to shape pricing and marketing strategy

---

## Repository Structure
```
/data
  ├── megaline_users.csv
  ├── megaline_calls.csv
  ├── megaline_messages.csv
  ├── megaline_internet.csv
  └── megaline_plans.csv
notebook_sda.ipynb          # full analysis
README.md                   # this file
```

---

## Links
- **Full Notebook:** [View on GitHub](#)  
- **Author:** Dr. Danisha L. Thomas
