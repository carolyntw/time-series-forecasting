# Multivariate Retail Sales Forecasting with Prophet

## Overview

This project uses **Facebook Prophet** to forecast daily sales across various product families for a national retail dataset. Each product category is modeled separately with **automated hyperparameter tuning**, **holiday effects**, and **30-day forward predictions**. Forecast accuracy is evaluated using **Mean Absolute Percentage Error (MAPE)** and visualized for each category.

---

## Objectives

- Forecast **30 days** of daily sales for each product family
- Tune Prophet's hyperparameters for optimal accuracy
- Incorporate **Ecuadorian national holidays** as external regressors
- Evaluate forecast performance using MAPE and visual comparisons

---

## Dataset

- **Source:** [Kaggle - Store Sales Time Series Forecasting](https://www.kaggle.com/competitions/store-sales-time-series-forecasting)
- **Structure:**
  - Daily sales records from 2013 to 2017
  - Aggregated by `date` and `family` (product category)
- **Target Variable:** `sales`

---

##  Methodology

### 1. Data Preparation
- Cleaned and pivoted the dataset into a time-series format per family
- Added Ecuadorian national holidays using the `holidays` package

### 2. Model Selection & Tuning
For each product family:
- Data was formatted for Prophet (`ds`, `y`)
- Hyperparameters tuned via grid search:
  - `changepoint_prior_scale`
  - `seasonality_prior_scale`
  - `holidays_prior_scale`
- Evaluated with Prophet's built-in `cross_validation()` and `performance_metrics()`
- Best configuration selected by **lowest MAPE**

### 3. Forecasting
- Trained final Prophet model with tuned parameters
- Forecasted the next 30 days
- Visualized and compared predictions with actuals

---

## Results

### MAPE by Product Family

| Product Family         | MAPE (%) |
|------------------------|----------|
| **Excellent (<6%)**    | BREAD/BAKERY (5.5), MEATS (5.7), PRODUCE (5.1) |
| **Good (6–10%)**       | BEVERAGES (7.7), EGGS (7.2), DAIRY (8.2), DELI (8.8), GROCERY I (8.6), POULTRY (7.5), PERSONAL CARE (9.8) |
| **Acceptable (10–15%)**| HOME CARE (11.7), PREPARED FOODS (11.5), HOME & KITCHEN I (13.3), HOME & KITCHEN II (15.3) |
| **Needs Improvement**  | GROCERY II (16.0), CLEANING (19.3), SEAFOOD (13.8) |

📌 High-volume, stable product families consistently performed well with <10% error  
📌 A few volatile or lower-volume families (e.g., **CLEANING**, **GROCERY II**) showed underfitting and may benefit from alternative models or additional features

---

## Forecast Quality Assessment

| MAPE Range | Interpretation             | Forecast Use-Case Suitability        |
|------------|----------------------------|--------------------------------------|
| 0–5%       | Exceptional                | Pricing, promotions, automation      |
| 5–10%      | Good                       | Supply chain, operations planning    |
| 10–15%     | Acceptable with buffer     | Inventory stocking, general planning |
| >15%       | Likely underfit/erratic    | Needs regressors or model refinement |

Overall, 12 out of 17 product families had **MAPE < 10%**, indicating **strong predictive performance** using Prophet.

---

## Deliverables

| File | Description |
|------|-------------|
| `forecasting.ipynb` | Full pipeline with data prep, tuning, and forecasting |
| Charts (in notebook) | Forecast vs actual plots for visual comparison |

---

## Insights

- Prophet effectively captured **seasonal trends and holidays** for high-volume categories
- Per-category hyperparameter tuning significantly improved results
- Some categories with higher MAPE may benefit from:
  - External regressors (e.g., promotions, oil price)
  - Alternative models (e.g., XGBoost, LightGBM)

---

## Future Enhancements

- Incorporate promotional calendars and price changes
- Benchmark Prophet against other models on volatile categories
- Build a dashboard (Streamlit, Tableau, Power BI) for stakeholder access
- Deploy via API for real-time forecasting in production