# Feature Tables

All four files in this folder were produced by `notebooks/04_feature_engineering.ipynb`
from the cleaned event log (`ecommerce_oct_nov_clean.parquet`). They are the shared
inputs for all downstream modeling and analysis notebooks (05–10). Do not recompute
these in individual notebooks — load the parquet files directly.

Each table is saved to a private Kaggle dataset. Contact kylenaluan@gmail.com for access,
or attach the relevant dataset to your Kaggle notebook before running.

---

## Files

| File | Shape | Primary key | Used in |
|---|---|---|---|
| `user_features.parquet` | (5,316,649 × 11) | `user_id` (index) | 05 |
| `session_features.parquet` | (23,016,650 × 16) | `user_session` | 07, 08 |
| `user_category_affinity.parquet` | (6,753,416 × 6) | (`user_id`, `main_category`) | 06, 08 |
| `product_price_history.parquet` | (4,998,112 × 7) | (`product_id`, `date`) | 10 |

---

## user_features.parquet

One row per user. Captures behavioral profile across both months — RFM metrics,
engagement rates, and preferences. Primary input for user segmentation (05).

> **Note:** `user_id` is stored as the **index**, not a regular column.
> Load with `df = pd.read_parquet(...)` and access via `df.index`.

| Column | Description |
|---|---|
| `n_views` | Total view events across both months |
| `n_carts` | Total cart addition events |
| `n_purchases` | Total purchase events |
| `recency_days` | Days between the user's last event and November 30, 2019. Lower = more recently active. |
| `total_spend` | Sum of all purchase prices. 0 for users who never purchased. |
| `active_days` | Number of distinct calendar days with at least one event |
| `preferred_category` | Top-level category the user viewed most. `'unknown'` if they only viewed products with unknown category; `NaN` if they had no view events. |
| `preferred_brand` | Brand the user viewed most (excluding `'unknown'`). `NaN` if the user only viewed unknown-brand products or had no view events. |
| `conversion_rate` | `n_purchases / n_views`. Can exceed 1 if a user purchases without a preceding view event in the same session. 0 for users with no views. |
| `cart_abandonment_rate` | Fraction of cart additions not followed by a purchase. 0 = every cart was purchased; 1 = no carts were purchased (or user never added to cart). |
| `avg_spend_per_purchase` | `total_spend / n_purchases`. 0 for users who never purchased. |

**How to load:**

```python
import pandas as pd

user_features = pd.read_parquet(
    '/kaggle/input/datasets/kylenaluan/user-features/user_features.parquet'
)
# user_id is the index
print(user_features.shape)  # (5316649, 11)
```

---

## session_features.parquet

One row per session. Captures what happened within a single user session — events,
timing, revenue, and outcome flags. Input for A/B testing (07) and time sensitive recommendations (08).

> **This repository contains `session_features_sample.parquet` (100,000 rows), sampled
> stratified by the `converted` flag. The full dataset (23,016,650 × 16) is available
> on Kaggle — attach it to your notebook for production use.

| Column | Description |
|---|---|
| `user_session` | Unique session identifier (UUID). Primary key. |
| `user_id` | The user who generated this session |
| `n_events` | Total events in the session across all event types |
| `n_views` | View events in the session |
| `n_carts` | Cart addition events in the session |
| `n_purchases` | Purchase events in the session |
| `unique_products` | Distinct products interacted with during the session |
| `session_start` | Timestamp of the first event |
| `session_end` | Timestamp of the last event |
| `session_revenue` | Sum of purchase prices in the session. 0 for sessions with no purchases. |
| `duration_seconds` | Seconds between `session_start` and `session_end`. 0 for single-event sessions. |
| `converted` | `True` if the session contains at least one purchase |
| `cart_abandoned` | `True` if the session has at least one cart addition but no purchase |
| `hour_of_day` | Hour (0–23 UTC) when the session started |
| `day_of_week` | Day of the week the session started, e.g. `Monday` |
| `month` | Month number: `10` for October, `11` for November |

**How to load:**

```python
session_features = pd.read_parquet(
    '/kaggle/input/datasets/kylenaluan/session-features/session_features.parquet'
)
print(session_features.shape)  # (23016650, 16)
```

---

## user_category_affinity.parquet

One row per (user, category) pair. Captures how strongly each user is associated with
each product category based on their interaction history. Primary input for collaborative
filtering (06) and time-sensitive recommendations (08).

Rows with `main_category = 'unknown'` are excluded — only known categories are present.

| Column | Description |
|---|---|
| `user_id` | Foreign key to `user_features` |
| `main_category` | Top-level product category (e.g. `electronics`, `appliances`, `furniture`). Never `'unknown'`. |
| `cat_views` | View events this user had on products in this category |
| `cat_carts` | Cart addition events in this category |
| `cat_purchases` | Purchase events in this category |
| `affinity_score` | `(cat_views × 1) + (cat_carts × 3) + (cat_purchases × 5)`. Weights reflect the relative intent behind each action type. Higher = stronger interest. |

**How to load:**

```python
user_category_affinity = pd.read_parquet(
    '/kaggle/input/datasets/kylenaluan/user-category-affinity/user_category_affinity.parquet'
)
print(user_category_affinity.shape)  # (6753416, 6)
```

---

## product_price_history.parquet

One row per (product, date). Tracks median daily price and 30-day rolling statistics
for each product. Primary input for the purchase timing predictor (10).

| Column | Description |
|---|---|
| `product_id` | Foreign key — matches `product_id` in the cleaned event data |
| `date` | Calendar date (midnight UTC) for this price observation |
| `median_price` | Median of all observed prices for this product on this date. Median is used to reduce the influence of outlier prices. |
| `price_30d_avg` | Rolling 30-day mean of `median_price`. Represents typical price over the past month. |
| `price_30d_min` | Rolling 30-day minimum of `median_price`. Lowest price in the past 30 days. |
| `price_30d_max` | Rolling 30-day maximum of `median_price`. Highest price in the past 30 days. |
| `price_30d_pct` | Position of today's `median_price` within the 30-day min–max range, as a value between 0 and 1. 0 = at the 30-day low; 1 = at the 30-day high. Products with a stable price (zero range) receive **0.5**. Primary buy-signal feature in notebook 10. |

**How to load:**

```python
product_price_history = pd.read_parquet(
    '/kaggle/input/datasets/kylenaluan/product-price-history/product_price_history.parquet'
)
print(product_price_history.shape)  # (4998112, 7)
```

---

## How these tables were built

Each table is computed in a separate pass over `ecommerce_oct_nov_clean.parquet`,
loading only the columns needed for that table. This avoids holding the full 6.1 GB
dataset in memory while building features. After each table is saved to parquet, it
is deleted from memory before the next table is built.

See `notebooks/04_feature_engineering.ipynb` for the full derivation logic.