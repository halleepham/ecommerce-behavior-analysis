# Data

This folder contains all data for the project. Read this before touching anything in this folder.

---

## Two tiers of data

This project uses two tiers of data. Understanding the difference is important 
before you write any code.

**Tier 1 — Sample data (in this repo)**
A session-based sample of the **processed data** committed directly to git so every 
teammate can develop and test code immediately after cloning the repo without 
downloading anything. Use this tier for all day to day development (writing functions, running tests, and making sure your code works).

**Tier 2 — Full dataset (Kaggle)**
The complete raw dataset lives permanently on Kaggle. The full **processed** dataset 
is saved as a Kaggle dataset that any teammate can attach to their Kaggle notebook 
with one click. Use this tier for final analysis, model training, and generating 
results for the report.

---

## Why session-based sampling

Random row sampling would break user sessions. If a user has 10 events in a 
session and only 3 are sampled, the sequence is destroyed. Components like 
collaborative filtering, customer journey analysis, and cart abandonment all 
depend on seeing complete sequences of user behavior. Session-based sampling 
solves this by keeping all rows that belong to a sampled session fully intact.

---

## Folder structure
```
data/
├── README.md                    ← you are here
├── raw/
│   └── README.md                ← points to full dataset on Kaggle
└── processed/
    ├── README.md                ← what changed during cleaning and why
    └── events_clean.csv         ← session-based sample, use this for development
```

---

## How to load the sample data

For all development and testing, load the processed sample:
```python
import pandas as pd
df = pd.read_csv('data/processed/events_clean.csv', parse_dates=['event_time'])
```

---

## How to access the full dataset

**Full raw dataset:**
[E-Commerce Behavior Data from Multi-Category Store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store)

**Full processed dataset:**
Available as a Kaggle dataset — link in the team group chat. Attach it to your 
Kaggle notebook with one click. It will be available at:

    /kaggle/input/ecommerce-processed/events_clean.parquet

> Note: the Kaggle processed dataset will be created after the preprocessing 
notebook is complete. Check the team group chat for the link.

---

## Switching between sample and full data

Every script that loads data uses a single path variable at the top. To switch 
between the sample and the full dataset, change only that one line:
```python
# Development — sample data in the repo
DATA_PATH = 'data/processed/events_clean.csv'

# Final runs — full processed dataset on Kaggle
# DATA_PATH = '/kaggle/input/ecommerce-processed/events_clean.parquet'
```

Never hardcode a data path anywhere else in your code. Always use DATA_PATH.