# Model Card — Healthcare Call Demand Forecasting

**Date:** 2026-05-08  
**Notebook:** `notebooks/0.2-ams-preprocessing-modeling.ipynb`

---

## Final Model

| Field | Value |
|---|---|
| Model type | ARIMA |
| Library | `statsmodels.tsa.statespace.SARIMAX` |
| Non-seasonal order (p, d, q) | (1, 1, 0) |
| Seasonal order (P, D, Q, s) | (0, 0, 0, 12) — no seasonal terms |
| Modeling target | `log_total_calls` (log1p-transformed `total_calls`) |
| Forecast back-transformation | `np.expm1()` |

---

## Features & Target

| Feature | Description | Used in Final Model |
|---|---|---|
| `total_calls` | Raw monthly call volume (target) | No (used for evaluation only) |
| `log_total_calls` | `np.log1p(total_calls)` — stabilizes variance | Yes (model input) |
| `covid_period` | Binary indicator: 1 if date ≥ 2020-03-01 | No (engineered for potential exogenous modeling) |
| `month_sin` | `sin(2π × month / 12)` — cyclical month encoding | No (engineered for potential exogenous modeling) |
| `month_cos` | `cos(2π × month / 12)` — cyclical month encoding | No (engineered for potential exogenous modeling) |

---

## Hyperparameters & Fitting Options

| Parameter | Value |
|---|---|
| `enforce_stationarity` | `False` |
| `enforce_invertibility` | `False` |
| `optimized` (MLE) | `True` (statsmodels default, `disp=False`) |
| `initialization_method` | statsmodels default |

---

## Model Selection

Six ARIMA/SARIMA candidates were evaluated using an **expanding-window cross-validation** (24 one-step-ahead forecasts) over the evaluation period. The best model was selected by **MAPE**.

| Candidate | Seasonal Order |
|---|---|
| ARIMA(0,1,1) | (0,0,0,12) |
| ARIMA(1,1,0) | (0,0,0,12) ✓ **selected** |
| ARIMA(1,1,1) | (0,0,0,12) |
| SARIMA(0,1,1)×(1,1,1,12) | (1,1,1,12) |
| SARIMA(1,1,0)×(1,1,1,12) | (1,1,1,12) |
| SARIMA(1,1,1)×(1,1,1,12) | (1,1,1,12) |

---

## Data Splits

| Split | Date Range | # Months | Purpose |
|---|---|---|---|
| Initial training | 2016-04 to 2021-12 | 69 | Warm-up for expanding window |
| Evaluation window | 2022-01 to 2023-12 | 24 | Model selection (expanding window) |
| Held-out test | 2024-01 to 2025-02 | 14 | Final unbiased evaluation |

---

## Validation Metrics (Expanding Window, 2022-01 – 2023-12)

Metrics are computed on the original call-count scale after back-transforming ARIMA forecasts.

| Model | MAE | RMSE | MAPE (%) | SMAPE (%) |
|---|---|---|---|---|
| **ARIMA(1,1,0)** | **20,801.1** | **31,044.9** | **11.82** | **11.79** |
| SeasonalNaive(12) | 102,190.0 | 126,489.2 | 74.42 | 51.46 |
| ETS_Additive | 114,371.7 | 150,841.0 | 75.52 | 72.81 |

---

## Test Metrics (Held-Out, 2024-01 – 2025-02)

Final model refit on all pre-test data (2016-04 to 2023-12) before evaluation.

| Model | MAE | RMSE | MAPE (%) | SMAPE (%) |
|---|---|---|---|---|
| ARIMA(1,1,0) | 15,871.1 | 21,736.9 | 9.92 | 9.80 |

---

## Artifacts

| File | Description |
|---|---|
| `models/validation_metrics.csv` | Expanding-window metrics for all compared models |
| `models/test_metrics.csv` | Held-out test metrics for the final model |
| `models/final_test_forecasts.csv` | Actual vs. forecast values for the test period |
| `reports/figures/expanding_window_comparison.png` | One-step-ahead forecasts vs. actuals (evaluation window) |
| `reports/figures/final_test_forecast.png` | Full-range test forecast plot |
| `reports/figures/final_test_forecast_zoomed.png` | Zoomed test forecast plot (from 2023 onward) |
