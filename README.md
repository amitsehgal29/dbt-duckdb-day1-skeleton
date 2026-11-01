# dbt-duckdb-day1-skeleton

Day 1 of a 7-day dbt + Airflow series: a minimal, runnable dbt project on DuckDB. No warehouse or cloud account needed; everything runs in a local `shop.duckdb` file.

## What's inside

- `seeds/raw_orders.csv`: 8 sample orders with the usual mess (mixed-case status, stray spaces, text timestamps, a refund row).
- `models/staging/stg_orders.sql`: a staging view that renames, trims, lowercases and casts the seed into clean types.
- `dbt_project.yml`: project config. The seed loads every column as text, so all the typing happens in staging.
- `profiles.yml.example`: a DuckDB profile pointing at `shop.duckdb`.

## Run it

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp profiles.yml.example profiles.yml
dbt debug --profiles-dir .
dbt seed --profiles-dir .
dbt run --profiles-dir .
```

Query the result:

```bash
python3 -c "import duckdb; print(duckdb.connect('shop.duckdb').sql('select * from main.stg_orders'))"
```

## The series

1. dbt skeleton on DuckDB (this repo)
2. Tests and docs
3. Incremental model and SCD2 snapshot
4. Airflow DAG running dbt
5. Sensors, retries and alerts
6. Data-quality gates
7. End-to-end demo
