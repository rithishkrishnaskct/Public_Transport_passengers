# Public_Transport_passengers
DAILY PUBLIC TRANSPORT PASSENGER FORECASTING USING PROPHET MODEL

1. Introduction

Public transport systems generate large amounts of operational data every day. Analysing this data helps transport authorities understand passenger demand, plan schedules, allocate resources, and reduce congestion.
In this project, I worked with a dataset containing daily public transport passenger journeys for multiple service types such as Local Route, Light Rail, Peak Service, Rapid Route, School Service, and Other categories.

The main objective of the study was to build a forecasting model that can predict the number of passengers for the upcoming days. I selected Facebook Prophet, a time-series forecasting model developed by Meta, because it works well with daily data, seasonal patterns, and missing or irregular values.

2. Dataset Description

The dataset includes the following fields:

Date: The day of transport operation

Local Route

Light Rail

Peak Service

Rapid Route

School

Other
Each column represents the total number of passengers using that service type on that particular day.

Since each service type has different operational patterns, the dataset behaves as a multi-seasonal time-series. Understanding and preparing the dataset correctly is important before building a forecasting model.

3. Data Pre-Processing Steps

Before applying Prophet, several cleaning and preparation steps were performed:

3.1 Handling Missing Values

Some records in the Other service category had missing values. These were replaced using the mean of that column to maintain the overall trend without introducing bias.

3.2 Replacing Zero Values

Certain service columns had zeros which were not actual zero passengers but recording gaps. I replaced all zeros with the mean of the non-zero values in each column. This avoids misleading the model with invalid zeros.

3.3 Converting Date Column

The Date column was converted to datetime format using:

df['Date'] = pd.to_datetime(df['Date'], dayfirst=True)


This is important because Prophet requires the date column to be in proper datetime format.

3.4 Setting Date as Index

After conversion, the Date column was set as the index and sorted in chronological order. This prepares the data for proper time-series modelling.

3.5 Creating Total Passenger Column

A new column Total was created by summing all service categories:

df['Total'] = df[cols].sum(axis=1)


This represents the overall passenger count for each day and is useful for trend analysis.

4. Exploratory Data Analysis (EDA)

The Total passenger values were plotted over time. This helped to observe:

Rising and falling trends

Seasonal weekly and monthly patterns

Sudden peaks during holidays

Variations due to school terms and workdays

The time-series was irregular and multi-seasonal, confirming that Prophet is a suitable model.

5. Model Selection and Justification

I selected the Prophet model for forecasting because:

It automatically handles daily, weekly, and yearly seasonal patterns

It allows adding custom seasonalities

It works well even when data has missing values

It is simple to train and interpret

It generates trend, seasonality, and forecast components

Prophet is widely used for business forecasting in areas like traffic, sales, and resource planning.

6. Train–Test Split

To evaluate model performance, the last 30 days were kept as the test set, and the remaining data was used for training.

train = df.iloc[:-30]
test  = df.iloc[-30:]


Prophet requires the column names:

ds – date

y – target value

So the training and testing DataFrames were created accordingly.

7. Prophet Model Training

A tuned version of Prophet was used with:

Weekly seasonality

Yearly seasonality

No daily seasonality

Moderate trend flexibility using changepoint_prior_scale

Additional custom seasonalities if required

model = Prophet(
    weekly_seasonality=True,
    yearly_seasonality=True,
    daily_seasonality=False,
    changepoint_prior_scale=0.8,
    seasonality_prior_scale=2.5
)


The model was then fitted on the training set.

8. Forecasting and Evaluation

A future dataframe for 30 days was created, and predictions were generated.

To correctly evaluate the model, predicted values were aligned by date with the test set. Evaluation metrics used were:

Mean Absolute Error (MAE)
Root Mean Squared Error (RMSE)

The model initially showed a higher error when forecasting the total passengers directly because the total combines many different patterns. However, the methodology is correct for demonstrating Prophet forecasting.

9. Key Findings

Prophet successfully captured general patterns in the dataset such as weekly passenger cycles.

The model performed reasonably but showed limitations when predicting the total passenger count directly due to high variability.

Forecasts for the next 7 days were generated using the fitted model.

For highest accuracy, forecasting should be done separately for each service category, but demonstrating the total forecast is sufficient for this project.

10. Conclusion

This project demonstrates a complete workflow for forecasting daily public transport demand using Prophet. The steps included cleaning raw data, handling missing and zero values, performing EDA, preparing time-series data, splitting into train and test sets, training the Prophet model, and evaluating the results.

Prophet proved to be a simple and interpretable tool that captures important seasonal effects in transportation data. The generated forecasts can help transport agencies in planning bus frequency, allocating resources, predicting peak demand, and improving overall service efficiency.

The project shows that time-series forecasting is a powerful technique for solving real-world operational problems in the public transport sector.
