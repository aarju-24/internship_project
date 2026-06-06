# Assumptions Register

## Project Scope Assumptions

### Research Objective

The project focuses on identifying statistically related asset pairs using correlation analysis, Engle-Granger cointegration, and rolling cointegration as a foundation for future pair trading research.

---

# Asset Universe Assumptions

## Stock Universe

* Current top 100 US large-cap companies by market capitalization.

## ETF Universe

The ETF universe contains:

* Broad market ETFs
* Sector ETFs
* Commodity ETFs
* Bond ETFs

Examples:

* SPY
* QQQ
* DIA
* IWM
* XLK
* XLF
* XLE
* XLV
* GLD
* SLV
* TLT

Total ETFs analyzed: 15.

ETFs provide useful benchmarks and relationship comparisons against individual stocks.

---

# Data Assumptions

## Data Source

Yahoo Finance

---

## Price Series

Adjusted close prices are used throughout the project.

### Assumption

Adjusted prices better represent actual investor returns because they account for:

* Stock splits
* Dividends
* Corporate actions

---

## Frequency

Daily observations.

### Assumption

Daily frequency captures medium- and long-term relationships while reducing market microstructure noise.

---

## Historical Window

Primary dataset:

2018 – Present

### Assumption

A multi-year period captures different market regimes and economic environments.

---

# Return Assumptions

## Simple Returns

Used for:

* Correlation analysis
* Volatility calculations

---

## Log Returns
Used for:

* Distribution analysis
* Statistical diagnostics

### Assumption

Log returns possess desirable statistical properties for financial analysis.

---

# Volatility Assumptions

## Annualized Volatility

Annualized Volatility = Daily Standard Deviation × √252

### Assumption

A trading year contains approximately 252 trading days.

---

# Correlation Assumptions

## Correlation Measure

Pearson Correlation Coefficient

### Assumption

Linear correlation is an appropriate first-stage screening tool for identifying candidate pairs.

### Limitation

Correlation alone does not imply:

* Cointegration
* Causality
* Mean reversion

---

## Pair Selection Assumption

Candidate pairs should demonstrate:

* High correlation
* Similar industry exposure
* Similar business models
* Stable relationships through time

---

# Rolling Correlation Assumptions

## Rolling Window

Primary window:

252 trading days

Future candidate windows:

* 30 days
* 60 days
* 90 days

### Assumption

Rolling windows capture changing relationships across market regimes.

---

# Cointegration Assumptions

## Methodology

Engle-Granger Two-Step Procedure

Process:

1. OLS Regression
2. Residual Construction
3. ADF Test on Residuals

### Assumption

Cointegrated assets exhibit a long-run equilibrium relationship.

---

## Candidate Pair Selection

Current validation pairs:

* MA – V
* HD – LOW
* AMAT – LRCX

### Assumption

Economically related businesses are more likely to exhibit stable long-term relationships.

---

# Stationarity Assumptions

## Statistical Test

Augmented Dickey-Fuller (ADF) Test

### Significance Level

α = 0.05

### Decision Rule

p-value < 0.05

### Assumption

Stationary residuals indicate evidence of cointegration.

---

# Future Trading Framework Assumptions

## Mean Reversion

Assumption:

Cointegrated spreads may revert toward a long-run equilibrium.

---

## Signal Thresholds

Planned parameters:

* Entry Z-score = 2.0
* Exit Z-score = 0.5
* Stop Z-score = 3.0

### Assumption

Large deviations from equilibrium may present trading opportunities.

---

# Known Constraints

The project currently does not include:

* Transaction costs
* Slippage
* Market impact
* Short-sale constraints
* Borrowing costs

These factors will be considered only if backtesting is performed.

---

# Summary

The methodology assumes that highly correlated and economically related assets may exhibit stable long-run relationships. Correlation analysis is used as a screening stage, while Engle-Granger cointegration testing serves as the primary statistical validation framework before rolling cointegration and future strategy development.
