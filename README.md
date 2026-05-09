# E-Commerce User Behavior Analysis & Recommendation System

A graduate Data Science project analyzing user behavior from a large-scale e-commerce dataset. The project covers exploratory data analysis, preprocessing, feature engineering, user segmentation, collaborative filtering, A/B testing, time-sensitive recommendations, customer journey analysis, and purchase timing prediction.

---

## Dataset

**Full dataset source:** [E-Commerce Behavior Data from Multi-Category Store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store)

---

## Project Structure
```
ecommerce-behavior-analysis/
│
├── README.md
├── final_report.pdf                     # Project report deliverable
├── final_poster.pdf                     # Project poster deliverable
│
├── data/
│   ├── README.md                        # Explaination of folder data folder set up
│   ├── raw/                             # Points to full dataset on Kaggle
│   └── processed/                       # Processed output files from each notebook
│
├── notebooks/
│   ├── 00_data_exploration.ipynb
│   ├── 01_full_dataset_exploration.ipynb
│   ├── 02_initial_data_preprocessing.ipynb
│   ├── 03_user_behavior_analysis.ipynb
│   ├── 04_feature_engineering.ipynb
│   ├── 05_user_segmentation.ipynb
│   ├── 06_collaborative_filtering.ipynb
│   ├── 07_ab_testing_analysis.ipynb
│   ├── 08_time_sensitive_recommendations.ipynb
│   ├── 09_customer_journey_visualization.ipynb
│   └── 10_price_timing_predictor.ipynb
│
└── reports/
└── figures/                         # Output plots organized by notebook
```

---

## Notebooks

Each notebook is self-contained and runs on Kaggle. All required datasets are attached as Kaggle inputs — see the header of each notebook for data paths and reproduction instructions.

| Notebook | Description | Author |
|---|---|---|
| 00 | Sample data exploration | Hallee Pham |
| 01 | Full dataset exploration (110M rows) | Hallee Pham |
| 02 | Data preprocessing and cleaning | Kyle Naluan |
| 03 | User behavior analysis | Kyle Naluan |
| 04 | Feature engineering | Kyle Naluan |
| 05 | User segmentation (K-Means clustering) | Kyle Naluan |
| 06 | Collaborative filtering recommendations | Hallee Pham |
| 07 | A/B testing analysis | Kai White |
| 08 | Time-sensitive recommendations | Hallee Pham |
| 09 | Customer journey visualization | Kai White |
| 10 | Price timing predictor | Morgan Sansone |
