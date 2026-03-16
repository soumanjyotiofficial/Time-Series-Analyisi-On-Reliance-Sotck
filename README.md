# Time Series Analysis on Reliance Stock

## Project Overview
This project analyzes the statistical properties of **Reliance Industries Ltd. daily stock returns** using time series techniques.

The objective is to understand whether the return series is **stationary** and whether it shows **serial dependence on past values**.

---

## Research Questions

1. Do Reliance stock returns exhibit **stationarity**?
2. Is there **serial dependence** in the return series?

---

# Methodology

The analysis follows a structured **time-series workflow**.

1. Collect historical daily stock prices.
2. Convert price data into **return series**.
3. Test stationarity using:
   - Augmented Dickey–Fuller Test (ADF)
   - Kwiatkowski–Phillips–Schmidt–Shin Test (KPSS)
4. Examine serial dependence using:
   - Autocorrelation Function (ACF)
   - Partial Autocorrelation Function (PACF)

---

# Data Collection

Historical data was collected using **Yahoo Finance** through the `yfinance` library.

```python
import yfinance as yf
import pandas as pd
import warnings
import matplotlib.pyplot as plt

warnings.filterwarnings("ignore")

ticker_name = "RELIANCE"

data = yf.download(
    tickers=f"{ticker_name}.NS",
    start="2013-01-01",
    end="2025-12-31",
    progress=False
)
```

Selecting the closing price series:

```python
Closing_price = data["Close"][f'{ticker_name}.NS'].tail(int(len(data)*.5))

Closing_price.index = pd.to_datetime(Closing_price.index)
```

---

# Exploratory Data Analysis

## Classical Decomposition

The time series was decomposed into:

- Trend
- Seasonal
- Residual components

```python
from statsmodels.tsa.seasonal import seasonal_decompose

decompose = seasonal_decompose(Closing_price, model="additive", period=25)

trend = decompose.trend
seasonal = decompose.seasonal
residual = decompose.resid
```

---

# Return Series Transformation

Log returns were computed from the closing price series.

```python
import numpy as np

percentage_change = np.log(Closing_price / Closing_price.shift(1)).dropna()
```

---

# Stationarity Tests

Two statistical tests were applied to check stationarity.

## 1. Augmented Dickey–Fuller Test (ADF)

**Hypothesis**

- H₀: Series has a unit root (non-stationary)
- H₁: Series is stationary

**Result**

- ADF Statistic: **-11.799**
- p-value: **0.0**

**Conclusion**

The null hypothesis is rejected, indicating the series is **stationary**.

---

## 2. KPSS Test

**Hypothesis**

- H₀: Series is stationary
- H₁: Series is non-stationary

**Result**

- KPSS Statistic: **0.072**
- p-value: **0.1**

**Conclusion**

Fail to reject the null hypothesis, indicating the series is **stationary**.

---

# Autocorrelation Analysis

To check for serial dependence, the following plots were generated:

- **Autocorrelation Function (ACF)**
- **Partial Autocorrelation Function (PACF)**

```python
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf

plot_acf(percentage_change, lags=30)
plot_pacf(percentage_change, lags=30)
```

**Result**

No statistically significant autocorrelation was observed at different lags.

---

# Key Findings

- The **return series is stationary** based on both ADF and KPSS tests.
- The **ACF and PACF plots show no significant autocorrelation**.
- Reliance stock returns behave like a **white noise process**, meaning past returns do not significantly influence future returns.

---

# Conclusion

The time series analysis shows that the **Reliance return series is stationary and does not exhibit serial dependence**.

This implies that past returns do not provide significant predictive information for future returns, which is consistent with the **weak form of market efficiency**.

---

# Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Statsmodels
- yFinance

---

# Author

**Souman Jyoti**

Finance Professional | Python for Finance | Financial Data Analysis
