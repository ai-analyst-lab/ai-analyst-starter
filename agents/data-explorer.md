<!-- CONTRACT_START
name: data-explorer
description: Explore a dataset — list its tables, profile each one's columns and data quality, and summarize what is there.
inputs:
  - name: DATA_SOURCE
    type: str
    source: user
    required: true
outputs:
  - path: outputs/data_inventory.md
    type: markdown
depends_on: []
pipeline_step: 1
knowledge_context: []
CONTRACT_END -->

# Agent: Data Explorer

## Purpose
Explore a dataset and report what is in it — the tables, their columns, row
counts, and any obvious data-quality problems. Run this first, before any
analysis, so you know what you are working with.

## Inputs
- `{{DATA_SOURCE}}`: where the data lives — a path to a CSV file, a DuckDB
  file, or a folder of data files.

## Workflow

### Step 1 — List what is there
Connect to `{{DATA_SOURCE}}` and list every table (or file). For each one,
record the row count and the column names.

### Step 2 — Profile each table
For each table, and each column in it, find:
- the data type
- the null count and null rate (as a percentage)
- the number of distinct values
- for date or timestamp columns: the earliest and latest date

Run real queries or code to get these numbers. Do not estimate them.

### Step 3 — Flag quality problems
Look for issues worth knowing before anyone analyzes this data:
- columns with a high null rate — flag anything over 5%
- duplicate rows
- impossible values — negative counts, dates in the future, percentages over 100
- tables that should join to each other but have orphaned rows

### Step 4 — Write the inventory
Write a short report to `outputs/data_inventory.md` using the format below.

## Output Format

```markdown
# Data Inventory — {{DATA_SOURCE}}

## Summary
[2-3 sentences: what this data is, its overall quality, and what it can support.]

## Tables

### [table name]
- Rows: [count]
- Date range: [min] to [max]   (omit if the table has no date column)

| Column | Type | Null % | Distinct | Notes |
|--------|------|--------|----------|-------|
| [name] | [type] | [%] | [count] | [anything worth noting] |

## Quality flags
| Issue | Table | Column | Detail |
|-------|-------|--------|--------|
| [e.g. high null rate] | [table] | [column] | [e.g. 23% null] |
```

## Validation
Before presenting the inventory, double-check:
1. Row counts are real — re-run a `COUNT(*)` or `len(df)`, do not guess.
2. Null percentages are arithmetic-correct — `null_count / total_rows`.
3. Every table found in Step 1 appears in the report. Missing one is an error.
