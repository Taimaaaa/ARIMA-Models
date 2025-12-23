# ARIMA Time Series Forecasting

## Objective
The goal of this assignment is to build, evaluate, and compare ARIMA-based time series models to forecast **Adjusted Close** stock prices.  
Using historical data from **2010–2020**, the task is to predict prices for the **next quarter (≈ 65 business days)** and select a final model based on both quantitative metrics and visual performance.

---

## Data Preparation
- Source data contains a `Date` column and `Adj Close` prices.
- Data filtered to the range **2010–01–01 to 2020–12–31**.
- `Date` converted to a **business-day (`'B'`) datetime index**.
- Missing business days introduced by resampling were handled appropriately.
- Final time series used: **Adjusted Close (business-day frequency)**.

---

## Stationarity Analysis
- Stationarity is required for ARIMA modeling.
- The Augmented Dickey-Fuller (ADF) test was applied.
- The raw series was **non-stationary**.
- First differencing was sufficient to achieve stationarity:
  - **d = 1**
- This was confirmed via both:
  - ADF test results
  - Visual inspection of the differenced series

---

## Model Order Selection
- ACF and PACF plots of the stationary series were analyzed.
- Results indicated weak short-term autocorrelation.
- This suggested starting with:
  - A simple random-walk-style baseline model
- Initial and candidate orders explored:
  - ARIMA(0,1,0)
  - ARIMA(1,1,1)
  - ARIMA(2,1,1)
  - ARIMA(1,1,0)
  - ARIMA(0,1,1)

---

## Train–Test Split
- The time series was split **chronologically**.
- Training set: data from 2010 up to the start of the final quarter.
- Test set: **~65 business days** (next quarter).
- This preserves temporal order and avoids data leakage.

---

## Model Fitting
- Models were fit using `statsmodels.tsa.ARIMA`.
- Each model used:
  - The same training data
  - Identical test horizon
- Forecasts were generated using:
  - `get_forecast(steps=len(test))`
  - Confidence intervals were retained for interpretation.

---

## Model Evaluation
Models were evaluated using the following metrics:
- Mean Absolute Error (MAE)
- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)
- R²
- Mean Absolute Percentage Error (MAPE)

### Key Results (Test Set)
- **ARIMA(0,1,0)** achieved the lowest error:
  - **MAPE ≈ 2.46%**
- More complex models did not improve performance.
- All ARIMA variants produced negative R² values, which is common for financial time series.

---

## Visual Forecast Assessment
- Forecasts were plotted against test data.
- ARIMA(0,1,0):
  - Produced smooth, stable forecasts.
  - Closely followed the level of the test data.
- More complex models:
  - Slightly more reactive
  - No visible improvement in tracking actual prices
- Volatility was not captured by any model, which is expected for ARIMA applied to stock prices.

---

## Final Model Selection
**Chosen Model:** **ARIMA(0,1,0)**

### Justification
- Lowest MAPE among all tested models.
- Simpler structure with no unnecessary parameters.
- Consistent with financial theory:
  - Stock prices over short horizons behave like a random walk.
- Additional AR/MA terms added complexity without improving accuracy.

---

## Conclusions
- ARIMA models are effective for **short-term trend continuation**, not precise price prediction.
- A simple random-walk baseline can outperform more complex ARIMA configurations.
- A MAPE of **≈ 2.5%** is strong performance for stock price forecasting.
- Further improvements would require:
  - Exogenous variables (ARIMAX)
  - Volatility models (e.g., GARCH)
  - Or shorter prediction horizons

---

## Key Takeaway
For short-horizon stock forecasting, **model simplicity + disciplined evaluation** is more effective than increasing model complexity.
