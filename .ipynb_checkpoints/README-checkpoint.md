## **Beyond Accuracy: Evaluating Directional Trends vs. Prediction Errors in Gold Market Forecasting**

**📌 1. Project Overview**
The project focuses on forecasting international gold prices (XAU/USD) using both traditional statistical models (ARIMA) and modern machine learning techniques, including XGBoost and Random Forest.

The core differentiation of this project lies in challenging the conventional assumption that “lower error equals a better model” in financial forecasting. Instead of solely optimizing traditional metrics such as RMSE or MAE, the project introduces a Directional-First evaluation framework that prioritizes the model’s ability to correctly capture price direction (Directional Accuracy).

By emphasizing trend prediction (upward/downward movements) rather than just minimizing numerical error, the framework better aligns model performance with real-world trading decisions (Buy/Sell), thereby increasing its practical applicability in financial markets.

**2. Business Context**

**Industry Reality**
- In financial investment, the objective is not merely to estimate the future price level, but more importantly to correctly identify the market direction. For traders and portfolio managers, directional accuracy often has greater practical value than small numerical forecasting errors.

**Strategic Paradox**
- Models with low forecasting errors (such as ARIMA) often suffer from a lagging effect. Since these models heavily rely on recent price patterns, they tend to predict tomorrow’s price very close to today’s price. As a result, they may miss critical turning points and generate delayed trading signals, reducing their effectiveness in real-world trading decisions.

**Macroeconomic Drivers**
Gold prices are modeled using a multivariate framework incorporating key macroeconomic indicators:

- **USD Index (DXY):** The currency benchmark in which gold is denominated.

- **Crude Oil Prices:** A proxy for inflation expectations.

-**S&P 500 Index:** A measure of market risk appetite and capital allocation behavior.

**🎯 3. Project Objectives**

- Analyze the multivariate correlation between gold and leading economic indicators.

- Perform time-series diagnostics, including: Stationarity testing (ADF Test), Multicollinearity detection (Variance Inflation Factor – VIF), Outlier treatment caused by geopolitical shocks (e.g., the Russia–Ukraine conflict in 2022)

- Compare the performance of ARIMA, XGBoost, and Random Forest based on both: Statistical error metrics (RMSE, MAE), Practical trading effectiveness (Directional Accuracy).

**📂 4. Data Structure & Preparation**

- Time Horizon: Market data from December 2020 to December 2025.

- Features: Gold price (XAU/USD), USD Index (DXY), S&P 500 Index, Crude oil prices, Lag features of both target and exogenous variables.

- Preprocessing Techniques: Linear interpolation for handling missing values, Walk-forward validation to simulate real trading conditions and avoid look-ahead bias.

**🔍 5. Key Findings**

- **Forecasting Paradox:**: ARIMA achieved a lower RMSE (~1.3), but demonstrated weaker sensitivity to trend reversals compared to XGBoost (RMSE ~2.3). This confirms that lower numerical error does not necessarily translate into better trading performance.

- **Role of Random Forest:**
Random Forest functioned as a multi-layer forecasting system: Lagged price features acted as the structural backbone, Macroeconomic variables (USD, oil) served as moderating factors, This combination helped mitigate risk during sudden market shocks and regime shifts.

**💡6. Development Direction**

Enhance model performance by incorporating additional lag structures for exogenous variables.

Investigate macroeconomic impact using rate of change (returns) instead of absolute price levels.

Upgrade the dataset to higher frequency (intraday/hourly) to capture short-term volatility and improve responsiveness to market movements.
**🛠 Tools & Technologies**

- Python (Pandas, NumPy)

- Matplotlib & Seaborn

- MinMaxScaler