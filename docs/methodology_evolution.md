# Methodology Evolution

## Version 1: Initial Asset Universe Selection

### Original Approach

Select stocks manually based on familiarity and perceived importance.

### Problem

Selection process was subjective and difficult to justify.

### Why Problematic

* Potential researcher bias.
* Difficult to reproduce.
* May overlook important assets.

### Revised Approach

Use a systematic stock universe construction process.

### Impact

Improved objectivity and reproducibility.

---

## Version 2: S&P 500 Ordering Approach

### Original Approach

Select the first 100 companies from the S&P 500 list.

### Problem

The ordering of S&P 500 constituents does not necessarily reflect company size or liquidity.

### Why Problematic

* Universe definition becomes arbitrary.
* Large-cap representation is inconsistent.

### Revised Approach

Select companies based on market capitalization.

### Impact

Created a more representative universe of highly liquid US equities.

---

## Version 3: Current Top Market-Cap Universe

### Original Concern

Using today's largest companies with historical data introduces:

* Survivorship bias
* Look-ahead bias

### Discussion

The concern was discussed during mentor review.

### Mentor Guidance

* Continue using current market leaders.
* Focus on liquid assets available today.
* Prioritize methodology validation over survivorship-bias correction.

### Decision

Retain the current top market-cap universe.

### Impact

Avoided restarting the project and maintained practical relevance.

---

## Version 4: Combined Stock and ETF Analysis

### Original Approach

Analyze stocks and ETFs together.

### Problem

Different asset structures produced difficult-to-interpret results.

### Why Problematic

* ETFs automatically rebalance.
* Stocks represent individual businesses.
* Correlation structures differ significantly.

### Revised Approach

Analyze stocks and ETFs separately.

### Impact

Improved interpretation and cleaner comparisons.

---

## Version 5: Static Correlation Analysis

### Original Approach

Use a single correlation matrix covering the entire historical period.

### Problem

Assumes relationships remain constant through time.

### Why Problematic

* Market regimes change.
* Sector dynamics evolve.
* Historical relationships may no longer be relevant.

### Mentor Feedback

Long-term correlations may not represent current market behaviour.

### Revised Approach

Add yearly correlation analysis.

### Impact

Allowed comparison of relationships across different market environments.

---

## Version 6: Correlation Stability Analysis

### Original Approach

Use yearly correlations only.

### Problem

Yearly snapshots provide limited information about relationship dynamics.

### Why Problematic

Relationships may strengthen or weaken within a year.

### Revised Approach

Implement rolling correlation analysis.

### Impact

Enabled evaluation of correlation stability through time.

---

## Version 7: Pair Selection Methodology

### Original Approach

Potentially run cointegration tests on all possible pairs.

### Problem

Large number of pair combinations.

Example:

* 93 stocks
* 4,278 possible stock pairs

### Why Problematic

* Computationally inefficient.
* Increased risk of false positives.
* Difficult to interpret results.

### Mentor Feedback

Validate methodology before scaling.

### Revised Approach

Use correlation analysis as a screening stage.

Pipeline:

Data Collection
→ EDA
→ Correlation Analysis
→ Rolling Correlation
→ Candidate Selection
→ Cointegration Testing

### Impact

More efficient and statistically defensible workflow.

---

## Version 8: Candidate Pair Selection

### Original Approach

Purely statistical pair selection.

### Problem

High correlation alone does not guarantee meaningful relationships.

### Why Problematic

Relationships may lack economic justification.

### Revised Approach

Select pairs using:

* High correlation
* Industry similarity
* Business similarity
* Correlation stability

### Candidate Pairs

* MA – V
* HD – LOW
* AMAT – LRCX

### Impact

Improved economic interpretability.

---

## Version 9: Cointegration Testing Strategy

### Original Approach

Run Engle-Granger on all candidate pairs immediately.

### Problem

Implementation correctness had not yet been validated.

### Why Problematic

Errors could scale across hundreds or thousands of tests.

### Revised Approach

Validate Engle-Granger methodology on a small number of highly relevant pairs first.

### Current Validation Pairs

* MA – V
* HD – LOW
* AMAT – LRCX

### Impact

Improved reliability and easier debugging.

---

## Planned Future Evolution

### Rolling Cointegration

Current Status:

Planned

Purpose:

Evaluate whether cointegration relationships remain stable through time.

### Pair Trading Framework

Current Status:

Planned

Purpose:

Generate and evaluate statistical arbitrage signals.

### Backtesting

Current Status:

Planned

Purpose:

Assess real-world performance of the methodology.
