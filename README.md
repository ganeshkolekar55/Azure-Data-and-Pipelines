# Azure Data Factory — Selective CSV File Copy Pipeline

## What This Pipeline Does

This pipeline automates selective file ingestion from a raw data container to a reporting folder in **Azure Data Lake Storage (ADLS)**. Instead of copying all files blindly, it filters files based on a filename keyword condition — copying only relevant CSV files to the destination.

## Pipeline Logic

```
Get Metadata (source folder)
        │
        ▼
  ForEach (loop through all files)
        │
        ▼
  If Condition → filename contains "ca"
        │
        ├── TRUE  → Copy file to /reporting folder
        └── FALSE → Skip
```

## Result

From the source folder, only matching files are copied:

| File | Contains "ca" | Action |
|------|--------------|--------|
| cars.csv | ✅ Yes | Copied to reporting folder |
| orders.csv | ❌ No | Skipped |

## Tech Stack

| Component | Tool |
|-----------|------|
| Orchestration | Azure Data Factory |
| Storage | Azure Data Lake Storage Gen2 |
| Source | ADLS Container — raw folder |
| Destination | ADLS Container — reporting folder |
| Filter activity | If Condition (contains expression) |
| Loop activity | ForEach |
| Metadata activity | Get Metadata (child items) |

## ADF Expression Used

```
@contains(item().name, 'ca')
```

This expression is evaluated inside the **If Condition** activity for each file returned by Get Metadata.

## Key Concepts Demonstrated

- **Metadata-driven pipeline design** — no hardcoded filenames
- **Dynamic filtering** using ADF expressions
- **ForEach + IfCondition** pattern — standard in production data engineering
- **ADLS Gen2 integration** via linked services and datasets
- **Scalable pattern** — add more files to the source folder and the pipeline handles them automatically without any code change

## Pipeline Screenshots

> *(Add screenshot of ADF canvas here)*

## How to Reuse This Pipeline

1. Clone this repo
2. Import the JSON from `/pipeline/` into your own ADF instance
3. Update the **Linked Service** to point to your ADLS account
4. Update the **source** and **sink** dataset folder paths
5. Change the filter keyword in the If Condition expression if needed

## Author

Ganesh Kolekar — [GitHub](https://github.com/ganeshkolekar55)
