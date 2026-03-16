# Processed Data

This folder contains the cleaned session-based sample of the full dataset. 
This is the file your code should load for all development, analysis, and testing.


> **Note:** This README will be completed after `notebooks/02_preprocessing.ipynb` is finished. The cleaning decisions, new columns, and file statistics will all be filled in at that point.

---

## Files

| File | Source | Sessions | Rows | Description |
|---|---|---|---|---|
| `events_clean.csv` | `2019-Oct.csv` + `2019-Nov.csv` | TBD | TBD | Combined and cleaned |

These values will be filled in after the preprocessing notebook is run.

---

## What changed from raw to processed

Every change made during cleaning is documented and justified here. Nothing was changed without a reason.

| Issue | Column | What we did | Why |
TBD

---

## New columns added during preprocessing

These columns do not exist in the raw data. They were derived from existing columns 
to make downstream analysis easier. Every component of the project uses at least some of these.

| Column | Description | Derived from |
|---|---|---|
TBD

---

## What this file is NOT for

This file is the cleaned base dataset. It is not shaped or aggregated for any 
specific project component. Transformations like building a user-item matrix for 
collaborative filtering, or computing per-user behavioral summaries for 
segmentation, happen at runtime via functions in `src/features/`. 

Additional files will only be added to this folder if a specific transformation 
is too slow to recompute each time.

---

## How to load this file
```python
import pandas as pd

df = pd.read_csv('data/processed/events_clean.csv', parse_dates=['event_time'])
```

---

## How the processed file was created

TBD

---

## Full processed dataset

The full processed dataset (not the sample) is available as a Kaggle dataset. 
Get the link from the team group chat. Attach it to your Kaggle notebook with 
one click — it will be available at:

    TBD