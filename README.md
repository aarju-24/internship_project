# Statistical Arbitrage & Pair Trading Research
## Quantitative Finance Internship Project

---

## Overview

This project builds a systematic pipeline for identifying and validating statistically related equity pairs as a foundation for pairs trading research. The pipeline progresses from raw data collection through exploratory analysis, correlation screening, and Engle-Granger cointegration validation on three candidate pairs.

The codebase is designed to scale from three validation pairs to the full stock universe by changing a single configuration list.

---

## Repository Structure

```
├── data/
│   ├── raw/
│   │   └── top100_us_stocks.csv          # Market cap ranked S&P 500 constituents
│   ├── cleaned/
│   │   └── adj_close_cleaned.csv         # Adjusted close prices, missing values handled
│   └── processed/
│       ├── returns.csv                   # Daily simple returns
│       ├── log_returns.csv               # Daily log returns
│       ├── normalized_prices.csv         # Prices indexed to 1.0 at start date
│       ├── rolling_volatility.csv        # 30-day rolling standard deviation
│       ├── sector_info.csv               # GICS sector metadata
│       └── removed_assets.csv            # Assets excluded due to data quality
│
├── results/
│   └── cointegration/
│       ├── cointegration_summary.csv     # ADF results for all candidate pairs
│       ├── spreads.csv                   # OLS residual spread time series
│       └── zscores.csv                   # Standardised spread signals
│
├── notebooks/
│   ├── 01_data_collection.ipynb          # Universe construction and data download
│   ├── 02_eda.ipynb                      # Exploratory data analysis
│   └── 03_engle_granger.ipynb            # Cointegration validation pipeline
│
├── docs/
│   ├── methodology_evolution.md          # Version history of key design decisions
│   ├── research_log.md                   # Phase-by-phase progress notes
│   └── assumptions_register.md           # Documented assumptions and limitations
│
└── README.md
```

---

## Asset Universe

**Stocks — 93 assets**
Top US large-cap companies by market capitalisation, sourced programmatically from the S&P 500 Wikipedia constituent list and ranked by `marketCap` via the Yahoo Finance API. Using market-cap ordering (rather than the default S&P 500 alphabetical or index-weight ordering) ensures the universe consistently represents the most liquid US equities.

**ETFs — 15 assets**

| Category | Tickers |
|----------|---------|
| Broad market | SPY, QQQ, DIA, IWM |
| Sector | XLF, XLK, XLE, XLV, XLY, XLP, XLI, XLU |
| Alternatives | VNQ, GLD, TLT |

ETFs are analysed separately from stocks throughout the project. Their automatic rebalancing and different risk structures make mixing the two asset classes in correlation and cointegration tests misleading.

**Data source:** Yahoo Finance adjusted close prices, 2018-01-01 to present, daily frequency.

---

## Pipeline

```
01_data_collection.ipynb
    │
    ├── Universe construction (market-cap ranked S&P 500)
    ├── Historical price download (yfinance, 2018–present)
    ├── Missing value handling (>5% threshold for removal, ffill for remainder)
    └── Output: adj_close, returns, log_returns, normalized_prices, rolling_volatility

02_eda.ipynb
    │
    ├── Price trend and normalised performance analysis
    ├── Return and volatility statistics
    ├── Outlier detection (z-score based)
    ├── Rolling volatility analysis (30-day window)
    ├── Return distribution analysis (skewness, kurtosis)
    ├── Annualised return estimation (geometric)
    ├── Stock correlation matrix (1-year window)
    ├── ETF correlation matrix (1-year window)
    ├── Yearly correlation analysis (stocks only, same-company pairs removed)
    └── Rolling correlation analysis (252-day window, full history)

03_engle_granger.ipynb
    │
    ├── OLS regression on log prices → hedge ratio estimation
    ├── Residual spread construction
    ├── ADF test on spread (Engle-Granger Step 2)
    ├── Z-score signal framework
    ├── Cross-validation via statsmodels.coint()
    ├── Final comparison table (all pairs ranked by ADF statistic)
    └── CSV export of results, spreads, and z-scores
```

---

## Candidate Pairs

Three pairs were selected from correlation screening based on high correlation, sector similarity, and rolling stability across multiple years.

| Pair | Sector | 1-Year Correlation | Economic Rationale |
|------|--------|-------------------|--------------------|
| MA / V | Financials — Payment Networks | 0.856 | Identical business models, same regulatory exposure, same transaction volume drivers |
| HD / LOW | Consumer Discretionary — Home Improvement | 0.882 | Duopoly in home improvement retail, revenues driven by the same housing cycle |
| AMAT / LRCX | Technology — Semiconductor Equipment | 0.862 | Both supply wafer fabrication equipment to the same customer base under the same spending cycle |

---

## Cointegration Methodology

**Engle-Granger two-step procedure:**

1. OLS regression of `log(Y)` on `log(X)` to estimate the hedge ratio β and extract residuals
2. ADF test (autolag=AIC) on the residuals to test for stationarity

A stationary residual spread is evidence that the two log-price series share a long-run equilibrium — meaning price deviations are temporary and mean-reverting.

**Parameters:** significance level α = 0.05, lookback window = 252 trading days, z-score entry ±2.0, exit ±0.5, stop ±3.0.

All analytical steps are implemented as reusable functions. Scaling from three pairs to the full universe requires only replacing `CANDIDATE_PAIRS` in the configuration cell. A `run_cointegration_pipeline()` batch function is included for Phase 2.

---

## Key Results

Results are produced by running `03_engle_granger.ipynb`. The final comparison table ranks all three pairs by ADF statistic. Output files are written to `results/cointegration/`.

---

## Known Limitations

| Limitation | Notes |
|-----------|-------|
| Survivorship bias | Universe uses current top market-cap companies with historical data. Discussed with mentor; retained for practical relevance. |
| Static hedge ratio | β estimated once over the full 252-day window. In live deployment, rolling re-estimation is required. |
| Static z-score normalisation | Mean and std computed over the full in-sample window. Rolling normalisation is required before any backtesting. |
| Engle-Granger critical values | The two-step procedure introduces estimation error not captured in standard ADF critical values. Borderline cases (0.03 < p < 0.05) should be treated conservatively. |
| No transaction costs | Spread and z-score analysis does not include bid-ask spread, borrowing costs, or market impact. |

---

## Setup

```bash
pip install pandas numpy matplotlib seaborn statsmodels yfinance requests beautifulsoup4
```

Run notebooks in order:
```
01_data_collection.ipynb  →  02_eda.ipynb  →  03_engle_granger.ipynb
```

---

## Project Status

| Phase | Description | Status |
|-------|-------------|--------|
| 1 | Universe construction & data collection | Complete |
| 2 | Exploratory data analysis | Complete |
| 3 | Correlation analysis & pair selection | Complete |
| 4 | Engle-Granger cointegration validation | Complete |
| 5 | Rolling cointegration & hedge ratio stability | Planned |

