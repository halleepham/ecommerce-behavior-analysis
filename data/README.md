# Data

This folder contains all datasets used and produced by the project.

---

## Structure

```
data/
├── raw/                                  # See README inside — raw data comes from Kaggle directly
│   └── README.md
├── processed/
│   ├── ecommerce_oct_nov_clean_sample.parquet
│   ├── 04_feature_engineering_output/
│   │   ├── product_price_history.parquet
│   │   ├── session_features_sample.parquet
│   │   ├── user_category_affinity.parquet
│   │   └── user_segments.parquet
│   ├── 05_user_segmentation_output/
│   │   └── user_segments.parquet
│   ├── 06_collaborative_filtering_output/
│   │   ├── loo_test.parquet
│   │   └── recommendations.parquet
│   ├── 08_time_sensitive_recommendations_output/
│   │   └── temporal_recommendations_sample.parquet
│   └── 10_price_timing_predictor_output/
│       └── purchase_timing_recommendations.parquet
└── metadata/
    ├── processed_data_metadata.md        # Cleaned event data — schema and cleaning decisions
    ├── feature_engineering_metadata.md   # Outputs from notebook 04
    ├── user_segments_metadata.md         # Outputs from notebook 05
    ├── purchase_timing_metadata.md       # Outputs from notebook 10
    └── ...                               # Other notebook output metadata files
```

---

## Finding what you need

**Cleaned event data** — `processed/ecommerce_oct_nov_clean_sample.parquet`
is a 500k-row stratified sample of the full cleaned dataset, suitable for local
development. See `metadata/processed_data_metadata.md` for full schema and
cleaning decisions.

**Feature tables, segmentation results, and model outputs** — each notebook's
output files live in a subfolder named after that notebook under `processed/`
(e.g. `04_feature_engineering_output/`). See the corresponding metadata file in
`metadata/` for column descriptions and load instructions.

**Raw data** — see `raw/README.md`. Raw data is sourced directly from the
original Kaggle dataset and is not stored in this repository.

---

## Accessing full datasets

Most data files in this repository can be uploaded directly to a Kaggle notebook,
or accessed by requesting Kaggle dataset access from kylenaluan@gmail.com.

If a file is labeled `_sample`, the full dataset can be obtained in one of two ways:
- Request Kaggle access (see above), then attach the dataset to your notebook
- Re-run the notebook that produced it — the full output will be written to `/kaggle/working`