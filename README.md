🇩🇪 Germany Weekly Electricity Forecasting
Time Series Analytics Project (BAN 673)
📌 Project Overview

This project develops and compares multiple time series forecasting models to predict weekly electricity consumption in Germany (2006–2017).

Accurate forecasting supports:

⚡ Grid stability

🌱 Renewable energy integration

🏗 Infrastructure planning

📊 Policy decision-making

The objective was to identify the most reliable model that captures both:

Strong yearly seasonality (52-week cycle)

Short-term consumption fluctuations

📊 Dataset

Source: Open Power System Data (OPSD)
Time Period: 2006–2017
Frequency: Daily data aggregated to weekly totals
Target Variable: Weekly electricity consumption (MWh)

Data Structure
Column	Description
Week Start	Start date of each week
Consumption	Total electricity consumed that week
🔎 Exploratory Analysis

Key observations:

Clear yearly seasonal pattern (winter peaks, summer dips)

No strong long-term upward or downward trend

Significant autocorrelation across lags

Series is not a random walk (AR(1) ≠ 1, p < 0.05)

Time series decomposition revealed:

✔ Strong seasonal component

✔ Stable underlying trend

✔ Minimal unexplained residual structure

🧠 Models Implemented
1️⃣ Holt-Winters (ETS – ANN)

Automatic model selection using:

ets(train.ts, model = "ZZZ")

Issue: Could not capture weekly seasonality (frequency = 52 limitation)

Metric	Value
RMSE	416.649
MAPE	2.966%
2️⃣ Holt-Winters + Trailing Moving Average

Improved short-term correction by modeling residuals.

Improvement: Better responsiveness to fluctuations.

Metric	Value
RMSE	384.183
MAPE	2.71%
3️⃣ Regression with Seasonal Dummies

Modeled weekly seasonality explicitly using 52 dummy variables:

tslm(train.ts ~ season)
Metric	Value
RMSE	590.719
MAPE	5.212%

Captured seasonal structure but struggled with short-term volatility.

4️⃣ Regression + Trailing Moving Average (Best Model) 🏆

Two-Level Forecasting Approach:

Seasonal regression baseline

Trailing Moving Average (window = 4) on residuals

Final Forecast:

Combined Forecast = Regression Prediction + MA Adjustment
Metric	Value
RMSE	238.916
MAPE	1.807%

✅ Lowest forecasting error
✅ Captures seasonality + short-term shocks
✅ Most production-ready model

5️⃣ Seasonal ARIMA

Identified via:

auto.arima(train.ts)

Selected Model:

ARIMA(1,0,2)(0,1,1)[52] with drift
Metric	Value
RMSE	277.4
MAPE	2.104%

Strong performance but slightly underestimated winter peaks.

📈 Model Comparison
Model	RMSE	MAPE (%)
Holt-Winters	416.649	2.966
Holt-Winters + TMA	384.183	2.71
Regression (Seasonal)	590.719	5.212
Regression + TMA ⭐	238.916	1.807
Seasonal ARIMA	277.4	2.104
🏆 Final Recommendation

Regression with Seasonality + Trailing Moving Average

Why?

Explicit seasonal modeling

Residual correction improves precision

Lowest validation error

Stable residual diagnostics

🔬 Validation Strategy

Training Set: 2006–2013

Validation Set: 2014–2017

Evaluation Metrics:

RMSE

MAPE

MAE

Residual ACF diagnostics

Theil’s U statistic

🛠 Technical Implementation

Language: R

Libraries Used:

forecast

zoo

ggplot2

stats

Time Series Setup:

Weekly frequency: 52

Structured as ts object

💡 Key Insights

Strong seasonality dominates electricity demand

Hybrid models outperform single-method approaches

Residual modeling significantly improves accuracy

Explicit seasonal handling is critical for weekly data

🔄 Production Recommendations

To maintain forecasting reliability:

Re-train model every 6 months

Incorporate latest consumption data

Monitor residual diagnostics

Adjust for structural policy changes

👩‍💻 Author

Amisha Farhana Shaik
MS in Business Analytics
California State University, East Bay

Interested in analytics, operations, forecasting, and data-driven strategy.
