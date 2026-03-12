# Brent Crude Forward Curve & Spread Regime Analysis

**Forward curve monitoring of the global crude oil market using ICE Brent Crude futures**

---

## Project Overview

This project analyzes the **ICE Brent Crude** forward curve over the period **January 2025 to February 2026** in order to study:
- forward curve structure,
- prompt tightness,
- spread behavior,
- volatility regimes,
- and the statistical properties of stress events.

**Period:** Jan 2025 – Feb 2026 (288 trading days)  
**Asset:** ICE Brent Crude Futures  
**Focus:** rolling contracts, spread monitoring, anomaly detection, regime analysis

The objective is not to build a predictive trading model, but to develop a **research-grade monitoring framework** showing how forward-curve information can be used to identify periods of structural stress in the global crude oil market.

---

## Project Logic & Motivation

### Why This Project?

In commodity markets, flat price alone rarely gives a complete picture of supply-demand conditions. Traders and risk managers often monitor the **shape of the forward curve** to understand whether the market is balanced, tightening, or under stress.

This project was built to address four practical questions:

1. **How should raw futures contracts be rolled properly?**  
Rolling logic is critical in commodity analytics. Poor expiry handling can distort returns, spreads, and risk signals.

2. **How can curve structure be transformed into systematic indicators?**  
The notebook builds rolling spreads, volatility measures, Z-scores, and a simple regime framework.

3. **What does the 2025–2026 sample suggest about crude oil market structure?**  
The analysis examines whether the curve remained stable or entered repeated stress regimes.

4. **How can these results be communicated in a way that is useful for monitoring?**  
The notebook emphasizes concise charts, validation steps, and interpretable thresholds rather than complex modeling for its own sake.

---

## What Was Built

### 1. Data Engineering

The starting point is a matrix of daily ICE settlement prices across contract months. From this raw structure, I built rolling contracts:
- **M1**: front month
- **M2**: second nearby
- **M3**: third nearby
- **M6**: sixth nearby

Particular attention was given to:
- contract ordering,
- expiry handling (Brent-specific rules: contracts expire 2 months prior to delivery),
- clean settlements (no duplicates on expiry dates),
- missing values,
- and roll-date consistency.

This is the technical core of the project: without a coherent rolling methodology, all downstream analysis becomes unreliable.

### 2. Spread Construction

Using the rolling contracts, I calculated:
- **M1–M2**
- **M1–M3**
- **M1–M6**

These spreads are used to monitor:
- prompt tightness,
- near-term imbalance,
- and broader storage / structural conditions.

I also computed:
- daily log returns on M1,
- 20-day rolling annualized volatility.

### 3. Statistical Monitoring Framework

To measure how unusual current spread levels are relative to recent history, I implemented:
- **60-day rolling Z-scores** for M1–M3 and M1–M6,
- distribution diagnostics (skewness, kurtosis, histogram analysis),
- spread-volatility relationship analysis.

This allows the notebook to distinguish between ordinary backwardation and statistically unusual tightening episodes.

### 4. Regime Framework

A simple regime classification was added using M1–M6 thresholds:
- **Normal**
- **Moderate Tight**
- **Tight**
- **Stress**

This classification is not presented as a universal truth, but as a practical monitoring framework for interpreting changes in curve structure.

### 5. Validation & Visualization

The notebook includes explicit validation steps:
- roll-date checks,
- jump diagnostics,
- spread consistency review,
- missing-value inspection.

It then summarizes the results through a structured set of charts and concise interpretation.

---

## Main Findings

The sample suggests that the global crude oil market remained in moderate backwardation over the period, with a brief episode of stronger prompt tightness in April-June 2025.

The most notable episode occurred in **April-June 2025**, when short-end curve spreads moved to elevated levels relative to their recent history, though remaining well below crisis thresholds.

The results show three main patterns:

1. **Moderate backwardation**  
The market remained backwardated 88.9% of the time, consistent with moderately firm prompt conditions but with more flexibility than regional refined products markets.

2. **Normal spread behavior**  
Spread distributions show thin tails. Extreme dislocations occur less frequently than in fat-tailed commodity markets, consistent with a highly liquid global benchmark.

3. **Moderate association between curve stress and volatility**  
Periods of spread expansion tend to coincide with higher realized volatility. This is consistent with the view that prompt tightness contributes to price instability.

This should be interpreted as a **descriptive and monitoring result**, not as formal proof of causality.

---

## Key Findings

### 1. Moderate Tightness

- Market in backwardation on **256 out of 288 trading days** (88.9%)
- Average **M1–M3 spread: 1.14 USD/bbl**
- Average **M1–M6 spread: 2.25 USD/bbl**
- Consistent with a moderately firm prompt market over the sample

### 2. Moderate April-June 2025 Episode

- **April 30, 2025**: short-term structure reached a local peak
- **M1–M3 peaked at 3.27 USD/bbl** (2.9x average)
- **M1–M6 peaked at 5.75 USD/bbl** (2.6x average)
- **20D annualized volatility reached 52%**

Under standard normal assumptions, such a move represents a 4-sigma event; the fact that it occurs within a short sample suggests moderate tail risk in crude oil spreads.

### 3. Thin-Tailed Distribution

- **Skewness: 1.02**
- **Kurtosis: 1.37**
- **1.7% of observations** recorded Z-scores above 3, versus roughly **0.3%** under a normal benchmark

This suggests that extreme spread dislocations are less frequent than in fat-tailed commodity markets, consistent with Brent's role as a highly liquid global benchmark.

### 4. Structure and Volatility

- Correlation **M1–M3 vs Volatility: 0.516**
- Correlation **M1–M6 vs Volatility: 0.241**

These results suggest a meaningful association between short-term spread widening and higher volatility, consistent with the idea that prompt tightness and price instability often emerge together.

---

## Technical Highlights

### Data Engineering
- Rolling contract construction with explicit expiry handling (Brent-specific rules)
- Clean settlements (no duplicates on expiry dates)
- Validation of jumps, missing values, and spread consistency

### Statistical Analysis
- 60-day rolling Z-score framework
- Distribution analysis using skewness and kurtosis
- Correlation review between curve structure and volatility

### Regime Classification
- **Normal:** M1–M6 < 5 USD/bbl
- **Moderate Tight:** M1–M6 between 5 and 10 USD/bbl
- **Tight:** M1–M6 between 10 and 20 USD/bbl
- **Stress:** M1–M6 > 20 USD/bbl

These thresholds are intended as practical monitoring references for this sample, not universal market constants.

### Visualization
- 11 structured charts with concise interpretation
- Summary table of key metrics
- Multi-panel dashboard for market monitoring
- Regime timeline for fast visual interpretation

---

## Key Charts

| Chart | Description | Main Insight |
|-------|-------------|--------------|
| **Chart 1** | M1 Price & Volatility | Volatility rises during structural tightening |
| **Chart 2** | Forward Curve Snapshots | Curve shape remained in mild backwardation throughout |
| **Chart 3** | Short-Term Structure (M1-M2, M1-M3) | Prompt tightness intensified moderately in April-June 2025 |
| **Chart 4** | Long Structure (M1-M6) | Moderate backwardation kept storage economics viable |
| **Chart 5** | Z-Score M1-M3 | April 2025 stands out as a moderate short-end event |
| **Chart 6** | Z-Score M1-M6 | Broader curve showed moderate stress in June |
| **Chart 7** | Distribution (Histogram) | Spread behavior is closer to normal than fat-tailed |
| **Chart 8** | Spread vs Volatility | Higher spreads are associated with higher volatility |
| **Chart 9** | Event Timeline | Structural moves can be interpreted alongside market events |
| **Chart 10** | Market Dashboard | Summary monitoring view across price, spread, Z-score, and volatility |
| **Chart 11** | Regime Classification | The sample remained in Normal regime throughout |

---

## What This Project Signals

This project is designed to demonstrate:
- **forward curve literacy**
- understanding of **physical commodity imbalance**
- ability to build **clean rolling futures analytics**
- ability to turn market structure into **interpretable monitoring signals**
- ability to present commodity analysis in a professional format

---

## Limitations

- 13-month sample only
- no direct fundamental inputs (EIA inventories, OPEC production, freight)
- no out-of-sample validation
- regime thresholds are practical, not universal

This should be viewed as a **structured exploratory framework**, not a production trading system.

---

## Next Extensions

- extend to multi-year history
- add fundamental and flow data
- compare with other crude benchmarks (WTI, Dubai)
- test alternative regime thresholds
- develop a systematic monitoring dashboard
