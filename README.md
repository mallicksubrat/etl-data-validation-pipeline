# 🔄 ETL Data Validation Pipeline

A modular ETL pipeline that ingests CSV data, validates schema and data quality, and outputs structured JSON/JSONL for downstream processing.

---

## 📌 Problem
Raw CSV data is often inconsistent and unreliable, leading to downstream failures in data pipelines and analytics systems.

---

## ⚙️ Solution
This project implements a lightweight ETL pipeline with:

- CSV ingestion  
- Schema and data validation  
- Transformation into JSON / JSONL formats  
- Modular and testable components  

---

## 🛠 Tech Stack
Python • Pandas • CLI • Pytest  

---

## 🔥 Key Features
- Data validation (numeric checks, required fields)  
- Modular architecture (reader, validator, writer)  
- CLI-based execution  
- Unit-tested components  

---

## 🚀 Usage

### Basic run
```bash
python -m src.csv_to_json.main input.csv output.json
```

### With validation
```bash
python -m src.csv_to_json.main input.csv output.json --validate
```

### JSON Lines output
```bash
python -m src.csv_to_json.main input.csv output.jsonl --format jsonl --validate
```

---

## 📦 Project Structure
- `reader.py` → CSV ingestion  
- `validator.py` → data validation  
- `writer.py` → JSON/JSONL output  
- `main.py` → CLI entrypoint  

---

## 💡 What I Learned
- Building modular ETL components  
- Designing validation layers for data pipelines  
- Writing testable and maintainable Python code 
