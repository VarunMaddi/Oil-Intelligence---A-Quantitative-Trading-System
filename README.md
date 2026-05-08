# Oil Intelligence — Quantitative Trading System

**Cross-sectional long/short equity strategy using physical oil supply-chain data to rank 7 oil-linked equities weekly.**

Built on Databricks (data engineering) + Google Colab (modelling). Trained 2017–2021, tested out-of-sample 2022–2023.

---

## Results (2022–2023 Out-of-Sample)

| Metric | Oil Intel | EW Benchmark¹ | S&P 500 |
|---|---|---|---|
| Sharpe Ratio | **1.890** | 1.108 | ~0.2 |
| Annualised Return | **15.57%** | 39.37% | ~-5% |
| Annualised Volatility | **5.63%** | 30.64% | ~18% |
| Max Drawdown | **-12.05%** | -25.50% | ~-25% |
| Sortino / Calmar / Fitness | **3.52 / 2.25 / 2.27** | — | — |
| t-stat / p-value | **2.64 / 0.009** | — | — |

> ¹ Equal-weighted long-only portfolio of all 7 traded assets (XLE, XOM, CVX, SLB, VLO, FRO, STNG) — the correct benchmark for a cross-sectional strategy.
>
> Oil Intel is dollar-neutral (long top-2, short bottom-2). At 5× leverage to match benchmark volatility it would return ~+165% vs benchmark +89%. Sharpe is scale-invariant.

Validated across **2,000-path block bootstrap** (P(Sharpe ≥ 1.25) = 82.4%) and **10-seed Monte Carlo** (mean Sharpe 2.13, min 1.54, all positive).

<img width="2235" height="1623" alt="oil_intel_results" src="https://github.com/user-attachments/assets/457b9058-8abf-428c-bbba-798a1221dfb2" />
---

## What It Does

Uses AIS vessel tracking data, EIA petroleum inventories, OPEC events, and macro rates to predict **which oil equities will outperform each other** over the next 5 trading days — not whether oil goes up or down.

- **Long** the top-2 ranked assets each week
- **Short** the bottom-2 ranked assets each week
- **Flat** on the remaining 3
- **Dollar-neutral** at all times — profit comes from relative performance, not market direction

**Why this works:** AIS vessel flow signals encode physical supply-chain dynamics (Middle East loading activity, dark fleet expansion, floating storage) with a 5–15 day lag before equity markets price them in. In 2022–2023, the model identified tanker outperformance (FRO +149%, STNG +275%) from Russia-Ukraine rerouting signals in AIS dark fleet and loitering data before these patterns were reflected in equity valuations.

---

## Architecture
# Oil Intelligence — Quantitative Trading System

**Cross-sectional long/short equity strategy using physical oil supply-chain data to rank 7 oil-linked equities weekly.**

Built on Databricks (data engineering) + Google Colab (modelling). Trained 2017–2021, tested out-of-sample 2022–2023.

---

## Results (2022–2023 Out-of-Sample)

| Metric | Oil Intel | EW Benchmark¹ | S&P 500 |
|---|---|---|---|
| Sharpe Ratio | **1.890** | 1.108 | ~0.2 |
| Annualised Return | **15.57%** | 39.37% | ~-5% |
| Annualised Volatility | **5.63%** | 30.64% | ~18% |
| Max Drawdown | **-12.05%** | -25.50% | ~-25% |
| Sortino / Calmar / Fitness | **3.52 / 2.25 / 2.27** | — | — |
| t-stat / p-value | **2.64 / 0.009** | — | — |

> ¹ Equal-weighted long-only portfolio of all 7 traded assets (XLE, XOM, CVX, SLB, VLO, FRO, STNG) — the correct benchmark for a cross-sectional strategy.
>
> Oil Intel is dollar-neutral (long top-2, short bottom-2). At 5× leverage to match benchmark volatility it would return ~+165% vs benchmark +89%. Sharpe is scale-invariant.

Validated across **2,000-path block bootstrap** (P(Sharpe ≥ 1.25) = 82.4%) and **10-seed Monte Carlo** (mean Sharpe 2.13, min 1.54, all positive).

---

## What It Does

Uses AIS vessel tracking data, EIA petroleum inventories, OPEC events, and macro rates to predict **which oil equities will outperform each other** over the next 5 trading days — not whether oil goes up or down.

- **Long** the top-2 ranked assets each week
- **Short** the bottom-2 ranked assets each week
- **Flat** on the remaining 3
- **Dollar-neutral** at all times — profit comes from relative performance, not market direction

**Why this works:** AIS vessel flow signals encode physical supply-chain dynamics (Middle East loading activity, dark fleet expansion, floating storage) with a 5–15 day lag before equity markets price them in. In 2022–2023, the model identified tanker outperformance (FRO +149%, STNG +275%) from Russia-Ukraine rerouting signals in AIS dark fleet and loitering data before these patterns were reflected in equity valuations.

---

## Architecture

