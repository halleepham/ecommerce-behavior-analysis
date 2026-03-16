# E-Commerce User Behavior Analysis & Recommendation System

A graduate Data Science project analyzing user behavior from a large-scale e-commerce dataset. The project covers user segmentation, collaborative filtering, A/B testing, time-sensitive recommendations, customer journey analysis, and purchase timing prediction.

---

## Table of Contents

- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Setup](#setup)
- [Development Workflow](#development-workflow)
- [Branching & Collaboration](#branching--collaboration)
- [Tools & Technologies](#tools--technologies)

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
│   ├── README.md                           # Overall data strategy
│   ├── raw/
│   │   └── README.md                       # Points to full dataset on Kaggle
│   └── processed/
│       ├── README.md                       # What changed and why
│       └── events_clean.csv                # Session-based sample — use this for development
│
├── notebooks/
│   ├── 00_data_exploration.ipynb           # Quick sample-based exploration of schema and data types
│   ├── 01_full_dataset_exploration.ipynb   # Full exploration across all 110 million rows
│   ├── 02_preprocessing.ipynb              # Sampling and cleaning pipeline walkthrough
│   ├── 03_user_behavior_analysis.ipynb
│   ├── 04_user_segmentation.ipynb
│   ├── 05_collaborative_filtering.ipynb
│   ├── 06_ab_testing_analysis.ipynb
│   ├── 07_time_sensitive_recommendations.ipynb
│   ├── 08_customer_journey_visualization.ipynb
│   └── 09_price_timing_predictor.ipynb
│
├── src/
│   ├── data/
│   │   └── preprocess.py                # Sampling and cleaning functions
│   ├── models/
│   │   ├── collaborative_filter.py
│   │   ├── segmentation.py
│   │   └── price_predictor.py
│   ├── features/
│   │   ├── time_features.py
│   │   └── behavior_features.py
│   └── visualization/
│       └── customer_journey.py
│
├── tests/
│   ├── test_preprocess.py
│   ├── test_models.py
│   └── test_features.py
│
└── reports/
    ├── figures/
    └── final_report.md
```

---

## Dataset

The project works with a session-based sample of a large e-commerce event dataset from Kaggle. The full dataset is never stored in this repository.

**Every teammate can start working immediately after cloning the repo** — once the preprocessing notebook is complete, `data/processed/events_clean.csv` will be committed to git and require no additional downloads.

> **Note:** `data/processed/events_clean.csv` does not exist yet. It will be committed after `notebooks/02_preprocessing.ipynb` is complete.

If you need the full dataset for any reason, see `data/README.md` for instructions.

**Full dataset source:** [E-Commerce Behavior Data from Multi-Category Store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store)

---

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/halleepham/ecommerce-behavior-analysis.git
cd ecommerce-behavior-analysis
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Verify your setup

> Note: this step requires `data/processed/events_clean.csv` to exist. See the Dataset section above.
```python
import pandas as pd
df = pd.read_csv('data/processed/events_clean.csv', parse_dates=['event_time'])
print(df.shape)
print(df.head())
```

---

## Development Workflow

### Working with data

For all development, load the processed sample:
```python
import pandas as pd
df = pd.read_csv('data/processed/events_clean.csv', parse_dates=['event_time'])
```

For final analysis and model training, use the full processed dataset on Kaggle:
```python
import pandas as pd
df = pd.read_parquet('/kaggle/input/ecommerce-processed/events_clean.parquet')
```

### Notebook to src workflow

1. Explore and develop logic freely in the relevant notebook
2. Once the logic works, move it into the appropriate file in `src/` as a clean function
3. Import that function back into the notebook
4. Write a test for the function in `tests/`

### Running tests
```bash
pytest tests/
```

All tests must pass before opening a pull request.

---

## Branching & Collaboration

Nobody commits directly to `main`. We use a feature branch workflow.

### Branch naming

| Type | Convention | Example |
|---|---|---|
| New feature | `feature/short-description` | `feature/collaborative-filter` |
| Notebook work | `notebook/short-description` | `notebook/data-exploration` |
| Bug fix | `fix/short-description` | `fix/null-price-handling` |

### Standard process

1. Pull latest main: `git pull origin main`
2. Create your branch: `git checkout -b feature/your-feature`
3. Do your work and commit often with clear messages
4. Run tests: `pytest tests/`
5. Push and open a pull request on GitHub
6. At least one teammate reviews and approves before merging
7. Delete the branch after merging

### Good commit messages
```
Add cart abandonment feature engineering
Fix null values in price column preprocessing
Update notebook 03 with final segmentation plots
Extract collaborative filter logic into src/models/
```

---

## Tools & Technologies

All tools used in this project are free.

| Tool | Purpose |
|---|---|
| `pandas` / `numpy` | Data loading, cleaning, and manipulation |
| `scikit-learn` | Clustering, collaborative filtering, A/B testing |
| `matplotlib` / `seaborn` | Charts, plots, and visualizations |
| `scipy` | Statistical tests for A/B testing |
| `pyarrow` | Parquet file format support |
| `pytest` | Automated testing |
| `python-dotenv` | Loading credentials from `.env` |
| Kaggle Notebooks | Cloud notebook environment for data processing |
| Google Colab | Alternative cloud notebook environment |
| GitHub | Version control and code review |
| Google Drive | Sharing large processed files if needed |