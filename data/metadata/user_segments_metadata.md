# User Segments

This file was produced by `notebooks/05_user_segmentation.ipynb` from `user_features.parquet`.
It assigns every user a behavioral cluster label for use in A/B testing (07) and any other
notebook that needs to stratify analysis by user type.

Contact kylenaluan@gmail.com for access to the private Kaggle dataset, or attach it to your
Kaggle notebook before running.

---

## File

| File | Shape | Primary key | Used in |
|---|---|---|---|
| `user_segments.parquet` | (5,316,649 × 2) | `user_id` (index) | 07 |

> **Note:** `user_id` is stored as the **index**

---

## Schema

| Column | Description |
|---|---|
| `cluster_id` | Integer cluster assignment (0–3) from K-Means |
| `cluster_label` | Human-readable label for the cluster (see below) |

---

## Segments

K-Means was run with k=4, selected via elbow and silhouette analysis. Labels were assigned
by inspecting per-cluster median feature values. The four segments are:

| `cluster_id` | `cluster_label` | Size | Characteristics |
|---|---|---|---|
| 0 | Passive Browsers | 3,291,223 | Low event counts across all features, high `recency_days`, zero purchases, `cart_abandonment_rate` = 1.0 |
| 1 | One Time Buyers | 170,343 | Low views (~4), (~1) purchase, non-zero `total_spend` (~$283), high `recency_days` |
| 2 | Active Browsers | 1,359,678 | High views (~24), low purchases, high `cart_abandonment_rate`, low `recency_days` (~9 days) |
| 3 | Engaged Buyers | 495,405 | High views, non-zero purchases, non-zero `total_spend`, low `recency_days` |

Passive Browsers are the majority at ~62% of all users, consistent with the finding in
`user_features` that the median user has 2 views and 0 purchases.

---

## How to load

```python
import pandas as pd

user_segments = pd.read_parquet(
    '/kaggle/input/datasets/kylenaluan/user-segments/user_segments.parquet'
)
# user_id is the index
print(user_segments.shape)  # (5316649, 2)

# Join back to user_features if needed
user_features = pd.read_parquet('.../user_features.parquet')
merged = user_features.join(user_segments)
```