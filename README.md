# Oil Intelligence - Quantitative Trading System

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

> Equal-weighted long-only portfolio of all 7 traded assets (XLE, XOM, CVX, SLB, VLO, FRO, STNG) — the correct benchmark for a cross-sectional strategy.
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
# Oil Intelligence - Quantitative Trading System

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

> ¹ Equal-weighted long-only portfolio of all 7 traded assets (XLE, XOM, CVX, SLB, VLO, FRO, STNG) the correct benchmark for a cross-sectional strategy.
>
> Oil Intel is dollar-neutral (long top-2, short bottom-2). At 5× leverage to match benchmark volatility it would return ~+165% vs benchmark +89%. Sharpe is scale-invariant.

Validated across **2,000-path block bootstrap** (P(Sharpe ≥ 1.25) = 82.4%) and **10-seed Monte Carlo** (mean Sharpe 2.13, min 1.54, all positive).

---

## What It Does

Uses AIS vessel tracking data, EIA petroleum inventories, OPEC events, and macro rates to predict **which oil equities will outperform each other** over the next 5 trading days, not whether oil goes up or down.

- **Long** the top-2 ranked assets each week
- **Short** the bottom-2 ranked assets each week
- **Flat** on the remaining 3
- **Dollar-neutral** at all times profit comes from relative performance, not market direction

**Why this works:** AIS vessel flow signals encode physical supply-chain dynamics (Middle East loading activity, dark fleet expansion, floating storage) with a 5–15 day lag before equity markets price them in. In 2022–2023, the model identified tanker outperformance (FRO +149%, STNG +275%) from Russia-Ukraine rerouting signals in AIS dark fleet and loitering data before these patterns were reflected in equity valuations.

---

## Architecture
<img width="552.5" height="475" alt="image" src="https://github.com/user-attachments/assets/6fcb7b64-9012-4bea-8a3f-935d46017c9d" />


---

## How to Run

### Data Pipeline - Databricks Community Edition

```bash
# Set environment variables (never hardcode keys)
export GFW_TOKEN="your-gfw-token"
export EIA_KEY="your-eia-key"
export FRED_KEY="your-fred-key"
```

Run notebooks in order:

| Step | Notebook | Runtime |
|---|---|---|
| 1 | `bronze/01_bronze_pv_parallel.py` | ~100 hrs (7 parallel jobs) |
| 2 | `bronze/02_bronze_gfw_loitering.py` | ~2 hrs |
| 3 | `bronze/03_bronze_gfw_ais_gaps.py` | ~1 hr |
| 4 | `bronze/04_bronze_market_eia_fred_opec.py` | ~10 min |
| 5 | `silver/05_date_spine.py` → `11_eia_macro_opec_daily.py` | ~35 min total |
| 6 | `gold/14_feature_store_v2.py` | ~8 min |

Output: `gold_features_v2.csv` (1,761 rows × 92 columns)

### Model - Google Colab

1. Upload `gold_features_v2.csv`
2. Open `modeling/quant_pipeline.ipynb`
3. Runtime → Run all

---

## Data Sources

| Source | What | Volume |
|---|---|---|
| [GFW API v3](https://globalfishingwatch.org/our-apis/) | Port visits, loitering, AIS gaps | **15.6M rows** |
| [EIA API v2](https://www.eia.gov/opendata/) | Crude stocks, refinery utilisation, imports/exports | 7 weekly series |
| [FRED](https://fred.stlouisfed.org/) | TB3MS 3-month T-bill (risk-free rate) | Monthly |
| [Yahoo Finance](https://finance.yahoo.com/) | Brent, DXY, XLE/XOM/CVX/SLB/VLO/FRO/STNG/SPY | Daily OHLCV |
| OPEC press releases | Meeting dates + outcomes (hand-curated) | 37 events |

---

## Architecture

**Bronze → Silver → Gold** Delta Lake medallion on Databricks Unity Catalog  
`/Volumes/workspace/oil_intel/project_data/`

**Gold feature store:** 67 features - AIS vessel flows (21 cols), EIA petroleum
(7 series), Brent technicals, FRED rates, OPEC proximity flags, equity returns
and betas, crack spread, rig count.

**Model:** 5-model ensemble on 8,736-row panel (1,248 dates × 7 assets)
→ LGBMRanker (LambdaRank) + LightGBM + XGBoost + ExtraTrees + ElasticNet  
→ 25-fold purged walk-forward (5-day purge + 5-day embargo, Lopez de Prado)  
→ Dollar-neutral construction, 15% vol targeting, 2-tier DD brakes

---

## Tech Stack

`Databricks` `PySpark` `Delta Lake` `Unity Catalog` `LightGBM` `XGBoost`
`scikit-learn` `Python` `SQL` `GFW REST API v3` `EIA API v2` `FRED API`

---


