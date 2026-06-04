# Research Log

## Phase 1: Project Planning and Universe Selection

### Date

18 May 2026 – 24 May 2026

### Objective

Define project scope and construct a suitable asset universe for correlation, cointegration, and pair trading research.

### Work Completed

* Explored pair trading and statistical arbitrage concepts.
* Investigated correlation and cointegration methodologies.
* Evaluated stock universe construction approaches.
* Collected historical market data using Yahoo Finance.
* Constructed stock and ETF universes for analysis.

### Problems Encountered

#### Universe Selection

Initial universe construction relied on S&P 500 ordering rather than market capitalization.

#### Bias Concerns

Using current top companies with historical data raised concerns regarding:

* Survivorship bias
* Look-ahead bias

### Mentor Feedback

* Current top market-cap companies are acceptable.
* Focus on liquid and currently tradeable assets.
* Do not restart the project because of survivorship concerns.

### Decisions Made

* Use current top market-cap US stocks.
* Retain historical data for exploratory analysis.
* Document survivorship bias as a project limitation.

### Outcome

Final asset universe established:

* 93 Stocks
* 15 ETFs

---

## Phase 2: Data Collection and Data Preparation

### Date

24 May 2026 – 30 May 2026

### Objective

Build a clean and consistent dataset suitable for statistical analysis.

### Work Completed

* Downloaded adjusted close prices.

* Validated data availability.

* Performed missing value checks.

* Generated:

  * Daily returns
  * Log returns
  * Normalized prices
  * Rolling volatility metrics

* Conducted dataset validation checks.

### Problems Encountered

* Yahoo Finance sector metadata retrieval produced incomplete results.
* Some assets required additional validation due to missing metadata.

### Decisions Made

* Use adjusted close prices.
* Use daily frequency.
* Store stocks and ETFs separately.

### Outcome

Created a clean dataset for EDA and correlation analysis.

---

## Phase 3: Exploratory Data Analysis

### Date

30 May 2026 – 03 June 2026

### Objective

Understand return behaviour, volatility characteristics, and overall dataset structure.

### Work Completed

* Price trend analysis.
* Normalized performance comparison.
* Return statistics.
* Log-return statistics.
* Outlier analysis using Z-scores.
* Volatility ranking analysis.
* Rolling volatility analysis.
* Return distribution analysis.
* Annualized return calculations.

### Key Findings

* Return distributions are non-normal.
* Several assets exhibit heavy tails and extreme observations.
* Volatility varies substantially across assets.
* Technology and growth-oriented stocks exhibit higher volatility profiles.

### Outcome

Established a statistical understanding of the asset universe before pair selection.

---

## Phase 4: Correlation Analysis and Pair Selection

### Date

03 June 2026 – 04 June 2026

### Objective

Identify candidate pairs for future cointegration testing.

### Work Completed

* Stock correlation matrix analysis.
* ETF correlation matrix analysis.
* Top correlated stock pair identification.
* Top correlated ETF pair identification.
* Stock versus ETF comparison.

### Key Findings

#### Top Stock Pairs

| Pair      | Correlation |
| --------- | ----------- |
| HD-LOW    | 0.882       |
| STX-WDC   | 0.869       |
| AMAT-LRCX | 0.862       |
| MA-V      | 0.856       |
| KLAC-LRCX | 0.850       |

#### Top ETF Pairs

| Pair    | Correlation |
| ------- | ----------- |
| SPY-QQQ | 0.948       |
| QQQ-XLK | 0.946       |
| SPY-XLK | 0.873       |

### Problems Encountered

#### Same-Company Pair Issue

Examples:

* GOOG-GOOGL
* Similar share-class relationships

These artificially inflated correlation rankings.

### Solution

Implemented filtering logic to remove same-company pairs from candidate selection.

### Mentor Feedback

* Correlation analysis alone is insufficient.
* Relationships should be evaluated through time.
* Stocks and ETFs should be analyzed separately.

### Outcome

Candidate pairs identified for further analysis.

---

## Phase 5: Correlation Stability Analysis

### Date

04 June 2026

### Objective

Evaluate whether highly correlated relationships remain stable through time.

### Work Completed

* Yearly correlation analysis.
* Yearly top-pair analysis.
* Rolling correlation analysis.
* Correlation stability summaries.

### Key Findings

Persistent relationships identified:

* AMAT-LRCX
* KLAC-LRCX
* AMAT-KLAC
* MA-V
* HD-LOW

Rolling correlation analysis showed:

* Correlations are not constant through time.
* Relationships strengthen and weaken across market regimes.
* Highly correlated pairs remain more stable than average pairs.

### Problems Encountered

#### Rolling Correlation Window Issue

Using:

* 252 observations
* 252-day rolling window

produced only a single rolling observation.

### Solution

Moved rolling analysis from recent datasets to full historical datasets.

### Mentor Feedback

* Static correlations are insufficient.
* Add rolling correlation analysis.
* Add yearly correlation analysis.

### Outcome

Correlation stability framework completed.

---

## Phase 6: Engle-Granger Cointegration Validation

### Date

Current Phase

### Objective

Validate Engle-Granger methodology before scaling to larger pair universes.

### Candidate Pairs

* MA-V
* HD-LOW
* AMAT-LRCX

### Planned Workflow

Correlation Analysis
→ Candidate Selection
→ Engle-Granger Cointegration
→ Spread Construction
→ ADF Testing
→ Hedge Ratio Estimation
→ Rolling Cointegration
→ Backtesting

### Current Status

In Progress

### Next Steps

1. Validate Engle-Granger implementation.
2. Construct spreads.
3. Test spread stationarity.
4. Estimate hedge ratios.
5. Develop rolling cointegration framework.
6. Evaluate pair stability.
7. Prepare for strategy development.

---

# Current Project Status

## Completed

* Project Planning
* Universe Selection
* Data Collection
* Data Cleaning
* EDA
* Volatility Analysis
* Correlation Analysis
* ETF Analysis
* Yearly Correlation Analysis
* Rolling Correlation Analysis
* Candidate Pair Selection

## In Progress

* Engle-Granger Cointegration Validation

## Planned

* Rolling Cointegration
* Pair Trading Signals
* Backtesting
* Performance Evaluation
