# Quantitative Equity Research: Risk, Returns, and Trading Strategies

A self-contained quantitative research project covering the full pipeline from raw price data to portfolio optimization, tail risk measurement, and systematic trading strategy evaluation. All analysis is implemented in Python using publicly available market data.

---

## Project Structure

```
quant_equity_analysis.ipynb   ← Main notebook (run top-to-bottom)
README.md                     ← This file
monthly_returns_2010_2025.csv ← Generated on first run
MSFT_prices.csv               ← Generated on first run
```

---

## Modules

### Module 1 · Return Mechanics & Probabilistic Price Modeling
Establishes the mathematical foundations of return analysis. Covers simple vs. log return calculation, multi-period aggregation under the i.i.d. lognormal model, probability of breaching price thresholds, and implied long-run geometric growth rates.

### Module 2 · Empirical Distribution Analysis
Tests the normality assumption for Microsoft (MSFT) daily log returns (2022–2024) using:
- Empirical CDF vs. fitted Normal CDF
- Kernel Density Estimation vs. Normal PDF
- Quantile-Quantile (QQ) plot
- Skewness and kurtosis hypothesis tests
- Jarque-Bera joint normality test
- Student-*t* MLE for heavy-tail modeling

### Module 3 · Bootstrap Inference for Return Statistics
Uses nonparametric bootstrap resampling on Apple (AAPL) weekly log returns (2010–2024) to estimate standard errors, bias, and MSE for the sample mean, variance, and standard deviation — with comparison against analytic asymptotic formulas.

### Module 4 · Multi-Asset Portfolio Construction
Constructs and compares three canonical portfolios across AXP, CAT, and SBUX (1999–2024):
- **Equally Weighted (EW)**
- **Global Minimum Variance (GMV)**
- **Tangency (Maximum Sharpe Ratio)**

Includes log-price autoregression and mean return hypothesis tests.

### Module 5 · Value-at-Risk & Expected Shortfall
Estimates tail risk for a $100,000 MSFT position at the 1% significance level using three methods: historical simulation, parametric Normal, and parametric Student-*t*. Bootstrap 95% confidence intervals quantify estimation uncertainty. Power-law tail regression extrapolates estimates to the 0.1% extreme quantile.

### Module 6 · Technical Trading Strategies
Evaluates systematic rules on the S&P 500 (2010–present):
- **Moving-average momentum and contrarian** strategies across bandwidths *k* ∈ {5, 10, 20, 50, 100, 200}
- **Bollinger Band momentum and contrarian** strategies using ±2σ band crossings

All signals are lagged one day to prevent look-ahead bias. Performance is assessed on annualized Sharpe ratio and expected daily log return.

---

## Key Findings

| Module | Result |
|--------|--------|
| Return Mechanics | Log returns aggregate linearly; lognormality yields closed-form threshold probabilities |
| Distribution Analysis | MSFT returns exhibit significant excess kurtosis; Student-*t* fits substantially better than Gaussian |
| Bootstrap Inference | Bootstrap and analytic SEs align closely for the mean; bootstrap SE for variance is larger, reflecting fat tails |
| Portfolio Construction | Tangency portfolio maximizes risk-adjusted return; GMV minimizes volatility at the cost of lower expected return |
| Risk Measurement | Normal VaR/ES underestimates tail risk; Student-*t* estimates are more conservative and realistic |
| Trading Strategies | Short-horizon MA momentum (*k* ≤ 20) dominates on Sharpe ratio; price continuation outperforms mean reversion at daily frequencies |

---

## Data

All data is downloaded automatically via `yfinance` on first run. No manual data files are required.

| Asset(s) | Frequency | Period |
|----------|-----------|--------|
| MSFT | Daily | 2012–2024 (risk), 2022–2024 (distribution) |
| AAPL | Weekly | 2010–2024 |
| AXP, CAT, SBUX | Daily | 1999–2024 |
| BA, MSFT, DAL, SBUX | Monthly | 2010–2025 |
| S&P 500 (^GSPC) | Daily | 2010–present |

---

## Requirements

```
python >= 3.10
numpy
pandas
matplotlib
yfinance
scipy
statsmodels
```

Install all dependencies:

```bash
pip install numpy pandas matplotlib yfinance scipy statsmodels
```

---

## Usage

```bash
# Clone or download the repository, then launch Jupyter
jupyter notebook quant_equity_analysis.ipynb
```

Run all cells sequentially (`Kernel → Restart & Run All`). Internet access is required on first run to download price data. Subsequent runs will use the cached CSV files generated automatically.

---

## Notes

- Bootstrap sections use `np.random.seed(432)` for reproducibility.
- VaR and ES figures are expressed as dollar losses on a $100,000 initial position.
- All trading strategy signals are lagged by one trading day to eliminate look-ahead bias.
- The `yfinance` API may occasionally rate-limit downloads; re-running the affected cell is sufficient.
