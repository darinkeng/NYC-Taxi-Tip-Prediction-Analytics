# NYC Taxi Tip Prediction Pipeline

## Project Overview

This project builds an end-to-end data analytics and machine learning pipeline to predict taxi tip amounts using NYC Yellow Taxi trip records. The workflow combines local SQL exploration, cloud-based querying, feature engineering, and regression modeling to transform raw trip-level data into predictions.

The goal of this project is to demonstrate how large transportation datasets can be cleaned, queried, modeled, and interpreted using a modern data science workflow.

## Business Problem

Taxi tipping behavior is influenced by factors such as trip distance, fare amount, pickup and dropoff locations, payment type, time of day, and passenger behavior. Understanding these patterns can help transportation platforms, fleet operators, and analysts better understand customer payment behavior and identify the main drivers of tip amounts.

In this project, I focus on predicting the tip amount for credit-card taxi trips using trip, fare, time, and location-based features.

## Key Questions

* Which trip characteristics are most associated with higher taxi tips?
* Can fare, distance, time, and location variables be used to predict tip amount?
* How can raw taxi trip records be transformed into model-ready features using SQL and Python?
* What does a scalable workflow look like when moving from local data exploration to cloud-based querying?

## Dataset

The project uses NYC Yellow Taxi trip records in Parquet format. The dataset includes trip-level information such as:

* Pickup and dropoff timestamps
* Pickup and dropoff location IDs
* Trip distance
* Fare amount
* Tip amount
* Total amount
* Payment type
* Passenger count
* Rate code and vendor information

Only credit-card trips were used for tip prediction, since cash tips are generally not recorded in the dataset.

## Tools and Technologies

* **Python**: data processing, feature engineering, modeling
* **SQL**: data extraction and transformation
* **DuckDB**: local SQL exploration on Parquet files
* **BigQuery**: cloud-based querying and scalable feature extraction
* **Google Cloud Storage**: cloud data storage
* **Pandas / NumPy**: data manipulation
* **CatBoost**: gradient boosting regression model
* **Scikit-learn**: model evaluation and train-test split
* **Matplotlib / Seaborn**: exploratory visualization

## Project Workflow

```text
Raw NYC Yellow Taxi Parquet Data
        ↓
Local data validation with DuckDB
        ↓
Upload data to Google Cloud Storage
        ↓
Create external table in BigQuery
        ↓
Query and filter credit-card taxi trips
        ↓
Feature engineering in SQL and Python
        ↓
Train CatBoost regression model
        ↓
Evaluate model using MAE and RMSE
        ↓
Interpret feature importance
```

## Methodology

### 1. Local Data Exploration

I first used DuckDB to inspect the raw Parquet files locally. This helped validate the schema, check column types, and understand the basic structure of the taxi trip records before moving the data into a cloud environment.

Example checks included:

* Counting total records
* Inspecting missing values
* Reviewing fare and tip distributions
* Filtering invalid or unusual trip records

### 2. Cloud-Based Querying

After local inspection, the data was uploaded to Google Cloud Storage and queried using BigQuery. This step simulated a more scalable analytics workflow where large datasets are stored and queried in the cloud.

BigQuery was used to:

* Filter valid taxi trips
* Select credit-card transactions
* Remove unrealistic fare, distance, and tip values
* Extract time-based features
* Prepare a modeling table for Python

### 3. Feature Engineering

I created model-ready features from raw trip records, including:

* Trip distance
* Fare amount
* Total amount excluding tip
* Passenger count
* Pickup hour
* Pickup day of week
* Pickup and dropoff location IDs
* Vendor ID
* Rate code
* Payment type

Categorical variables such as location IDs and vendor information were handled using CatBoost, which can directly process categorical features.

### 4. Predictive Modeling

I trained a CatBoost regression model to predict taxi tip amount. CatBoost was selected because it performs well on tabular datasets and can handle categorical variables efficiently without requiring extensive one-hot encoding.

The model was evaluated using:

* **Mean Absolute Error (MAE)**
* **Root Mean Squared Error (RMSE)**

These metrics provide an interpretable view of how close the predicted tip amounts are to the actual recorded tips.

## Results

Key findings:

* Fare amount and total trip cost were strong predictors of tip amount.
* Trip distance and time-based features added useful predictive signal.
* Pickup and dropoff location IDs helped capture geographic tipping patterns.
* CatBoost was effective for handling high-cardinality categorical features such as taxi zone IDs.

Model performance:

| Metric |                Value |
| ------ | -------------------: |
| MAE    |              `$1.18` |
| RMSE   |              `$2.33` |



