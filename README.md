# RetailIQ - Customer Segmentation & Purchase Behaviour Analytics

Customer segmentation project using the UCI Online Retail II dataset.

## Overview

This project analyses over one million retail transactions and segments customers based on purchasing behaviour using RFM analysis and K-Means clustering.

## Pipeline

Data Cleaning -> EDA -> RFM Feature Engineering -> Outlier Treatment -> Standardisation -> K-Means -> PCA -> Customer Segment Profiling

## Machine Learning

- K-Means Clustering
- PCA for visualisation
- Elbow Method
- Silhouette Score

## Key Results

- Analysed 1.06M+ transactions
- Retained 5,878 identifiable customers after cleaning
- Identified four behavioural customer segments
- High-value loyal customers represented approximately 21% of customers while contributing approximately 74% of revenue
- K=4 was selected to provide more actionable customer segmentation despite K=2 achieving the highest silhouette score

## Customer Segments

- High-Value Loyal
- Recent / Developing
- Lapsed Valuable Customers
- Low-Engagement / Dormant

## Tech Stack

Python, Pandas, NumPy, Scikit-learn, Matplotlib

## Dataset

UCI Machine Learning Repository - Online Retail II

The raw dataset is not included in this repository. Download it from:
https://archive.ics.uci.edu/dataset/502/online+retail+ii

## Run

Open RetailIQ_Customer_Segmentation.ipynb locally or in Google Colab and provide the Online Retail II Excel dataset when prompted.
