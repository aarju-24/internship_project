# Research Log

---

## Phase 1 — Project Planning and Universe Construction
**18 May – 24 May 2026**

Defined project scope and built the asset universe. Initial manual stock selection was replaced with a systematic market-cap ranked extraction from the S&P 500 Wikipedia constituent table. Survivorship bias concerns were raised regarding the use of current market leaders with historical data. After mentor review, the decision was made to retain the current universe, document the limitation, and focus on methodology correctness.

**Outcome:** Final universe established — 93 stocks, 15 ETFs.

---

## Phase 2 — Data Collection and Preprocessing
**24 May – 30 May 2026**

Downloaded adjusted close prices via `yfinance` for all 108 assets from 2018-01-01 to present. Handled the Yahoo Finance multi-ticker column structure, patched missing AXP and BRK-B data (these were not included correctly in the batch download and required individual downloads). Applied a 5% missing-value threshold to remove sparse assets, followed by forward-fill for remaining gaps. Generated derived datasets: daily returns, log returns, normalised prices, and 30-day rolling volatility. Sector metadata extracted from S&P 500 Wikipedia table.

**Outcome:** Clean dataset saved to `data/cleaned/` and `data/processed/`.

---

## Phase 3 — Exploratory Data Analysis
**30 May – 03 June 2026**

Analysed return behaviour, volatility, and distributional properties across the full universe. Key findings: return distributions are non-normal with heavy tails; technology and growth stocks exhibit meaningfully higher volatility than defensive sectors; several assets show extreme single-day observations consistent with earnings events. Geometric annualised returns were computed to avoid the overstatement inherent in arithmetic annualisation.

**Outcome:** Statistical baseline established for the full universe before pair selection.

---

## Phase 4 — Correlation Analysis and Pair Selection
**03 June – 04 June 2026**

Computed 1-year Pearson correlation matrices for stocks and ETFs separately. Top stock pairs by correlation: HD/LOW (0.882), AMAT/LRCX (0.862), MA/V (0.856). Same-company pairs (GOOG/GOOGL and similar) were found to dominate the rankings; a filtering function was implemented to remove them using a hard-coded blocklist and secondary name-matching via Yahoo Finance `longName`. Mentor feedback confirmed that correlation alone is insufficient — stability analysis required before candidate selection.

**Outcome:** Candidate pairs shortlisted pending rolling stability confirmation.

---

## Phase 5 — Correlation Stability Analysis
**04 June 2026**

Added yearly and rolling correlation analysis. Yearly loop showed that AMAT/LRCX, MA/V, and HD/LOW appeared consistently in the top pairs across multiple calendar years. Rolling 252-day analysis confirmed that these relationships, while not constant, remain above average correlation throughout the full sample with no extended periods of breakdown. An implementation error was identified and corrected: the rolling analysis was initially applied to a 252-row subset with a 252-day window, producing only a single valid observation. Moving the rolling analysis to the full history resolved this.

**Outcome:** Three pairs confirmed as candidates for cointegration testing.

---

## Phase 6 — Engle-Granger Cointegration Validation
**Current**

Implementing the Engle-Granger two-step procedure on the three candidate pairs. OLS regression on log prices produces the hedge ratio and residual spread. ADF test (autolag=AIC) on the spread tests for stationarity. Results are cross-validated against `statsmodels.coint()`. All analytical logic is encapsulated in reusable functions. A batch runner (`run_cointegration_pipeline()`) is ready for Phase 7 scaling.

**Status:** In progress.

---

## Next Steps

1. Complete cointegration validation on all three pairs
2. Confirm implementation correctness via `coint()` cross-check
3. Apply batch pipeline to full EDA top-pairs list (Phase 7)
4. Develop rolling cointegration framework (rolling OLS, hedge ratio stability)
5. Build signal generation with rolling z-score normalisation
6. Evaluate pair suitability for backtesting
