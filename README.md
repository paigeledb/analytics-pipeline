# P&C Analytics Pipeline
 
An end-to-end insurance data warehouse built with **dbt Core**, **Snowflake**, and **Python** — modeling Property & Casualty claims and financial performance data from raw ingestion through a full analytical layer to Power BI dashboards.
 
---
 
## Overview
 
This project mirrors a production analytics engineering workflow in the Property & Casualty insurance industry. Starting from raw public datasets, it builds a fully modeled, tested, and documented data warehouse with a reporting layer designed for business decision-making.
 
**Business questions this project answers:**
- How are claims trending over time by region and coverage type?
- What is the loss ratio, expense ratio, and combined ratio by period?
- Where are the highest-cost claims concentrated geographically?
- How does financial performance vary across time and geography?
 
---
 
## Architecture
 
```
Raw CSV Files (Kaggle / NAIC / Census)
        │
        ▼  Python ingestion scripts
┌─────────────────────────┐
│   Snowflake Raw Schema  │  ← Landing zone (untransformed source data)
└─────────────────────────┘
        │
        ▼  dbt staging models
┌──────────────────────────────┐
│  Snowflake Staging Schema    │  ← Cleaned, renamed, typed source data
└──────────────────────────────┘
        │
        ▼  dbt intermediate models
┌─────────────────────────────────────┐
│  Snowflake Intermediate Schema      │  ← Business logic, joins, enrichment
└─────────────────────────────────────┘
        │
        ▼  dbt mart models
┌──────────────────────────────┐
│  Snowflake Mart Schema       │  ← Reporting-ready analytical assets
└──────────────────────────────┘
        │
        ▼
┌──────────────────────────────┐
│  Power BI Dashboards         │  ← Claims Performance · Financial Performance
└──────────────────────────────┘
```
 
---
 
## Tech Stack
 
| Tool | Purpose |
|---|---|
| **Python** | Raw data ingestion — reads CSVs, loads to Snowflake raw schema |
| **Snowflake** | Cloud data warehouse — hosts all schema layers |
| **dbt Core** | Transformation layer — staging, intermediate, and mart models |
| **Power BI Desktop** | Reporting layer — dashboards built on Snowflake mart tables |
| **GitHub** | Version control and project documentation |
| **VS Code** | Development environment |
 
---
 
## Data Sources
 
| Dataset | Source | Description |
|---|---|---|
| Auto Insurance Claims | [Kaggle](https://www.kaggle.com) | Core claims fact data including claim type, amount, and geography |
| P&C Financial Statements | [NAIC](https://www.naic.org) | Premium, loss, and expense data by period |
| US ZIP Code Geography | [US Census Bureau](https://www.census.gov) | Geographic dimension for regional analysis |
 
---
 
## Project Structure
 
```
pc-analytics-pipeline/
│
├── README.md
│
├── ingestion/
│   ├── load_claims.py          ← Loads raw claims CSV to Snowflake
│   ├── load_financials.py      ← Loads raw financials CSV to Snowflake
│   ├── load_geography.py       ← Loads ZIP code reference data to Snowflake
│   └── requirements.txt        ← Python dependencies
│
├── dbt_project/
│   ├── dbt_project.yml
│   ├── .gitignore
│   │
│   ├── models/
│   │   ├── staging/
│   │   │   ├── stg_claims.sql
│   │   │   ├── stg_financials.sql
│   │   │   ├── stg_geography.sql
│   │   │   └── staging.yml         ← Tests & documentation
│   │   │
│   │   ├── intermediate/
│   │   │   ├── int_claims_enriched.sql
│   │   │   ├── int_financials_by_period.sql
│   │   │   └── intermediate.yml
│   │   │
│   │   └── marts/
│   │       ├── mart_claims_summary.sql
│   │       ├── mart_financial_performance.sql
│   │       ├── mart_claims_by_geography.sql
│   │       └── marts.yml
│   │
│   ├── seeds/
│   │   └── geography.csv
│   │
│   └── analyses/
│       └── loss_ratio_exploration.sql
│
└── dashboards/
    ├── claims_performance.png
    └── financial_performance.png
```
 
---
 
## Data Models
 
### Staging Layer
Cleans and standardizes raw source data. One staging model per source.
 
| Model | Description |
|---|---|
| `stg_claims` | Cleaned claims data — renamed fields, typed columns, nulls handled |
| `stg_financials` | Cleaned financial data — premiums, losses, expenses by period |
| `stg_geography` | ZIP code reference — state, region, lat/lon |
 
### Intermediate Layer
Applies business logic and joins across sources.
 
| Model | Description |
|---|---|
| `int_claims_enriched` | Claims joined to geography for regional analysis |
| `int_financials_by_period` | Financial metrics aggregated and structured by reporting period |
 
### Mart Layer
Reporting-ready analytical assets consumed directly by Power BI.
 
| Model | Description |
|---|---|
| `mart_claims_summary` | Claim volume, average cost, and loss trends by region and period |
| `mart_financial_performance` | Loss ratio, expense ratio, and combined ratio by period |
| `mart_claims_by_geography` | Geographic distribution of claim counts and total claim costs |
 
---
 
## Dashboards
 
### Claims Performance
- Claim volume over time
- Average claim cost by region and coverage type
- Loss ratio trend
 
*(Screenshot coming soon)*
 
### Financial Performance
- Premium vs. losses vs. expenses
- Combined ratio trend by period
- Geographic profitability view
 
*(Screenshot coming soon)*
 
---
 
## How to Run This Project
 
### Prerequisites
- Python 3.11+
- Snowflake account (free trial available at [snowflake.com](https://www.snowflake.com))
- dbt Core installed (`pip install dbt-snowflake`)
- VS Code or any code editor
 
### Setup
 
**1. Clone the repo**
```bash
git clone https://github.com/paigeledb/analytics-pipeline.git
cd analytics-pipeline
```
 
**2. Install Python dependencies**
```bash
pip install -r ingestion/requirements.txt
```
 
**3. Configure Snowflake credentials**
 
Create a `profiles.yml` file in your dbt directory (do not commit this file — it is in `.gitignore`):
```yaml
pc_analytics:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: your_account
      user: your_username
      password: your_password
      role: your_role
      database: ANALYTICS
      warehouse: your_warehouse
      schema: DBT_DEV
```
 
**4. Run Python ingestion scripts**
```bash
python ingestion/load_claims.py
python ingestion/load_financials.py
python ingestion/load_geography.py
```
 
**5. Run dbt**
```bash
cd dbt_project
dbt deps
dbt run
dbt test
```
 
**6. Explore documentation**
```bash
dbt docs generate
dbt docs serve
```
 
---
 
## About
 
Built by **Paige Ledbetter**, Senior Data Analyst & Analytics Engineer with 10+ years of experience in insurance data and analytics.
 
- 🔗 [LinkedIn](https://www.linkedin.com/in/paigeledb)
- 💼 [Portfolio](https://paigeledb.github.io) *(coming soon)*
