# STEM Factors

Analysis of factors influencing students' orientation toward STEM fields, using machine learning. Diploma thesis project — Faculty of Science, University of Kragujevac.

**Author:** Tijana Đoković

## Overview

The project analyzes survey data collected from gymnasium students to identify which factors — academic performance, subject attitudes, and socio-demographic background — are associated with choosing a STEM-oriented track, and builds machine learning models that classify students by this orientation.

Every step, from raw data to the final results, is documented in a single self-contained Jupyter notebook: each conclusion is backed by code and output in the same file.

## Project structure

| File                                 | Description                  |
| ------------------------------------ | ---------------------------- |
| `stem_factors.ipynb`                 | Main analysis notebook       |
| `STEM.xlsx`                          | Raw survey data (input only) |
| `Diplomski rad - dokumentacija.docx` | Thesis document              |

## Notebook contents

1. **Data preparation** — loading, data quality checks, target variable definition
2. **Descriptive analysis** — distributions, categorical breakdowns, correlations
3. **Statistical tests** — t-tests and χ² tests of association with the target variable
4. **Data preprocessing for modeling** — cleaning, encoding, train/test split; scaling and feature selection are built into the modeling pipeline (fitted inside each cross-validation fold), not applied once beforehand
5. **Model development and evaluation methodology** — the preprocessing pipeline (column-wise scaling → `SelectKBest` feature selection → model), cross-validation strategy, evaluation metrics, hyperparameter tuning (including the number of selected features `k`), nested cross-validation
6. **Results and comparative model analysis** — five classifiers (Logistic Regression, KNN, SVM, Random Forest, XGBoost) trained and compared
7. **Model interpretability** — odds ratios, feature importance, SHAP analysis
8. **Fairness and robustness analysis** — subgroup performance (compared against each subgroup's base rate), bias checks, sensitivity to removing the single strongest predictor and the whole block of retrospective "reasons for enrolment" questions

A guiding principle throughout preprocessing and modeling: a feature is only kept if it represents a genuine factor of STEM orientation rather than an administrative/institutional artifact of the sample (e.g. the specific school a student attends is excluded for this reason, even though it correlates with the target).

## Setup

Requires Python 3.10+ (developed and tested with 3.14).

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn xgboost shap openpyxl
```

## Running

Open `stem_factors.ipynb` and run all cells **top to bottom** — later sections reuse variables, helpers, and trained models defined earlier (e.g. `X_train`, `napravi_pipeline`, `skf`, `rf_best`), so cells must be executed in order for the notebook to run without errors.

## Data

`STEM.xlsx` contains anonymized survey responses from students at four Serbian gymnasiums, covering academic performance, subject interest/difficulty/usefulness ratings, learning habits, parental involvement, and socio-demographic background. The notebook derives a binary target variable (`STEM`) from each student's reported track/major.

The notebook can also be run on a different dataset, as long as it follows the same column structure and naming as `STEM.xlsx` (question wording, Likert-scale format, and the `Смер:` column used to derive the target variable) — otherwise the column references throughout the notebook would need to be adjusted accordingly.
