# Methodology Evolution

This document records the key design decisions made during the project and the reasoning behind each revision. It is intended to show the mentor and any reviewer that every methodological choice was deliberate rather than accidental.

---

## V1 → V2: Universe Construction

**Original:** Stocks selected manually based on familiarity.

**Problem:** Subjective selection introduces researcher bias and makes the universe difficult to justify or reproduce.

**Change:** Programmatically extract all S&P 500 constituents from Wikipedia, then rank by market capitalisation via the Yahoo Finance API. The top 93 companies by market cap form the stock universe.

**Effect:** Universe construction is now reproducible and auditable. Any reviewer can re-run the selection cell and get the same result.

---

## V2 → V3: S&P 500 Ordering vs. Market-Cap Ranking

**Original:** Selected the first 100 companies from the Wikipedia S&P 500 list.

**Problem:** The Wikipedia table is not ordered by company size. Picking the first 100 rows produces an arbitrary and inconsistent set of companies.

**Change:** Sort all S&P 500 constituents by `marketCap` descending, take the top 100. After data quality filtering, the final count is 93 stocks.

**Effect:** The universe reliably represents the most liquid and most actively traded US equities, which is a necessary condition for pairs trading research.

---

## V3 Discussion: Survivorship Bias

**Concern raised:** Using current market leaders with historical data means the universe is defined by companies that have already succeeded — companies that failed or were delisted during the sample period are not represented.

**Mentor guidance:** For an internship-level validation project, continue using current market leaders. The priority is methodology correctness and practical relevance. The bias is documented as a known limitation.

**Decision:** Retain the current market-cap universe. Survivorship bias is listed in the assumptions register.

---

## V4: Stocks and ETFs Separated

**Original:** Stocks and ETFs were analysed together in a combined correlation matrix.

**Problem:** ETFs automatically rebalance and hold diversified baskets of assets. Their correlation structure is fundamentally different from individual stocks. Mixed analysis produced results that were difficult to interpret.

**Change:** Stocks and ETFs are treated as separate universes throughout the EDA, correlation analysis, and cointegration testing. Cross-asset comparisons are added as a separate section.

---

## V5: Static → Yearly Correlation Analysis

**Original:** A single correlation matrix over the full historical period.

**Problem:** A single static figure assumes the relationship is constant. Market regimes change — correlations that held during 2018–2020 may not hold in 2023–2025.

**Mentor feedback:** Long-term static correlations do not represent current market behaviour. Add time-varying analysis.

**Change:** Added a yearly correlation analysis loop that computes the top correlated pairs for each calendar year, enabling comparison across market environments (pre-COVID, COVID, rate cycle, AI expansion).

---

## V6: Yearly → Rolling Correlation Analysis

**Original:** Yearly correlation snapshots only.

**Problem:** Yearly snapshots still miss intra-year regime changes. A pair could appear stable year-over-year while actually weakening significantly mid-year.

**Change:** Added a 252-day rolling correlation analysis over the full historical sample. This allows continuous monitoring of relationship stability rather than annual snapshots.

**Implementation note:** An initial attempt used a 252-day rolling window applied to a 252-observation recent-data subset. This produced only one valid rolling observation. The rolling analysis was moved to the full historical dataset, which produces a meaningful time series of rolling correlations.

---

## V7: Pair Universe Reduction

**Original:** Run cointegration tests on all possible pairs.

**Problem:** 93 stocks produce 4,278 unique pairs. Running the full pipeline on all pairs without prior screening would be computationally wasteful and statistically problematic — with 4,278 tests at α = 0.05, roughly 214 false positives are expected by chance alone.

**Mentor feedback:** Validate the methodology on a small number of well-justified pairs before scaling.

**Change:** Correlation analysis is used as a screening stage. Only pairs that pass a high correlation threshold and demonstrate rolling stability proceed to cointegration testing. Pipeline:

```
Data Collection → EDA → Correlation Screening → Rolling Stability →
Candidate Selection → Cointegration Testing
```

---

## V8: Economic Justification for Pair Selection

**Original:** Pairs selected on correlation coefficient alone.

**Problem:** High correlation is a necessary but not sufficient condition for cointegration. Without economic justification, a high-correlation pair may simply share a common factor temporarily.

**Change:** Final candidate pairs must satisfy three criteria: (1) high 1-year Pearson correlation, (2) similar sector and business model, (3) stable rolling correlation across multiple years. Same-company pairs (e.g. GOOG / GOOGL) are removed by a hard-coded blocklist and secondary name-matching filter.

**Final candidates:** MA/V, HD/LOW, AMAT/LRCX.

---

## V9: Validation Before Scaling

**Original:** Intended to run Engle-Granger on all candidate pairs immediately.

**Problem:** An implementation error in the pipeline would silently propagate across hundreds of tests.

**Change:** Validate the full workflow on three well-understood pairs first. Only after the implementation is confirmed correct — including cross-validation with `statsmodels.coint()` — will it be applied to the full universe.
