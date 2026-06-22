# NYC Taxi Analytics & Tip Prediction Pipeline

## Project Overview

This project builds an end-to-end analytics and machine learning pipeline using NYC Yellow Taxi trip records. The project combines cloud SQL benchmarking, data engineering workflows, feature engineering, and predictive modeling to analyze taxi trip behavior and predict credit-card tip amounts.

The goal is to demonstrate how large-scale transportation data can be queried, optimized, transformed, and modeled using tools commonly used in data analyst, data scientist, and data engineering workflows.

The project has two main components:

1. **Cloud SQL Benchmarking**: compares analytical SQL workloads across Databricks and BigQuery.
2. **Tip Prediction Modeling**: builds a machine learning model to predict taxi tip amounts using trip, fare, time, and location features.

## Business Problem

NYC taxi trip records contain rich information about fare behavior, trip patterns, passenger behavior, payment methods, and tipping. Analyzing this data can help transportation platforms, fleet operators, and analysts better understand what factors influence customer tipping behavior.

This project explores two practical questions:

* How can large taxi trip datasets be queried efficiently across different cloud data platforms?
* Can trip-level features be used to predict taxi tip amounts for credit-card transactions?

By combining SQL benchmarking with machine learning, this project shows both the infrastructure side and the modeling side of a real-world analytics workflow.

## Dataset

This project uses [NYC Yellow Taxi Trip Records](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) in Parquet format. The dataset includes trip-level variables such as:

* Pickup and dropoff timestamps
* Pickup and dropoff location IDs
* Trip distance
* Fare amount
* Tip amount
* Total amount
* Payment type
* Passenger count
* Vendor ID
* Rate code

For the tip prediction task, the modeling dataset focuses on credit-card trips because cash tips are generally not fully recorded in the dataset.

## Tools and Technologies

* **SQL**: data extraction, filtering, joins, CTEs, window functions, and performance testing
* **Databricks**: distributed SQL processing and performance benchmarking
* **BigQuery**: cloud data warehouse querying and table comparison
* **DuckDB**: local exploration of Parquet files
* **Python**: data processing, feature engineering, and modeling
* **Pandas / NumPy**: data manipulation
* **CatBoost**: gradient boosting regression model
* **Scikit-learn**: train-test split and model evaluation
* **Matplotlib / Seaborn**: exploratory visualization and result communication

## Project Workflow

```text
Raw NYC Yellow Taxi Parquet Data
        ↓
Local schema inspection with DuckDB
        ↓
Cloud table setup in Databricks and BigQuery
        ↓
SQL workload benchmarking
        ↓
Data filtering and feature extraction
        ↓
Python feature engineering
        ↓
CatBoost regression model
        ↓
Model evaluation and interpretation
```

## Part 1: Cloud SQL Benchmarking

The first part of the project evaluates analytical SQL workloads across Databricks and BigQuery. The goal is to understand how different query patterns and storage choices affect performance when working with large taxi trip datasets.

The benchmarking experiments include:

* Data size scaling
* Join strategy comparison
* Broadcast join optimization
* Data skew and repartitioning
* Sorting and filtering
* Common table expressions
* Self-joins
* Window functions
* Internal vs external table performance in BigQuery

### Example SQL Workloads

The SQL benchmarking section includes queries such as:

* Joining taxi trips with lookup tables
* Detecting outlier fares and trip distances
* Comparing trips within the same pickup and dropoff zones
* Ranking trips using window functions
* Testing runtime differences between internal and external tables

### Key Benchmarking Results

| Experiment                 | Key Result                                                                                             |
| -------------------------- | ------------------------------------------------------------------------------------------------------ |
| Data size scaling          | Measured how query runtime changed as row count increased from smaller samples to larger taxi datasets |
| Join optimization          | Compared standard joins with broadcast join strategies                                                 |
| Data skew handling         | Improved processing throughput after repartitioning skewed data                                        |
| BigQuery table comparison  | Compared performance between internal and external BigQuery tables                                     |
| Cross-platform SQL testing | Evaluated similar analytical queries across Databricks and BigQuery                                    |

These experiments demonstrate practical SQL optimization skills and the ability to reason about cloud data platform tradeoffs.

## Part 2: Tip Prediction Modeling

The second part of the project builds a machine learning model to predict taxi tip amounts for credit-card transactions.

The modeling workflow includes:

1. Filtering valid taxi trips
2. Removing unrealistic or invalid records
3. Creating time-based and trip-based features
4. Handling categorical variables such as pickup and dropoff location IDs
5. Training a CatBoost regression model
6. Evaluating model performance using regression metrics
7. Interpreting the most important predictors of tip amount

## Feature Engineering

The model uses a combination of fare, trip, time, and location-based features, including:

* Trip distance
* Fare amount
* Total amount excluding tip
* Passenger count
* Pickup hour
* Pickup day of week
* Pickup location ID
* Dropoff location ID
* Vendor ID
* Rate code

Categorical variables were handled using CatBoost, which is well-suited for tabular datasets with high-cardinality categorical features.

## Model

A CatBoost regression model was trained to predict taxi tip amounts.

CatBoost was selected because:

* It performs well on structured tabular data
* It can handle categorical variables directly
* It reduces the need for extensive one-hot encoding
* It is effective for nonlinear relationships between trip features and tip amounts

## Model Evaluation

The model was evaluated using:

* **Mean Absolute Error (MAE)**
* **Root Mean Squared Error (RMSE)**

| Metric |                Value |
| ------ | -------------------: |
| MAE    |              `$1.18` |
| RMSE   |              `$2.33` |

The MAE result means that, on average, the model’s predicted tip amount was about `$1.18` away from the actual recorded tip amount.

## Key Findings

The analysis and modeling workflow showed that:

* Repartitioning skewed taxi trip data improved Databricks processing throughput from 1.18M to 4.62M rows/sec, showing the importance of partition strategy in distributed SQL workloads.
* Fare amount and total trip cost were strong predictors of tip amount.
* Trip distance provided additional predictive signal.
* Pickup and dropoff locations helped capture geographic tipping patterns.
* Time-based features such as pickup hour and day of week added useful context.
* Cloud SQL performance can vary meaningfully depending on table format, join strategy, query structure, and data partitioning.

## Repository Structure

```text
nyc-taxi-analytics-tip-prediction/
├── README.md
├── taxi_tip_prediction.ipynb
├── sql/
│   ├── databricks/
│   │   ├── 00_setup_tables.sql
│   │   ├── 01_size_scaling.sql
│   │   ├── 02_broadcast_join.sql
│   │   ├── 03_data_skew_repartitioning.sql
│   │   ├── ctes_joins.sql
│   │   ├── outlier_detection.sql
│   │   ├── self_join.sql
│   │   └── window_functions.sql
│   └── bigquery/
│       ├── ctes_joins.sql
│       ├── outlier_detection.sql
│       ├── self_join.sql
│       └── window_functions.sql


```

## Skills Demonstrated

This project demonstrates:

* SQL querying and performance benchmarking
* Cloud data warehouse analysis with BigQuery
* Distributed query processing with Databricks
* Local Parquet exploration with DuckDB
* Data cleaning and feature engineering
* Regression modeling with CatBoost
* Working with high-cardinality categorical variables
* Translating raw transportation data into analytical and predictive insights

## Project Summary

This project combines cloud SQL benchmarking and machine learning modeling into one end-to-end NYC taxi analytics pipeline. It demonstrates practical experience with SQL, cloud data platforms, Python modeling, feature engineering, and data science communication.



