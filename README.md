# 📈 Beyond Accuracy — Directional Trend Evaluation in Gold Price Forecasting

## 1. Overview

This project forecasts international gold prices (XAU/USD) using a multivariate framework that combines traditional statistical modelling (ARIMA) with machine learning algorithms (Random Forest, XGBoost, SVR). The dataset spans December 2020 to December 2025 — 1,259 daily observations across four financial indicators.

The core contribution is a **Directional-First Evaluation Framework**: instead of optimising purely for error minimisation (RMSE/MAE), the project measures each model's ability to correctly predict the *direction* of price movement (up vs. down). This reframes model selection around the real objective of trading systems — generating reliable Buy/Sell signals — rather than the academic proxy of numerical closeness.

---

## 2. The Forecasting Paradox

The central finding of this project can be stated plainly:

> **ARIMA achieves the lowest RMSE (1.32) but is the worst model for trading. Random Forest achieves a higher RMSE (1.80) but is selected as the deployment model.**

This is not a contradiction — it is the point.

ARIMA(2,1,2) operates on an advanced form of Naïve Forecast: tomorrow's predicted price is essentially today's price. In markets where day-to-day moves are small relative to price levels, this produces a low numerical error while systematically lagging the market by one session. Reversal points — the moments that matter most for trading decisions — are always missed. Random Forest and XGBoost, despite higher RMSE, actively learn the relationship between gold and leading macroeconomic indicators (USD Index, crude oil, S&P 500), giving them the ability to anticipate directional shifts rather than merely echo the recent past.

---

## 3. Project Pipeline

```
Raw data (XAU/USD · DXY · S&P 500 · Crude Oil)
        │
        ▼
[1] Data preprocessing
        Linear interpolation for missing values
        Lag features: Gold_Lag1, USD_Lag1, Oil_Lag1, SP500_Lag1
        Trend features: MA_3, MA_7, Vol_5, Momentum_2
        MinMaxScaler (fit on train, transform test)
        │
        ▼
[2] Statistical diagnostics
        ADF Test → gold price is non-stationary → first-order differencing
        VIF → all macro vars > 10 (retained for ML interaction signals)
        Outlier analysis → 68 IQR outliers (~5.4%) concentrated in 2022
        │
        ▼
[3] Walk-forward validation split
        Train: Dec 2020 – end 2024  (80%)
        Test:  Jan 2025 – Dec 2025  (20%)
        │
        ├──► ARIMA (2,1,2)       RMSE 1.32 · lag effect · univariate
        ├──► Random Forest        RMSE 1.80 · multivariate · ✅ selected
        ├──► XGBoost              RMSE 2.31 · high directional sensitivity
        └──► SVR (RBF)            RMSE 16.05 · fails on unseen trend range
        │
        ▼
[4] Directional-First Evaluation
        Directional Accuracy = % of sessions where predicted ΔPrice sign = actual ΔPrice sign
        Random Forest selected for best balance: RMSE + directional accuracy + interpretability
```

---

## 4. Key Findings

- **The RMSE paradox is confirmed.** ARIMA's RMSE of 1.32 is the lowest among all models, but its forecast line traces today's price as tomorrow's estimate — a Lagging Effect that makes it unsuitable as a trading signal generator.

- **Multivariate features add real signal.** Random Forest's Feature Importance chart shows that `Gold_Lag1` dominates (consistent with random walk theory), but USD Index and crude oil lags provide measurable moderating influence during macro shocks — precisely the periods when univariate ARIMA breaks down.

- **The 2022 Russia–Ukraine shock stress-tested all models.** 68 IQR outliers are concentrated in 2022–2023, when gold spiked from ~$50 range to above $120/oz and reversed sharply. This structural break degraded ARIMA significantly (its stationarity assumption collapses during regime shifts) while tree-based models partially adapted through their lag feature architecture.

- **SVR generalises poorly to new regimes.** The SVR model (RMSE 16.05) collapses in Q3–Q4 2025 because the test period contains price levels outside its training distribution — a known weakness of kernel-based regression under trend extrapolation.

---

## 5. Model Selection Rationale

Random Forest was selected over ARIMA (lower RMSE) and XGBoost (higher directional sensitivity) for three reasons:

**Breaking univariate dependency.** ARIMA knows only gold's own history. Random Forest integrates DXY, crude oil, and S&P 500 as co-predictors, transforming the system from reactive to contextually predictive.

**Stability at reversal points.** XGBoost's forecast line exhibits more pronounced overshoot at turning points than Random Forest. For a trading signal system, false reversal signals are costly — Random Forest's smoother response reduces them.

**Interpretability via Feature Importance.** Random Forest provides a ranked contribution chart for each feature. This allows a portfolio manager to understand *why* a signal was generated (e.g., a USD Index drop driving a long signal) rather than accepting a black-box output.

---

## 6. Model Performance Summary

| Model | RMSE (USD) | Notes |
|---|---|---|
| ARIMA (2,1,2) | 1.32 | Lowest error; lagging effect; univariate |
| **Random Forest** | **1.80** | **Selected for deployment** |
| XGBoost | 2.31 | Higher directional sensitivity; more noise |
| SVR (RBF) | 16.05 | Fails on out-of-range trend in 2025 |

---

## 7. Data

- Time horizon: December 2020 – December 2025 (daily frequency)
- Observations: 1,259 rows, 4 features
- Features: `Gold (USD/Ounce)`, `USD_Index`, `S&P500`, `Crude_Oil`
- Missing values: linear interpolation + forward/backward fill
- Train/test split: 80/20 by chronological order (walk-forward, no look-ahead)

---

## 8. Tech Stack

| Layer | Tools |
|---|---|
| Language | Python 3 |
| Data processing | Pandas, NumPy |
| Visualisation | Matplotlib, Seaborn |
| Preprocessing | Scikit-learn (MinMaxScaler, TimeSeriesSplit) |
| Statistical diagnostics | Statsmodels (ADF, ACF/PACF, VIF) |
| Machine learning | Scikit-learn RandomForestRegressor, XGBRegressor, SVR |
| Environment | Jupyter Notebook |

---

## 9. Development Directions

Three concrete extensions would meaningfully improve this system:

**Longer exogenous lags.** Feature Importance results suggest that USD Index and crude oil signals take more than one day to fully transmit into gold prices. Adding lags at t-3, t-5, and t-10 may surface delayed macro effects that lag-1 features miss.

**Rate-of-change features.** Using daily returns (percentage change) instead of absolute price levels for macroeconomic variables would improve stationarity and reduce the multicollinearity observed in the VIF analysis (all vars > 10 in levels).

**Higher-frequency data.** Upgrading from daily to intraday (hourly) data would capture short-term volatility regimes and improve the model's responsiveness to geopolitical shocks — the scenario where the current daily model struggles most, as demonstrated by the 2022 outlier cluster.

---

## 10. Project Structure

```
├── gold_financial_data.csv         # Raw daily data: XAU/USD, DXY, S&P500, Crude Oil
├── Time_series_data.ipynb          # Full pipeline: EDA → diagnostics → modelling → evaluation
└── README.md
```
