# ML Long/Short Equity Strategy — A Leakage-Aware Backtesting Pipeline

End-to-end machine learning pipeline for a cross-sectional long/short
equity strategy on 111 US large caps — built around one question:

> **"Would this result survive scrutiny?"**

Most retail ML backtests fail on leakage, survivorship, or overfitting.
This project treats *robustness testing as the deliverable* and reports
an honest, modest result rather than an inflated one.

---

## TL;DR

| | |
|---|---|
| **Universe** | 111 US large-cap stocks |
| **Signal model** | L2-regularised logistic regression (direction of excess return) |
| **Portfolio** | Long top quintile / short bottom quintile, equal-weight |
| **Validation** | Expanding-window walk-forward, chronological splits |
| **Out-of-sample result** | **Net Sharpe 0.37 (t = 1.45)** — reported as **not statistically significant** |
| **Verdict** | A candidate signal, not a discovery — and the pipeline knows the difference |

## Features & data

- **Price/volume & volatility** features — Yahoo Finance
- **Macro regime:** Treasury yield-curve slope (FRED), VIX
- **Text sentiment:** Loughran–McDonald word lists over 10-K/10-Q filings,
  merged **point-in-time (as-of filing dates)** to prevent lookahead on
  information availability

## Robustness & falsification suite (the actual point)

- **Placebo / falsification testing** — signal re-estimated across 10
  randomised-label runs → empirical p-value against the "no signal" null
- **Feature-group ablation** — feature families removed one at a time to
  isolate what actually drives performance
- **Walk-forward hygiene** — expanding-window design; scalers and model
  fit on training data only; refit-frequency sensitivity tested
- **Subperiod stability** — performance checked across regimes, not just
  full-sample averages
- **Economic reality** — transaction-cost sensitivity at 1 / 5 / 10 bps
  with turnover analysis; rank-IC analysis of signal quality
- **Error analysis** — named failure cases examined, plus a drift-monitoring
  plan for how the signal should be watched in deployment

## Known limitations (disclosed, not hidden)

- Universe is a fixed, present-day stock list → **survivorship bias
  applies** and is discussed explicitly
- Yahoo Finance data; no delisting returns
- Short backtest window relative to institutional standards

## Roadmap — in progress as a solo extension

- **Point-in-time index constituents via CRSP/Compustat (WRDS)** to
  eliminate survivorship bias
- Delisting-adjusted returns
- Volatility targeting as a portfolio-level risk control

## Repository

- `equity_ls_pipeline.ipynb` — full pipeline: data build, features,
  walk-forward training, portfolio construction, and the complete
  robustness suite with charts

---

*Originally developed for FINC6028 (University of Sydney). Pipeline
design and implementation: **Samrat Chakraborty**.*
