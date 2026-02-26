Time Series Forecasting: Germany's Weekly Electricity Consumption
Project Overview
This project develops time series forecasting models to predict Germany's weekly electricity consumption (2006-2017) using data from Open Power System Data (OPSD). Multiple approaches including Holt-Winters, seasonal regression, ARIMA, and two-level combined forecasting were implemented to identify the optimal model for energy planning and grid management.

Best Model: Regression with Seasonality + Trailing Moving Average (RMSE: 238.9, MAPE: 1.81%)
​

Dataset Information:

Data Source
Provider: Open Power System Data (OPSD)
Time Period: 2006-2017 (624 weekly observations)
Original Format: Daily electricity consumption
Processed Format: Weekly totals (aggregated in Excel)

Data Structure

Week_Start    | Consumption (MWh)
2006-01-01    | 8477.67
2006-01-08    | 9497.16
...
2017-12-24    | 10830.53

Preprocessing
Daily → Weekly aggregation (sum by week start date)

Time series object: ts(data$Consumption, freq=52)

Train/Validation split: 2006-2013 / 2014-2017 (208 weeks)
​

Methodology
1. Exploratory Data Analysis
Visualization: Weekly consumption plot (winter peaks, summer dips)

STL Decomposition: Trend (stable), Seasonality (strong yearly), Residuals
​

ACF Analysis: Strong autocorrelation confirming predictability

Stationarity Test: AR(1) coefficient = 0.8247 (p<2.2e-16) → Not random walk
​

2. Forecasting Models
Model 1: Holt-Winters Exponential Smoothing

hw.ZZZ <- ets(train.ts, model="ZZZ")  # ANN(A) selected
hw.ZZZ.pred <- forecast(hw.ZZZ, h=208)
Metrics: RMSE 416.6, MAPE 2.97%

Model 2: Regression with Seasonality
r
train.season <- tslm(train.ts ~ season)
train.season.pred <- forecast(train.season, h=208)
Metrics: RMSE 590.7, MAPE 5.21%

Model 3: Two-Level Forecasting (Best)
r
# Level 1: Seasonal regression
elec.season <- tslm(elec.ts ~ season)

# Level 2: Trailing MA on residuals (k=4)
elec.ma.trail.res <- rollmean(elec.season$residuals, k=4, align="right")

# Combined forecast
tot.fst.2level.elec <- elec.season.pred$mean + elec.ma.trail.res.pred$mean
Metrics: RMSE 238.9, MAPE 1.81% ✅

Model 4: Seasonal ARIMA
r
arima.seas <- auto.arima(elec.ts)  # ARIMA(1,0,2)(0,1,1)[52]
arima.seas.pred <- forecast(arima.seas, h=52)
Metrics: RMSE 277.4, MAPE 2.10%

Model Performance Comparison
Model	RMSE	MAPE	Status
Holt-Winters	416.6	2.97%	❌
Regression + TMA ✅	238.9	1.81%	Best
Seasonal ARIMA	277.4	2.10%	🟢
Regression (Seasonal)	590.7	5.21%	❌
Code Highlights
Time Series Setup
r
elec.ts <- ts(data$Consumption, start=c(2006,1), end=c(2017,52), freq=52)
train.ts <- window(elec.ts, end=c(2013,52))
valid.ts <- window(elec.ts, start=c(2014,1))
STL Decomposition
r
elec.stl <- stl(elec.ts, s.window="periodic")
autoplot(elec.stl)
Accuracy Evaluation
r
accuracy(train.season.pred$mean, valid.ts)
# RMSE 238.916, MAPE 1.807
Visualizations
Time Series Plot: Full 2006-2017 consumption

STL Components: Trend/Seasonality/Residuals

ACF Plot: Autocorrelation diagnostics

Forecast vs Actual: Model validation (2014-17)

Residual Analysis: White noise confirmation
​

Requirements
r
install.packages(c("forecast", "zoo", "ggplot2"))
library(forecast)
library(zoo)
library(ggplot2)
Data: FinalProject.csv (Week_Start, Consumption columns)

Usage
r
# 1. Load data
data <- read.csv("data/FinalProject.csv")
elec.ts <- ts(data$Consumption, freq=52)

# 2. Run complete analysis
source("notebooks/FinalProject.R")

# 3. View results in console + plots
Key Findings
Strong weekly seasonality: Winter peaks ~10,500 MWh, summer ~8,000 MWh

Predictable patterns: Confirmed non-random walk (AR1 p<0.05)

Two-level superior: Captures both seasonality (80%) + short-term noise (20%)

Production ready: Reproducible, validated, business-applicable

Applications
Energy Grid Planning: Weekly demand forecasting

Renewable Integration: Balancing intermittent sources

Policy Analysis: Energiewende impact assessment

Capacity Planning: Infrastructure investment decisions
​

Future Work
Weather covariates (temperature, holidays)

Exogenous variables (GDP, industrial production)

Daily granularity modeling

Real-time dashboard implementation
