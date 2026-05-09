# Time-Sensitive Recommendation Outputs

All files in this folder were produced by `notebooks/08_time_sensitive_recommendations.ipynb` from the collaborative filtering outputs (notebook 06), session features (notebook 04), and user-category affinity data (notebook 04). Do not recompute these in individual notebooks — load the parquet files directly.

Each file is saved to a private Kaggle dataset. Contact hdph78@umsystem.com for access, or attach the relevant dataset to your Kaggle notebook before running.

---

## Files

| File | Shape | Primary key | Used in |
|---|---|---|---|
| `temporal_recommendations_sample.parquet` | (5,499,994 × 7) | (`user_id`, `temporal_rank`) | — |
> **Note:** This is a random sample of ~1.1M users (50% of the full dataset). The full file (11,138,954 × 7) is available on Kaggle at halleepham/time-sensitive-recommendations-output.
---

## temporal_recommendations.parquet

One row per user per recommendation. Extends the baseline collaborative filtering recommendations from notebook 06 with temporal re-ranking. Each recommendation's predicted score is multiplied by a combined temporal lift factor — computed from category-level hour-of-day, day-of-week, and seasonal conversion patterns — to produce a temporal score. Recommendations are then re-ranked by temporal score within each user.

| Column | Description |
|---|---|
| `user_id` | Foreign key to `user_category_affinity` |
| `recommended_category` | Top-level product category recommended for this user |
| `predicted_score` | Original predicted score from notebook 06 collaborative filtering model |
| `combined_lift` | Product of the category × hour lift, category × day lift, and seasonal lift factors for this user's temporal context. Based on the user's most recent session time. |
| `temporal_score` | `predicted_score × combined_lift`. The temporally adjusted score used for re-ranking. |
| `rank` | Original rank from notebook 06 (1 = highest predicted score) |
| `temporal_rank` | Re-ranked position after applying temporal lift factors (1 = highest temporal score) |

**How to load:**

```python
temporal_recommendations = pd.read_parquet(
    '/kaggle/input/datasets/halleepham/time-sensitive-outputs/temporal_recommendations.parquet'
)
print(temporal_recommendations.shape)  # (11138954, 7)
```

---

## How this file was built

Category-level conversion lift factors were computed for hour of day and day of week using purchase history from `user_category_affinity.parquet` joined with converted sessions from `session_features.parquet`. Seasonal lift factors were computed at the overall date level from daily conversion rates in November (excluding November 15 due to a confirmed data collection failure) and a single aggregate lift factor for October.

Shrinkage weighting (factor=5,000) was applied to smooth lift factors for sparse categories toward 1.0, consistent with the shrinkage approach used in notebook 06.

The three lift factors were combined by multiplication and applied to each user's baseline recommendations from notebook 06, using the user's most recent session as their temporal context. Recommendations were re-ranked by the resulting temporal score within each user.

See `notebooks/08_time_sensitive_recommendations.ipynb` for the full derivation logic.