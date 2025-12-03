Daily Public Transport Passenger Forecasting Using Facebook Prophet

📌 1. Project Overview

This project focuses on forecasting daily public transport passenger demand using the Prophet model. The dataset contains daily passenger counts across multiple service types (Local Route, Light Rail, Rapid Route, Peak Service, School, Other).
The goal is to build a forecasting model that helps transport authorities predict demand and improve planning.

📌 2. Dataset Summary

Columns included:

Date

Local Route

Light Rail

Peak Service

Rapid Route

School

Other

A new column Total was created to represent overall daily passengers.

📌 3. Data Pre-Processing

Key cleaning steps:

Missing values in “Other” replaced with column mean

Invalid zeros replaced with mean of non-zero values

Converted “Date” to datetime format

Set “Date” as index and sorted

Created Total = sum of all service columns

📌 4. Exploratory Data Analysis

Observations from the Total passenger plot:

Weekly demand cycles

Monthly and yearly variations

Peaks during holidays

School-day vs non-school-day variations

These patterns confirmed that Prophet is suitable.

📌 5. Model Used — Prophet
```
Prophet was chosen because it:
✔ Handles daily data well
✔ Automatically detects seasonality
✔ Manages missing/irregular data
✔ Is easy to interpret

Model configuration:
model = Prophet(
    weekly_seasonality=True,
    yearly_seasonality=True,
    daily_seasonality=False,
    changepoint_prior_scale=0.8,
    seasonality_prior_scale=2.5
)
```
📌 6. Train–Test Split

Last 30 days → Test

Remaining → Train

Columns renamed for Prophet:

ds → Date

y → Target (Total passengers)

📌 7. Forecasting & Evaluation

30-day future dataframe created

Predictions aligned with test dates

Evaluation metrics used:

MAE (Mean Absolute Error)

RMSE (Root Mean Squared Error)

Prophet captured the overall weekly trend but struggled with the high variability in the total passenger data. Forecasts for the next 7 days were generated.

📌 8. Key Findings

Prophet models weekly patterns well

Total passenger count is harder to predict because it mixes different behaviours

Better accuracy is possible by forecasting each service type separately

📌 9. Conclusion

This project demonstrates a complete, end-to-end workflow for time-series forecasting using Prophet. It includes data cleaning, EDA, model building, forecasting, and evaluation.
Prophet proved effective for detecting seasonal behaviour in public transport usage and can support scheduling, resource allocation, and operational planning.
