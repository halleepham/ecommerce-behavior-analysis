# Data

This folder contains all data for the project. Read this before touching anything in this folder.

---

## What is in this folder
```
data/
├── raw/
│   ├── README.md
│   ├── oct_sample.csv       ← random sample from 2019-Oct.csv, committed to git
│   └── nov_sample.csv       ← random sample from 2019-Nov.csv, committed to git
└── processed/
    ├── README.md
    └── events_clean.csv     ← combined and cleaned version of both samples, committed to git
```

## What is NOT in this folder

The full raw dataset from Kaggle is never stored in this repository. It is several gigabytes and would make the repo unusable. If you need the full dataset for any reason, see the instructions below.

---

## Full dataset

**Source:** [E-Commerce Behavior Data from Multi-Category Store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store)

The full dataset consists of two files:
- `2019-Oct.csv` — 5.67 GB
- `2019-Nov.csv` — 9.01 GB

To access the full dataset, go to the Kaggle page linked above. You will need a free Kaggle account.

---

## Data strategy

The project works with two tiers of data:

**Tier 1 — Sample data (in this repo)**
Random samples taken from each raw file before any cleaning. These are committed directly to git so every teammate can develop and test code immediately after cloning the repo without downloading anything.

**Tier 2 — Full dataset (Kaggle only)**
Only needed if a specific project component requires more data than the sample provides. In that case, download the full dataset from Kaggle, run the preprocessing notebook, and save the output to Google Drive. See the team group chat for the Google Drive link.

---

## How the samples were created

The sampling and cleaning process is documented in `notebooks/01_preprocessing.ipynb`. 
The underlying functions are in `src/data/preprocess.py`. To recreate the samples 
from scratch, download the full dataset from Kaggle and run:

    python src/data/preprocess.py