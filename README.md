# E-Commerce User Behavior Analysis & Recommendation System

A graduate Data Science project analyzing user behavior from a large-scale e-commerce dataset. The project covers user segmentation, collaborative filtering, A/B testing, time-sensitive recommendations, customer journey analysis, and purchase timing prediction.

---

## Table of Contents

- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Development Workflow](#development-workflow)
- [Branching & Collaboration](#branching--collaboration)
- [Environment Setup](#environment-setup)

---

## Project Structure
> The exact notebook and script names are subject to change
```
ecommerce-behavior-analysis/
│
├── README.md                        
├── .gitignore                       
├── requirements.txt                 
├── .env.example                     
│
├── data/
│   ├── README.md                    # How to download the dataset
│   ├── raw/                         # Full raw data — NOT committed to git
│   ├── processed/                   # Cleaned outputs — NOT committed to git
│   └── samples/                     # Small sample CSVs for development — committed to git
│
├── notebooks/
│   ├── 00_data_exploration.ipynb
│   ├── 01_user_behavior_analysis.ipynb
│   ├── 02_product_affinity_analysis.ipynb
│   ├── 03_user_segmentation.ipynb
│   ├── 04_collaborative_filtering.ipynb
│   ├── 05_ab_testing_analysis.ipynb
│   ├── 06_time_sensitive_recommendations.ipynb
│   ├── 07_customer_journey_visualization.ipynb
│   └── 08_price_timing_predictor.ipynb
│
├── src/
│   ├── data/
│   │   ├── download.py              # Kaggle API download script
│   │   └── preprocess.py            # Cleaning & feature engineering pipeline
│   ├── models/
│   │   ├── collaborative_filter.py
│   │   ├── segmentation.py
│   │   └── price_predictor.py
│   ├── features/
│   │   ├── time_features.py         # Seasonal, time-of-day, day-of-week features
│   │   └── behavior_features.py     # Clickstream, cart abandonment features
│   └── visualization/
│       └── customer_journey.py      # Customer journey chart logic
│
├── tests/
│   ├── test_preprocess.py
│   ├── test_models.py
│   └── test_features.py
│
└── reports/
    ├── figures/                     # Saved plots and charts
    └── final_report.md
```

### What goes where

**`data/`** — Data lives here but is mostly gitignored. Only the `samples/` subfolder (a few thousand rows) is committed so teammates can develop without downloading the full dataset. See the [Dataset](#dataset) section for how to get the full data.

**`notebooks/`** — Exploration, analysis, and storytelling. Each notebook corresponds to one component of the project. Notebooks are where you figure things out and present results — they are not where reusable logic lives.

**`src/`** — All reusable Python code. Once logic is finalized in a notebook, it is extracted into a function or class here. Notebooks import from `src/`, keeping them clean and testable. The subfolders are:
- `data/` — downloading and preprocessing the raw dataset
- `models/` — model training and inference logic
- `features/` — feature engineering functions
- `visualization/` — chart and plot generation functions

**`tests/`** — Unit tests for functions in `src/`. Run with `pytest` before merging any branch.

**`reports/figures/`** — All plots saved from notebooks go here so they can be referenced in the final report.

---

## Dataset

**Source:** [E-Commerce Behavior Data from Multi-Category Store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store) (Kaggle)

This dataset is several gigabytes and is **not stored in this repository**. See `data/README.md` for full download instructions using the Kaggle API.

### Why the data is not in git

Committing large files to git causes the repository to become slow and bloated for everyone. Instead, we use the following approach:

1. **One person** downloads the full raw dataset and runs `src/data/preprocess.py` to produce a cleaned, compressed Parquet file in `data/processed/`
2. The processed file is shared with the team via Google Drive (link in the team group chat)
3. Everyone else downloads only the processed file — the raw data never needs to touch your machine
4. The `data/samples/` folder in this repo contains a small slice of the cleaned data for local development and testing

### Memory considerations

The raw data is read in chunks to avoid loading it all into memory at once:

```python
for chunk in pd.read_csv('data/raw/events.csv', chunksize=100_000):
    # process each chunk
```

The processed Parquet output is significantly smaller than the raw CSV and loads much faster.

---

## Development Workflow

### 1. Setting up your environment

```bash
git clone https://github.com/halleepham/ecommerce-behavior-analysis.git
cd ecommerce-behavior-analysis
pip install -r requirements.txt
```

Copy the environment variable template and fill in your credentials:

```bash
cp .env.example .env
```

> Kaggle API key setup instructions will be added here once finalized.

### 2. Working with data locally

For day-to-day development, use the sample data in `data/samples/`. Your code should work on the sample before you test it on the full processed dataset.

To load the sample:

```python
import pandas as pd
df = pd.read_csv('data/samples/sample_events.csv')
```

To load the full processed data (after downloading from Google Drive):

```python
import pandas as pd
df = pd.read_parquet('data/processed/events_clean.parquet')
```

### 3. Notebook → src workflow

When working on a feature:

1. Start in the relevant notebook under `notebooks/` — explore freely, try things, make plots
2. Once the logic is solid, move it into the appropriate file in `src/` as a clean function
3. Import and call that function back in the notebook to keep it clean
4. Write a basic test for the function in `tests/`

**Example:** You figure out how to engineer time-of-day features in notebook `06`. Once it works, the function moves to `src/features/time_features.py`, and the notebook simply calls `from src.features.time_features import get_time_features`.

### 4. Running tests

Before opening a pull request, always run:

```bash
pytest tests/
```

All tests must pass before merging.

---

## Branching & Collaboration

We use a feature branch workflow. Nobody commits directly to `main`.

### Branch naming

| Type | Convention | Example |
|---|---|---|
| New feature | `feature/short-description` | `feature/collaborative-filter` |
| Notebook work | `notebook/short-description` | `notebook/ab-testing-analysis` |

### Standard process

```
main                  ← always stable, always runs
  └── feature/your-feature
        ├── do your work here
        ├── run pytest
        └── open a pull request → teammate reviews → merge
```

1. Pull the latest `main` before starting any work: `git pull origin main`
2. Create your branch: `git checkout -b feature/your-feature`
3. Commit often with clear messages: `git commit -m "Add cart abandonment feature engineering"`
4. Push your branch and open a pull request on GitHub
5. At least one teammate must review and approve before merging
6. Delete the branch after merging

### Commit message format

Keep messages short and specific. Start with a verb:

- `Add collaborative filtering model skeleton`
- `Fix null values in price column preprocessing`
- `Update notebook 03 with final segmentation plots`

---

## Environment Setup

All dependencies are pinned in `requirements.txt`. Install with:

```bash
pip install -r requirements.txt
```

Key libraries used in this project:

| Library | Purpose |
|---|---|
| `pandas` | Data loading and manipulation |
| `numpy` | Numerical operations |
| `scikit-learn` | Segmentation, collaborative filtering, A/B testing |
| `matplotlib` / `seaborn` | Visualization |
| `pyarrow` | Parquet file format support |
| `kaggle` | Dataset download via API |
| `pytest` | Running unit tests |
| `jupyter` | Notebook environment |

> All computation in this project uses free tools only. No paid APIs or cloud compute services are required.

> We may use Colab instead of Jupyter