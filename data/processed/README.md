# Processed Data

This folder contains the cleaned, combined version of the raw samples. This is the 
file your code should load for all development, analysis, and model building.

---

## Files

| File | Source | Rows | Description |
|---|---|---|---|
| `events_clean.csv` | `data/raw/oct_sample.csv` + `data/raw/nov_sample.csv` | TBD | Combined and cleaned sample |

---

## What changed from raw to processed

Every change made during cleaning is documented and justified here. Nothing was 
changed without a reason.

| Issue | Column | What we did | Why |
|---|---|---|---|
| Text timestamps | `event_time` | Converted to datetime with UTC timezone | Required for any time-based analysis |
| Missing category | `category_code` | Filled with `'unknown'` | Rows are still valid events, deleting them would bias the data |
| Missing brand | `brand` | Filled with `'unknown'` | Same reason as above |
| Duplicate rows | all columns | Removed exact duplicates | Same user cannot perform the same action on the same product at the exact same millisecond |
| Zero prices | `price` | Replaced with `NaN` | A price of zero is not a real price — we don't know the true value so we don't pretend we do |
| Mixed case text | `event_type`, `brand`, `category_code` | Lowercased and stripped whitespace | Prevents `'View'` and `'view'` from being treated as different values |

---

## New columns added during preprocessing

These columns do not exist in the raw data. They were derived from existing columns 
to make downstream analysis easier.

| Column | Description | Derived from |
|---|---|---|
| `event_date` | Date only, no time | `event_time` |
| `hour_of_day` | 0–23 | `event_time` |
| `day_of_week` | 0 (Monday) – 6 (Sunday) | `event_time` |
| `day_name` | 'Monday', 'Tuesday', etc. | `event_time` |
| `month` | 1–12 | `event_time` |
| `is_weekend` | True if Saturday or Sunday | `event_time` |
| `time_of_day_bin` | 'night', 'morning', 'afternoon', 'evening' | `hour_of_day` |
| `category_top` | Top level category e.g. 'electronics' from 'electronics.smartphone' | `category_code` |

---

## How to load this file
```python
import pandas as pd

df = pd.read_csv('data/processed/events_clean.csv', parse_dates=['event_time'])
```

---

## How the processed file was created

The cleaning and combining logic lives in `src/data/preprocess.py`. The process is 
documented step by step in `notebooks/01_preprocessing.ipynb`. To recreate this 
file from scratch:

1. Ensure `data/raw/oct_sample.csv` and `data/raw/nov_sample.csv` exist
2. Run:
```
python src/data/preprocess.py
```