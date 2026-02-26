🇩🇪 Germany Weekly Electricity Forecasting
Time Series Analytics Project
📊 Dataset

Source: Open Power System Data (OPSD)
Period: 2006–2017
Frequency: Weekly (Aggregated from daily)
Target: Electricity Consumption (MWh)

🔎 Exploratory Insights

Strong yearly seasonality (winter peaks • summer dips)
Stable long-term trend
Significant autocorrelation
Series confirmed not a random walk

🧠 Models Implemented

Holt-Winters (ETS – ANN)
RMSE: 416.649 • MAPE: 2.966%

Holt-Winters + Trailing MA
RMSE: 384.183 • MAPE: 2.71%

Regression (Seasonal Dummies)
RMSE: 590.719 • MAPE: 5.212%

Regression + Trailing MA (Best Model) 🏆
RMSE: 238.916 • MAPE: 1.807%

Seasonal ARIMA
Model: ARIMA(1,0,2)(0,1,1)[52]
RMSE: 277.4 • MAPE: 2.104%

📈 Model Comparison (Quick View)

Holt-Winters → 416.649 RMSE
Holt-Winters + MA → 384.183 RMSE
Regression → 590.719 RMSE
Regression + MA → 238.916 RMSE (Best)
ARIMA → 277.4 RMSE

🔬 Validation Strategy

Training: 2006–2013
Validation: 2014–2017

Metrics Used:
RMSE • MAPE • MAE • Residual ACF Diagnostics • Theil’s U

🛠 Technical Implementation

Language: R

Libraries:
forecast • zoo • ggplot2 • stats

Time Series Setup:
Weekly frequency = 52 • Structured as ts object

💡 Key Insights

Strong seasonality dominates electricity demand
Hybrid models outperform standalone approaches
Residual modeling significantly improves forecast accuracy
Explicit seasonal handling is critical for weekly data

🔄 Production Recommendation

Re-train semi-annually • Update with latest data • Monitor residual diagnostics • Adjust for structural energy policy changes

Author

Amisha Farhana Shaik
MS in Business Analytics
California State University, East Bay

Interested in analytics, operations, forecasting, and data-driven strategy.

Looks clean on mobile

Feels more “GitHub project” than “class report”
