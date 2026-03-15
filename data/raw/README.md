# Raw Data

This folder contains random samples of the original unprocessed data exactly as it 
comes from Kaggle. Nothing has been cleaned or modified — these files reflect the 
true state of the source data.

---

## Files

| File | Source | Rows | Random Seed |
|---|---|---|---|
| `oct_sample.csv` | `2019-Oct.csv` | TBD  | 42 |
| `nov_sample.csv` | `2019-Nov.csv` | TBD  | 42 |

---

## Why only samples

The full raw files are 5.67 GB and 9.01 GB respectively. Files that large cannot 
be committed to git — they would make the repository unusable for everyone. The 
full files live on Kaggle's servers permanently and can be accessed by anyone with 
a free Kaggle account.

---

## Why random sampling instead of the first N rows

Taking the first N rows would only capture events from the very beginning of each 
month, which over-represents certain time-based behavior patterns. Random sampling 
gives a more representative slice of the full dataset. The fixed random seed (42) 
ensures the sample is identical every time it is recreated — anyone running the 
same code gets the exact same rows.

---

## How the samples were taken

Sampling was done before any cleaning so these files reflect the raw source data 
accurately. The logic lives in `src/data/preprocess.py`. The process is documented 
step by step in `notebooks/01_preprocessing.ipynb`.

In plain terms, the sampling works like this:
1. Load the full CSV from Kaggle in chunks to avoid memory issues
2. Randomly select the target number of rows using `random_state=42`
3. Save the result as a CSV without any modifications

---

## Known issues in this data

These are the data quality issues we found during exploration 
(`notebooks/00_data_exploration.ipynb`). They are intentionally left as-is here 
because this is the raw data. They are all fixed in `data/processed/events_clean.csv`.

| Issue | Column | Details |
|---|---|---|
| Missing values | `category_code` | ~32% of rows have no category code |
| Missing values | `brand` | ~14% of rows have no brand |
| Wrong data type | `event_time` | Stored as text, not as a datetime |
| Duplicate rows | all columns | Small number of exact duplicate rows exist |
| Zero prices | `price` | Some rows have a price of 0.00 |

---

## Full dataset access

**Kaggle:** [E-Commerce Behavior Data from Multi-Category Store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store)

You will need a free Kaggle account to access the full files.