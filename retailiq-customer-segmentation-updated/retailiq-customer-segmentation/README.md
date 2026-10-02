# RetailIQ — Customer Segmentation, Retention & Predictive Customer Value

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Piyush-Toraskar/retailiq-customer-segmentation/blob/main/notebooks/RetailIQ_Advanced_Customer_Analytics.ipynb)

An end-to-end data science project that turns retail transaction history into actionable customer analytics using **RFM segmentation, cohort retention analysis and supervised machine learning for 90-day customer spend prediction**.

## Project overview

The project uses the **UCI Online Retail II** dataset, containing two years of transaction-level data from a UK-based online retailer. The raw workbook contains **1,067,371 transaction rows**. After removing missing customer IDs, cancellations, non-positive quantities/prices and exact duplicates, **793,609 valid purchase rows** remain across **5,878 customers**.

Dataset: https://archive.ics.uci.edu/dataset/502/online+retail+ii

> The raw Excel dataset is intentionally not committed to this repository. Download it from UCI and upload it when the Colab notebook prompts you.

## What the project does

### 1. Customer segmentation
- Engineers **Recency, Frequency and Monetary (RFM)** features.
- Clips extreme values, applies `log1p`, and standardises features.
- Evaluates candidate values of K using the **Elbow Method** and **Silhouette Score**.
- Fits **K-Means with K=4** for an interpretable business segmentation.
- Uses **PCA** to visualise the three-dimensional RFM feature space.

### 2. Cohort retention analysis
- Assigns customers to monthly acquisition cohorts based on first purchase.
- Tracks active customers by month since acquisition.
- Builds a **12-month cohort retention matrix and heatmap**.

### 3. 90-day customer spend prediction
- Uses a time-based cut-off so future information does not leak into model features.
- Engineers **13 behavioural features** including RFM, average order value, quantity, unique products, tenure, and recent 30/90-day activity.
- Compares:
  - Mean baseline
  - Linear Regression
  - Random Forest
  - XGBoost
- Evaluates models using **MAE, RMSE and R²**.
- Analyses feature importance and combines predicted value with customer segments.

## Key segmentation results

| Segment | Customers | Customer share | Avg. recency | Avg. frequency | Avg. spend | Revenue share |
|---|---:|---:|---:|---:|---:|---:|
| High-Value Loyal | 1,222 | 20.8% | 27.8 days | 19.0 | 10,760.9 | **74.4%** |
| Lapsed Valuable Customers | 1,461 | 24.9% | 230.1 days | 5.0 | 1,972.5 | 16.3% |
| Recent / Developing | 1,235 | 21.0% | 28.9 days | 3.0 | 830.5 | 5.8% |
| Low-Engagement / Dormant | 1,960 | 33.3% | 396.7 days | 1.4 | 320.5 | 3.6% |

### Segmentation insight

Only **20.8% of customers generate 74.4% of total revenue**, highlighting a highly concentrated high-value customer base. The segmentation also reveals a sizeable group of historically valuable customers who have become inactive, creating a clear re-engagement opportunity.

### Why K=4?

`K=2` achieves the highest Silhouette Score (**0.441**), while `K=4` scores **0.371**. K=4 is retained as a deliberate trade-off between statistical separation and business interpretability because it produces four distinct behavioural segments rather than a coarse high/low-value split.

## Predictive modelling results

The prediction task estimates each customer's spend in the final **90-day target window** using behaviour observed before the cut-off date.

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| **Random Forest** | **548.27** | 5,626.61 | 0.034 |
| XGBoost | 552.30 | **5,621.48** | **0.035** |
| Mean Baseline | 873.34 | 5,725.76 | -0.001 |
| Linear Regression | 1,209.37 | 14,544.63 | -5.457 |

**Random Forest reduced MAE by ~37% versus the mean baseline.** The low R² is reported transparently: individual future spend is noisy and highly skewed, so the model is more useful for **relative customer prioritisation** than exact spend forecasting.

## Most important predictive features

| Feature | Importance |
|---|---:|
| Monetary | 31.4% |
| Recency | 20.3% |
| Total Quantity | 9.8% |
| Frequency | 8.9% |
| Spend Last 90 Days | 7.7% |
| Unique Products | 6.9% |
| Customer Tenure | 6.6% |
| Average Order Value | 6.0% |

Historical customer value and recency are the strongest signals for near-term spend, while recent activity and breadth of purchasing add incremental predictive power.

## Business interpretation

- **High-Value Loyal:** protect retention and target premium cross-sell opportunities.
- **Recent / Developing:** encourage repeat purchases and identify emerging high-value customers.
- **Lapsed Valuable Customers:** prioritise targeted re-engagement because historical value is meaningful but recency has deteriorated.
- **Low-Engagement / Dormant:** use low-cost reactivation rather than high-cost retention activity.

The final customer-level output combines **historical segment + predicted 90-day spend**, allowing customers to be prioritised into groups such as `Protect / Retain`, `Emerging High-Value` and `Re-engagement Opportunity`.

## Repository structure

```text
retailiq-customer-segmentation/
├── notebooks/
│   └── RetailIQ_Advanced_Customer_Analytics.ipynb
├── outputs/
│   ├── cluster_summary.csv
│   ├── cohort_retention.csv
│   ├── customer_analytics.csv
│   ├── feature_importance.csv
│   ├── k_evaluation.csv
│   ├── model_metrics.csv
│   └── predictive_model_dataset.csv
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## Run in Google Colab

1. Open `notebooks/RetailIQ_Advanced_Customer_Analytics.ipynb` in Google Colab.
2. Use a **Python 3 / CPU** runtime.
3. Run all cells.
4. Upload `online_retail_II.xlsx` when prompted.
5. The notebook exports all analytics outputs into `RetailIQ_Advanced_outputs.zip`.

## Techniques demonstrated

`Python` · `Pandas` · `NumPy` · `EDA` · `Feature Engineering` · `RFM Analysis` · `StandardScaler` · `K-Means` · `Silhouette Score` · `PCA` · `Cohort Analysis` · `Linear Regression` · `Random Forest` · `XGBoost` · `MAE` · `RMSE` · `R²` · `Feature Importance`

## Limitations

- RFM does not capture demographics, marketing exposure or detailed product semantics.
- K-Means assumes distance-based cluster structure and requires K to be specified.
- Future spend is highly skewed and many customers make no purchase in the target window.
- Predictive evaluation uses a single historical/future cut-off rather than multiple rolling-origin backtests.
- Business actions are analytical recommendations rather than experimentally proven causal effects.
