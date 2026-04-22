# 🏥 U.S. Chronic Disease Indicators — Exploratory Analysis & State Clustering
> Exploratory analysis, unsupervised clustering, and regression modeling on the CDC's U.S. Chronic Disease Indicators dataset — uncovering geographic patterns in public health across all 50 states and territories.

## 🔍 Overview
This project analyzes the **U.S. Chronic Disease Indicators (CDI)** — a publicly available CDC dataset — to understand the distribution of chronic disease burden across U.S. states and territories. The analysis spans data cleaning, exploratory analysis, K-Means clustering to group states by health risk profile, and an in-progress regression model to predict cardiovascular mortality.

The central question: **Can we group U.S. states into meaningful health risk profiles using chronic disease indicators, and what predicts coronary heart disease mortality?**

---

## 📊 Dataset

| Attribute | Detail |
|-----------|--------|
| **Source** | [CDC U.S. Chronic Disease Indicators — data.gov](https://catalog.data.gov/dataset/u-s-chronic-disease-indicators) |
| **Rows** | 309,215 |
| **Columns** | 30 (26 after cleanup) |
| **Coverage** | All 50 U.S. states + D.C., Puerto Rico, Virgin Islands, Guam |
| **Years** | Multi-year spanning ~2015–2022 |
| **Topics** | 19 chronic disease categories |

### Disease Topics Covered

`Alcohol` · `Arthritis` · `Asthma` · `Cancer` · `Cardiovascular Disease` · `Chronic Kidney Disease` · `COPD` · `Cognitive Health & Caregiving` · `Diabetes` · `Disability` · `Health Status` · `Immunization` · `Maternal Health` · `Mental Health` · `Nutrition, Physical Activity & Weight` · `Oral Health` · `Sleep` · `Social Determinants of Health` · `Tobacco`

---

## 📁 Project Structure

```
├── USDAnalysis.ipynb                  # Main analysis notebook
├── U.S._Chronic_Disease_Indicators.csv  # Raw dataset (not tracked — see note below)
└── README.md
```

## 🔬 Analysis Phases

### Phase 1 — Data Cleaning
- Removed 4 unnamed/empty columns from the raw 30-column dataset
- Dropped `Response` and `ResponseID` columns (mostly null)
- Removed confidence interval columns (`LowConfidenceLimit`, `HighConfidenceLimit`)
- Excluded national aggregate rows (`LocationAbbr == "US"`) to focus on state-level analysis
- Filtered to percentage-based indicators for consistency across topics

**Result:** Clean working dataset of **303,452 rows × 19 columns**

---
### Phase 2 — Exploratory Analysis
- Profiled all 19 disease topics by record count
- Enumerated unique questions per topic:
  - **Cardiovascular Disease:** 8 questions
  - **Cancer:** 10 questions
  - **COPD:** 6 questions
  - **Diabetes:** 4 questions
  - **Nutrition, Physical Activity & Weight:** 14 questions

---
### Phase 3 — State Clustering (K-Means)
- Pivoted long-format data to **wide format** (one row per state, questions as features)
- Dropped sparse columns (>40% missing), retaining 84 features across 54 locations
- Imputed remaining missing values with column medians
- Scaled features with `StandardScaler`
- Applied **PCA** (retaining 90% variance → 9 components) to reduce noise and multicollinearity
- Ran K-Means for k=2 through k=8; evaluated with **elbow curve** and **silhouette scores**

**Best k = 2** (silhouette score: 0.35)

| Cluster | States |
|---------|--------|
| **0 — Higher Burden** | Alabama, Arkansas, Georgia, Indiana, Kentucky, Louisiana, Mississippi, Oklahoma, Puerto Rico, South Carolina, Tennessee, West Virginia |
| **1 — Lower Burden** | Remaining 42 states/territories |

---

### Phase 4 — Regression Modeling *(In Progress)*
- **Target:** Coronary heart disease mortality (per 100,000)
- **Predictors:** Diabetes prevalence, high blood pressure, smoking rates, physical inactivity, high cholesterol
- **Method:** Linear Regression with train/test split (70/30)
- **Status:** Debugging column naming inconsistency from pivot step — fix in progress

---

## 💡 Key Findings

- **Geographic clustering is real:** K-Means cleanly separated states into a higher-burden Southern group and a lower-burden group without any geographic features being fed into the model — the health indicators alone drove the separation.
- **Cardiovascular Disease is the most data-rich topic** in the dataset (~30,000 records at state level), making it the strongest candidate for predictive modeling.
- **~32% of `DataValue` entries are missing** across the full dataset, concentrated in specific indicators and territories — imputation strategy matters.

---

## 🛠 Tech Stack

| Library | Purpose |
|---------|---------|
| `pandas` | Data loading, cleaning, reshaping |
| `numpy` | Numerical operations |
| `matplotlib` / `seaborn` | Visualization |
| `scikit-learn` | PCA, K-Means, Linear Regression, StandardScaler |

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Run

1. Download the dataset from [data.gov](https://catalog.data.gov/dataset/u-s-chronic-disease-indicators)
2. Place the CSV in the project root as `U.S._Chronic_Disease_Indicators.csv`
3. Open and run `USDAnalysis.ipynb` top to bottom

---

## 📌 Status & Roadmap

- [x] Data cleaning & preprocessing
- [x] Exploratory analysis by topic
- [x] K-Means clustering with PCA
- [x] State risk ranking visualization
- [ ] Fix regression column naming bug
- [ ] Complete linear regression + evaluate R² / RMSE
- [ ] Add feature importance / coefficient plot
- [ ] Explore additional clustering methods (hierarchical, DBSCAN)
- [ ] Incorporate socioeconomic features for richer modeling

---

## 📄 License

This project uses publicly available government data. Dataset credit: [Centers for Disease Control and Prevention (CDC)](https://www.cdc.gov/).
