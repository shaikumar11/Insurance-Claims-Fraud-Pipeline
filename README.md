# Insurance Claims Fraud Pipeline

A PySpark-based ETL pipeline built on Databricks to ingest, clean, validate, and analyze auto insurance claims data, with a focus on fraud detection patterns.

## Overview
This project processes 1,000 auto insurance claims (40 attributes including policy details, incident details, claim amounts, and fraud status) through a full ETL workflow, surfacing fraud-related insights via SQL.

## Pipeline Steps
1. **Ingest** – Load raw CSV claims data into a Spark DataFrame
2. **Explore** – Profile the dataset to detect nulls and placeholder missing values
3. **Clean** – Drop an empty junk column (`_c39`), replace `'?'` placeholder values with proper labels across 3 fields (`collision_type`, `property_damage`, `police_report_available` — 178/360/343 affected rows), and fill 91 nulls in `authorities_contacted`
4. **Validate** – Automated assertions confirm zero nulls in key fields, zero negative claim amounts, and successful removal of the junk column
5. **Transform** – Derive new features: claim-to-premium ratio and a high-risk claim flag (major damage + no police report + high claim amount)
6. **Store** – Write cleaned data (1,000 records) to Delta Lake
7. **Analyze** – Run SQL queries to surface fraud patterns across severity, collision type, and police report status

## Key Findings
| Insight | Result |
|---|---|
| Highest fraud rate by severity | Major Damage claims (60.5%) |
| Lowest fraud rate by severity | Trivial Damage claims (6.7%) |
| Avg. claim amount (fraud vs. legitimate) | ₹60,302 (fraud) vs. ₹50,289 (legitimate) — ~20% higher |
| Highest fraud rate by collision type | Rear Collision (31.2%) |
| Effect of police report availability | Minimal — fraud rate stays 22.9–25.9% regardless of report status |

## Tech Stack
- **Databricks** (compute + workspace)
- **PySpark** (data processing)
- **Delta Lake** (storage)
- **SQL** (analytics)

## Files
- `insurance_claims_pipeline.ipynb` – full notebook (ingest, clean, validate, transform, store, analyze)
- `insurance_fraud_claims.csv` – source dataset (auto insurance claims with fraud labels)

## How to Run
1. Upload `insurance_fraud_claims.csv` to a Databricks Volume or DBFS
2. Open `insurance_claims_pipeline.ipynb` in a Databricks workspace and attach it to a cluster
3. Run cells sequentially — the notebook ingests, cleans, validates, stores, and analyzes the data end-to-end
