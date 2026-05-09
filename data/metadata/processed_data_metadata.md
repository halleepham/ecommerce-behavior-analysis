# Processed Data

This folder contains the cleaned, combined dataset produced by `notebooks/02_preprocessing.ipynb`.
This is the file your code should load for all development, analysis, and testing.

---

## Files

| File | Source | Rows | Memory | Description |
|---|---|---|---|---|
| `ecommerce_oct_nov_clean.parquet` | `2019-Oct.csv` + `2019-Nov.csv` | 109,820,004 | 6.1 GB | Combined and cleaned event log |
| `ecommerce_oct_nov_clean_sample.parquet` | `ecommerce_oct_nov_clean.parquet` | 500,000 | 28.89 MB | Stratified sample for local development |

---

## What changed from raw to processed

Every change made during cleaning is documented and justified here. Nothing was changed without a reason.

| Issue | Column | What we did | Why |
|---|---|---|---|
| String timestamps | `event_time` | Converted to `datetime64[ns, UTC]` | Enables time-based filtering, groupby, and arithmetic |
| Missing values | `category_code` | Filled with `'unknown'` | `groupby` and string operations handle `'unknown'` more predictably than `NaN` |
| Missing values | `brand` | Filled with `'unknown'` | Same reason as above |
| Zero prices | `price` | Replaced `0` with `NaN` | A price of exactly zero is not a valid product price; keeping it would skew price statistics |
| Duplicate rows | all columns | Removed exact duplicates within each chunk, then again after combining | Confirmed present in notebook 01 (30k in Oct, 100k in Nov) |
| Oversized numeric types | `product_id`, `user_id` | Downcast to `int32` | Reduces memory footprint without losing precision |
| Oversized float type | `price` | Downcast to `float32` | Reduces memory footprint; price precision does not require float64 |
| String category columns | `event_type`, `brand` | Cast to `category` dtype | Reduces memory for low-cardinality string columns |

---

## New columns added during preprocessing

No new columns were added. The processed file has the same 9 columns as the raw data — only dtypes and values were changed. Derived columns (session features, user features, etc.) are created in `notebooks/04_feature_engineering.ipynb`.

---

## Schema

| Column | Dtype | Description |
|---|---|---|
| `event_time` | `datetime64[ns, UTC]` | Timestamp of the event |
| `event_type` | `category` | One of: `view`, `cart`, `purchase` |
| `product_id` | `int32` | Product identifier |
| `category_id` | `int64` | Category identifier |
| `category_code` | `object` | Dot-separated category path (e.g. `electronics.smartphone`), or `'unknown'` |
| `brand` | `category` | Brand name, or `'unknown'` |
| `price` | `float32` | Product price at time of event, or `NaN` if zero |
| `user_id` | `int32` | User identifier |
| `user_session` | `object` | Session UUID |

---

## What this file is NOT for

This is the cleaned base dataset. It is not shaped or aggregated for any specific project component.
Transformations like building a user-item matrix for collaborative filtering, or computing per-user
behavioral summaries for segmentation, happen in `notebooks/04_feature_engineering.ipynb`.

Additional files will only be added to this folder if a specific transformation is too slow to recompute each time.

---

## How to load this file

```python
import pandas as pd

# Full dataset (Kaggle only — attach the dataset to your notebook first)
df = pd.read_parquet('/kaggle/input/datasets/kylenaluan/ecommerce-data-from-oct-and-nov-cleaned/ecommerce_oct_nov_clean.parquet')

# Sample (local development)
df = pd.read_parquet('data/processed/events_sample.parquet')
```

To load only the columns you need (recommended for memory):

```python
df = pd.read_parquet(
    '/kaggle/input/.../ecommerce_oct_nov_clean.parquet',
    columns=['event_time', 'event_type', 'user_id', 'price']
)
```

---

## How the processed file was created

Both raw CSVs were processed in chunks of 500,000 rows to stay within Kaggle's 30 GB memory limit.
Each chunk was cleaned individually, then the two months were appended into a single parquet file
using `fastparquet` (chosen over `pyarrow` because it supports appending without loading the full file).
See `notebooks/02_preprocessing.ipynb` for the full pipeline.

---

## Full processed dataset

The full processed dataset is available as a private Kaggle dataset. Contact kylenaluan@gmail.com for access. Once you have access, attach it to your Kaggle notebook with one click — it will be available at:

    /kaggle/input/datasets/kylenaluan/ecommerce-data-from-oct-and-nov-cleaned/ecommerce_oct_nov_clean.parquet