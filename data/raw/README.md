# Raw Data

This folder contains no data files. The raw dataset lives permanently on Kaggle 
and does not need to be downloaded for normal development.

---

## Where to get the raw data

**Kaggle:** [E-Commerce Behavior Data from Multi-Category Store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store)

You will need a free Kaggle account to access the files.

| File | Size | Description |
|---|---|---|
| `2019-Oct.csv` | 5.67 GB | October 2019 events |
| `2019-Nov.csv` | 9.01 GB | November 2019 events |

---

## Why the raw data is not in this repo

The raw files are 5.67 GB and 9.01 GB respectively — far too large to commit to 
git. Storing them here would make the repository unusable for everyone.

All work that requires the full raw dataset is done in Kaggle notebooks, where 
the data is already available without any download. See the main `data/README.md` 
for the full data strategy.

---

## Raw data structure

Each file is a CSV with the following columns:

| Column | Type | Description |
|---|---|---|
| `event_time` | text | When the action happened (UTC) |
| `event_type` | text | What the user did — `view`, `cart`, or `purchase` |
| `product_id` | integer | Which product was involved |
| `category_id` | integer | Numeric category identifier |
| `category_code` | text | Human readable category e.g. `electronics.smartphone` |
| `brand` | text | Product brand |
| `price` | float | Product price in dollars |
| `user_id` | integer | Which user performed the action |
| `user_session` | text | Unique session identifier |

---

## Known data quality issues

These issues were identified during full dataset exploration in 
`notebooks/01_full_dataset_exploration.ipynb`. They are all addressed in the 
preprocessing pipeline documented in `notebooks/02_preprocessing.ipynb`.

| Issue | Column | October | November |
|---|---|---|---|
| Missing values | `category_code` | 13,515,609 rows (31.8%) | 21,898,171 rows (32.4%) |
| Missing values | `brand` | 6,117,080 rows (14.4%) | 9,224,078 rows (13.7%) |
| Null session IDs | `user_session` | 2 rows | 10 rows |
| Wrong data type | `event_time` | Stored as text, not as a real datetime | Same |
| Duplicate rows | all columns | 30,219 rows (0.071%) | 100,510 rows (0.149%) |
| Zero prices | `price` | 68,673 rows (0.162%) | 188,088 rows (0.279%) |