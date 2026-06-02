# Side project — Inventory forecast service

## Facts

- **Name:** Inventory forecast service (personal / hackathon extension)
- **Period:** 2020-01 – 2020-08 (evenings)
- **Role:** Solo builder
- **Not employment** — label as Project in resume if used

## Tools

Python, scikit-learn, Airflow (local), SQLite → PostgreSQL

## Metrics

- Demo dataset: 50 SKUs, MAPE improved from 22% to 15% vs. moving average baseline

## Raw bullets

- Built batch pipeline to ingest CSV sales history and train weekly demand models per SKU.
- Scheduled Airflow DAG for retrain + export; documented assumptions for seasonality.
- Open-sourced README and sample data (no proprietary employer data used).
