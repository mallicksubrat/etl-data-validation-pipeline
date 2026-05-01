# ETL Data Validation Pipeline

A modular ETL pipeline that ingests CSV data, enforces schema and data quality rules, and outputs structured JSON/JSONL for downstream processing.

---

## Problem

Raw CSV data is often inconsistent and unreliable. Missing required fields, incorrect data types, and malformed records cause silent failures in downstream pipelines and analytics systems.

---

## Solution

A lightweight but production-patterned ETL pipeline with strict validation:

- CSV ingestion with configurable schema
- Schema validation — required fields, data types, format checks
- Data quality rules — numeric range checks, null enforcement
- Modular output — JSON or JSONL format
- CLI-driven execution with validation toggle

---

## Architecture

CSV Source → Reader → Validator → Transformer → Writer → JSON / JSONL Output

**Data flow:**
1. Reader ingests CSV and parses into structured records
2. Validator enforces schema rules — fails loudly on violations
3. Transformer normalizes fields into canonical format
4. Writer outputs JSON or JSONL depending on CLI flag

---

## Tech Stack

- **Python** — processing language
- **Pandas** — CSV ingestion and in-memory transformation
- **Pytest** — unit tests for each pipeline component
- **CLI** — flag-based execution for flexibility

---

## Key Features

- Schema validation with configurable rules
- Data quality checks — numeric types, required fields, range enforcement
- Modular architecture — reader, validator, writer as separate testable units
- Dual output format — JSON for single-record use, JSONL for streaming-compatible output
- Idempotent processing — same input always produces same output

---

## Project Structure

etl-data-validation-pipeline/
├── src/
│   └── csv_to_json/
│       ├── reader.py       # CSV ingestion
│       ├── validator.py    # Schema and quality validation
│       ├── writer.py       # JSON/JSONL output
│       └── main.py         # CLI entrypoint
├── sample_data/            # Synthetic CSV samples
├── tests/                  # Unit tests per component
├── requirements.txt        # Dependencies
└── .gitignore

---

## Usage

### Basic run

```bash
python -m src.csv_to_json.main input.csv output.json
```

### With validation enabled

```bash
python -m src.csv_to_json.main input.csv output.json --validate
```

### JSONL output

```bash
python -m src.csv_to_json.main input.csv output.jsonl --format jsonl --validate
```

---

## Engineering Decisions and Trade-offs

**Why separate reader, validator, writer modules:**
Each component is independently testable. Failures isolate to the responsible module — a validation error doesn't obscure an ingestion bug.

**Why fail loudly on validation errors:**
Silent data quality failures are worse than pipeline failures. A downstream analytics query on corrupted data produces wrong results that may not be caught for days. Explicit failure surfaces the problem immediately.

**Why support both JSON and JSONL:**
JSON is convenient for small datasets and human inspection. JSONL is streaming-compatible and works naturally with tools like Spark, Kafka consumers, and log aggregators. Supporting both makes the pipeline more versatile.

**Failure modes considered:**
- Empty CSV input — reader returns empty list, writer outputs empty file, no crash
- CSV with missing required columns — validator catches at schema check, fails with descriptive error
- Mixed data types in numeric column — validator flags offending rows with row index for easy debugging

**At 10x scale:**
Would replace Pandas with PySpark for distributed CSV processing, add Great Expectations for richer data quality reporting, and store output to cloud storage (S3 or GCS) instead of local filesystem.

---

## Future Improvements

- Cloud storage output (S3, GCS)
- Great Expectations integration for data quality reporting
- Schema definition via external config file
- REST API wrapper for pipeline triggering

---

## Dataset

Synthetic CSV data in `sample_data/` simulates realistic records with intentional quality issues — missing fields, wrong types, out-of-range values — to demonstrate validation behavior.
