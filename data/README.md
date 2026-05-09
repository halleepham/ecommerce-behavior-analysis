# Data

This folder contains all datasets used and produced by the project.

---

## Structure

```
data/
├── raw/                                    # See README inside — raw data comes from Kaggle directly
└── processed/
    ├── ecommerce_oct_nov_clean_sample.parquet    # 500k-row sample of the cleaned event log
    ├── README.md                                 # Full details on the cleaned dataset
    ├── 04_feature_engineering_output/            # Outputs from notebooks/04_feature_engineering.ipynb
    │   └── README.md
    ├── 05_user_segmentation_output/              # Outputs from notebooks/05_user_segmentation.ipynb
    │   └── README.md
    ├── 06_collaborative_filtering_output/        # Outputs from notebooks/06_collaborative_filtering.ipynb
    │   └── README.md
    ├── 08_time_sensitive_recommendations_output/ # Outputs from notebooks/08_time...recommendations.ipynb
    │   └── README.md
    └── 10_price_timing_predictor_output/         # Outputs from notebooks/10_price_timing_predictor.ipynb
        └── README.md
```

---

## Finding what you need

**Cleaned event data** — `processed/ecommerce_oct_nov_clean_sample.parquet`
is a 500k-row stratified sample of the full cleaned dataset, suitable for local
development. See `processed/README.md` for full schema and cleaning decisions.

**Feature tables, segmentation results, and model outputs** — each notebook's
output files live in a subfolder named after that notebook (e.g.
`04_feature_engineering_output/`). See the README inside each subfolder for
column descriptions and load instructions.

**Raw data** — see `raw/README.md`. Raw data is sourced directly from the
original Kaggle dataset and is not stored in this repository.

---

## Accessing full datasets

Most data files in this repository can be uploaded directly to a Kaggle notebook,
or accessed by requesting Kaggle dataset access from kylenaluan@gmail.com.

If a file is labeled `_sample`, the full dataset can be obtained in one of two ways:
- Request Kaggle access (see above), then attach the dataset to your notebook
- Re-run the notebook that produced it — the full output will be written to `/kaggle/working`