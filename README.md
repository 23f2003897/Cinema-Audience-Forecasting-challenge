# Cinema-Audience-Forecasting-challengE

🎬 Cinema Audience Forecasting Challenge

This project predicts daily cinema audience counts across multiple theaters using historical audience data, calendar information, and ensemble machine learning models. The solution is designed to be time-series safe, interpretable, and robust for real-world forecasting.

📌 Problem Statement

Given historical audience visits and booking data, the task is to predict the number of visitors per theater per day for a future period.
Audience behavior depends on strong temporal patterns such as weekdays, weekends, and seasonal trends.

📂 Dataset Overview

The notebook uses the following competition files:

File	Description
booknow_visits.csv	Target variable: daily audience count
date_info.csv	Calendar & weekday information
sample_submission.csv	Submission format

Note: The final modeling focuses on calendar-based and temporal signals, avoiding heavy reliance on sparse booking joins to maintain stability.

🔍 Exploratory Data Analysis (EDA)

The EDA focuses on understanding temporal demand patterns:

Audience distribution is right-skewed

Strong weekly seasonality (weekends outperform weekdays)

Clear monthly trends

Audience varies significantly across theaters

Online booking signal is sparse but informative

Key plots include:

Audience distribution histogram

Average audience by weekday

Weekend vs weekday comparison

Daily audience trend over time

Monthly audience patterns

Correlation heatmaps

🛠 Feature Engineering

Only leakage-safe, future-available features are used.

📅 Date-Based Features

Extracted from show_date:

day_of_week (numeric: 0–6)

is_weekend

month

quarter

weekofyear

dayofmonth

These features capture seasonality and weekly cycles, which dominate audience behavior.

🎟 Booking Features

total_online_tickets

total_offline_tickets

Missing values are filled safely with zeros.

🏷 Categorical Features

book_theater_id

day_of_week

theater_type

theater_area

Encoded using OneHotEncoder inside a pipeline.

⚙️ Preprocessing Pipeline

A single shared preprocessing pipeline is used across all models:

Numerical features

Median imputation

Standard scaling

Categorical features

One-hot encoding (handle_unknown="ignore")

This ensures consistent transformations across Linear Regression, XGBoost, and LightGBM.

🧪 Train–Validation Strategy

Train–validation split: 80/20

Random seed fixed for reproducibility

Validation set used consistently for all model comparisons

🤖 Models Trained
1️⃣ Linear Regression (Baseline)

Acts as a strong linear benchmark

Surprisingly competitive due to strong temporal structure

Validation R² ≈ 0.46

2️⃣ XGBoost Regressor

Captures nonlinear relationships

Tuned with depth, learning rate, and subsampling

Validation R² ≈ 0.52 (best single model)

3️⃣ LightGBM Regressor

Fast tree-based learner

Good performance but sensitive to sparse features

Validation R² ≈ 0.35–0.45 (varies with tuning)

🔧 Hyperparameter Tuning

RandomizedSearchCV used for XGBoost & LightGBM

Objective: maximize validation R²

Limited iterations to avoid overfitting

Tuning helped XGBoost more than LightGBM.

🧩 Ensemble Strategy

Final predictions are produced using a weighted ensemble:

Final Prediction =
0.50 × Rule-Based
+ 0.25 × LightGBM
+ 0.25 × XGBoost

Why this works:

Rule-based model captures strong calendar regularities

XGBoost models nonlinear interactions

LightGBM adds variance reduction

Ensemble improves stability over single models

📤 Submission

Predictions are clipped to non-negative values

Rounded to integers

Output saved as submission.csv with required format

📊 Final Results (Validation)
Model	R²
Linear Regression	~0.46
XGBoost (tuned)	~0.52
LightGBM	~0.36–0.45
Ensemble	Best overall stability
✅ Key Learnings

Calendar features dominate cinema attendance prediction

Simple models can outperform complex ones when features are strong

Ensembles improve robustness under noisy real-world data

Leakage-safe feature design is critical in time-series problems
