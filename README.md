# Surgical Instrument Failure Analysis Based on Procedures Performed

**Author:** Puneet Aggarwal

## Executive Summary

This project analyzes surgical instrument usage and failure patterns to estimate remaining instrument life and identify factors associated with early failures. Multiple regression models were compared, and Random Forest was further tuned using GridSearchCV.

## Rationale

Why should anyone care about this question?

This research is important because identifying factors that contribute to early surgical instrument failure can help organizations improve instrument design, usage guidelines, and maintenance strategies. Predicting potential failures in advance may reduce operational costs, improve utilization of instrument life, enhance surgeon experience, and ultimately contribute to improved patient safety.

## Research Question

The objective of this project is to determine whether there is a relationship between the usable life of a surgical instrument, the types of surgeries it is used for, and the experience level of the surgeon, and whether these factors can help predict the likelihood of instrument failure before the end of its intended life.

## Data Sources

What data will you use to answer your question?

Due to data privacy and organizational restrictions, proprietary production data cannot be used directly. For this project, the analysis is conducted using anonymized datasets derived from real-world patterns that simulate surgical instrument usage, surgery types, surgeon experience levels, and reported instrument failures.

## Methodology

The project applies multiple machine learning and data analysis techniques, including:

- Exploratory Data Analysis (EDA) to understand data distributions, trends, and correlations.
- Data preprocessing techniques such as encoding categorical variables and feature scaling.
- Supervised learning models including Random Forest, Linear Regression, and Gradient Boosting.
- Model evaluation and comparison using performance metrics.
- Visualization techniques (e.g., charts and graphs) to communicate insights effectively.

## Results

What did your research find?

### Final Summary of Exploratory Data Analysis

#### Data Overview and Cleaning

- The initial dataset contained 952 entries and 9 columns. Significant missing values were identified in `Anonymized Lot`, `4 digit Seq`, `Type of Procedures`, and `Experience of Surgeon who performed Last Surgery(in Years)`.
- Initial cleaning involved filling missing values and then dropping incomplete records, resulting in a cleaned dataset of 730 entries.
- A `Customer Lot` column was derived by concatenating `Anonymized Lot` and `4 digit Seq`.

#### Procedure Type Analysis

- The `Type of Procedures` column, containing pipe-separated values, was split and individual procedures were counted.
- The top 10 most frequent procedures were identified and visualized, with `Prostatectomy - Radical w/ Lymphadenectomy` being the most common.

#### Surgeon Experience Analysis

- The `Experience Level` column, categorized as `L` (Low), `M` (Medium), and `H` (High), was analyzed.
- The distribution showed `M` as the most common category (343 entries), followed by `H` (313 entries), and `L` (74 entries).

#### Remaining Uses Distribution

- `Remaining Uses` showed a right-skewed distribution, with a mean of approximately 6.37, a median of 5.0, and a maximum of 81.0.
- This indicates that most instruments fail with few remaining uses, but a few have significantly higher remaining uses, suggesting early defects.

#### Relationships Exploration

- **Surgeon Experience vs. Remaining Uses:** Box plots indicated no drastic differences across experience levels, but all groups showed high outliers, suggesting other factors contribute to premature failures.
- **Procedure Type vs. Remaining Uses:** Box plots for the top 5 procedures showed variation in `Remaining Uses` distribution across procedures, highlighting that some procedures may be more demanding on instruments.

#### Analysis of Early Defects

- The 75th percentile of `Remaining Uses` was calculated as 8.0 and used as the threshold for high remaining uses (early defects).
- A total of 175 records had `Remaining Uses > 8.0`.
- The top procedures associated with these early defects were identified and visualized, with `Prostatectomy - Radical w/ Lymphadenectomy` and `Hysterectomy - Malignant` appearing frequently.

#### Correlation Analysis of Combined Factors

- Individual procedures, `Experience Level`, and `Instrument` were one-hot encoded.
- Strongest positive correlation with `Remaining Uses`: `Component Separation TAR` (0.351).
- Strongest negative correlation with `Remaining Uses`: `Prostatectomy - Radical w/ Lymphadenectomy` (-0.207).

#### Baseline Machine Learning Model

- A `RandomForestRegressor` was built to predict `Remaining Uses` using one-hot encoded procedures, experience levels, and instruments as features.
- Baseline performance:
  - MAE: 2.242
  - RMSE: 4.630
  - R-squared: 0.636
- This indicates that approximately 63.6% of variance in `Remaining Uses` is explained by the selected features.

#### Additional Model Development and Comparison

- Two additional regression models were implemented and evaluated on the same train/test split:
  - Linear Regression: MAE = 1.599, RMSE = 5.186, R-squared = 0.543
  - Gradient Boosting Regressor: MAE = 2.684, RMSE = 5.370, R-squared = 0.510
- Model comparison showed that Random Forest remained strongest overall based on RMSE and R-squared, while Linear Regression produced the lowest MAE.

#### Hyperparameter Tuning with GridSearchCV

- `GridSearchCV` was applied to `RandomForestRegressor` using 3-fold cross-validation.
- Hyperparameter search space:
  - `n_estimators`: [100, 200, 300]
  - `max_depth`: [10, 20, 30, None]
  - `min_samples_split`: [2, 5, 10]
  - `min_samples_leaf`: [1, 2, 4]
- Best hyperparameters identified:
  - `max_depth = None`
  - `min_samples_leaf = 1`
  - `min_samples_split = 2`
  - `n_estimators = 300`
- Tuned Random Forest performance:
  - MAE: 2.235
  - RMSE: 4.675
  - R-squared: 0.629
- Compared with baseline Random Forest (MAE = 2.242, RMSE = 4.630, R-squared = 0.636), tuning provided a slight MAE improvement but slightly worse RMSE and R-squared.
- Final interpretation: the baseline Random Forest was already near-optimal within the tested hyperparameter range.

## Next Steps

Future work can focus on:

- Feature engineering (e.g., interaction terms, grouped procedure complexity features, and usage trajectory signals).
- Expanded tuning strategy (broader Random Forest search space, alternative scoring objectives such as RMSE, and revised cross-validation strategies).
- Additional advanced models (e.g., XGBoost, LightGBM, CatBoost) and ensemble approaches.
- Error analysis to identify procedure types or surgeon-experience groups where prediction performance drops.

## Outline of Project

- [[Link to notebook 1]()](https://github.com/aggpuneet01/Capstone-Project-Final/blob/main/Capstone_Project_Final.ipynb)

## Contact and Further Information
