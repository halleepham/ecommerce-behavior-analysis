# Purchase Timing Recommendations

This file was produced by `notebooks/10_price_timing_predictor.ipynb` from
`product_price_history.parquet`. It assigns each product a current buy/wait
recommendation based on a trained gradient boosting model that predicts whether
the product's price is likely to drop ≥5% within the next 7 days.

Contact kylenaluan@gmail.com for access to the private Kaggle dataset, or attach it to your
Kaggle notebook before running.

---

## File

| File | Shape | Primary key |
|---|---|---|---|
| `purchase_timing_recommendations.parquet` | (~150k products × 5) | `product_id` |

---

## Schema

| Column | Description |
|---|---|
| `product_id` | Foreign key — matches `product_id` in the cleaned event data and `product_price_history` |
| `date` | The most recent date for this product in `product_price_history` — the observation used to generate the recommendation |
| `median_price` | The product's median price on `date` |
| `price_30d_pct` | Where today's price sits within the product's 30-day min–max range (0 = at 30-day low, 1 = at 30-day high) |
| `drop_probability` | Model-predicted probability that the price will drop ≥5% within the next 7 days |
| `recommendation` | One of three tiers based on `drop_probability` (see below) |

---

## Recommendation Tiers

| `recommendation` | `drop_probability` range | Interpretation |
|---|---|---|
| `Buy Now` | < 0.30 | Price is unlikely to drop soon — good time to purchase |
| `Monitor` | 0.30 – 0.50 | Moderate drop probability — worth watching before committing |
| `Wait` | ≥ 0.50 | High drop probability — consider waiting for a lower price |

Thresholds were chosen based on the precision-recall tradeoff of the model. For a
purchase timing tool, missing a real drop is more costly to the user than a false
alarm, so recall is prioritized over precision.

---

## How to load

```python
import pandas as pd

recs = pd.read_parquet(
    '/kaggle/input/datasets/kylenaluan/purchase-timing-recommendations/purchase_timing_recommendations.parquet'
)
print(recs.shape)

# Join with user cart or wishlist data
# e.g., flag products in a user's cart that have a 'Wait' recommendation
cart_with_timing = cart_df.merge(recs[['product_id', 'recommendation', 'drop_probability']],
                                  on='product_id', how='left')
```