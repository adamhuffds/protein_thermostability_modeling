# Novozymes Enzyme Stability Prediction

Regression modeling project predicting protein thermostability (melting temperature, `tm`) from amino acid sequence composition and pH. Based on the [Novozymes Enzyme Stability Prediction](https://www.kaggle.com/competitions/novozymes-enzyme-stability-prediction) Kaggle competition dataset.

---

## Project Overview

Proteins denature at characteristic temperatures depending on their amino acid composition and the chemical environment they operate in. This project builds a regression pipeline to predict melting temperature (`tm`) using amino acid proportional composition as engineered features alongside pH.

The modeling approach is intentionally interpretable — no pretrained embeddings or language models — making it a useful baseline for understanding which physicochemical properties correlate with thermostability.

---

## Project Status

Code is functionally complete. Annotation and notebook splitting into discrete stages (EDA, cleaning, feature engineering, modeling) is in progress.

---

## Repository Structure

    novozymes-thermostability/
    ├── data/                   # excluded from version control (see below)
    ├── modeling.ipynb          # end-to-end pipeline: EDA → cleaning → feature engineering → modeling
    ├── requirements.txt        # pinned dependencies
    ├── .gitignore
    └── README.md

---

## Data

Data is sourced from the Kaggle competition and is **not included in this repository** in compliance with Kaggle's terms of use.

To reproduce: download `train.csv` from the [competition data page](https://www.kaggle.com/competitions/novozymes-enzyme-stability-prediction/data) and place it in a local `data/` directory.

The `data/` folder is listed in `.gitignore` and will never be committed.

---

## Pipeline Summary

**1. Data Loading & Exploration**  
Load `train.csv`, inspect shape, data types, and summary statistics. Columns: `seq_id`, `protein_sequence`, `pH`, `data_source`, `tm`.

**2. Missing Value Handling**  
Identify and quantify missing values in `pH` and `data_source`. Drop rows missing either column. Retain low-`tm` outliers (<=10°C) as they are sparse and unlikely to materially affect model performance.

**3. Duplicate Analysis**  
Flag full-row duplicates and duplicates on `protein_sequence`, `protein_sequence + pH`, and `protein_sequence + tm`. Duplicates are retained with rationale: the same sequence measured under different conditions or by different labs legitimately produces different `tm` values.

**4. Feature Engineering**  
Compute per-sequence amino acid proportional composition using `collections.Counter`. Each of the 20 standard amino acids becomes a numeric feature (proportion of residues in the sequence). Missing amino acids (not present in a given sequence) are filled with 0.

**5. Modeling**  
Three regression models evaluated on an 80/20 train/test split:
- Linear Regression
- Lasso Regression (`alpha=0.001`)
- Random Forest Regressor

Metrics: R² and RMSE.

---

## Environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Python Key dependencies: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`.

---

## References

- [Novozymes Enzyme Stability Prediction — Kaggle](https://www.kaggle.com/competitions/novozymes-enzyme-stability-prediction)
- [scikit-learn: RandomForestRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)
- [scikit-learn: train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)