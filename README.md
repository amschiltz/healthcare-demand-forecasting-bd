# Forecasting Healthcare Call Demand Across the COVID-Era Structural Break: Evidence from Bangladesh Health Portal Call Data

<a target="_blank" href="https://cookiecutter-data-science.drivendata.org/">
    <img src="https://img.shields.io/badge/CCDS-Project%20template-328F97?logo=cookiecutter" />
</a>

This project develops break-aware time-series models to forecast healthcare call demand across the COVID-Era structural break in Bangladesh.

## Key Results
- **Best Model:** ARIMA(1,1,1)
- **Test Performance:**
    - MAE: 16,416
    - RMSE: 21,869
    - MAPE: **10.34%**
- **Baseline Comparison:**
    - Seasonal Naive: ~74% MAPE
    - ETS: ~75% MAPE

## Final Test Forecast

![Final Forecast](reports/figures/final_test_forecast_zoomed.png)

The ARIMA model substantially outperformed simpler baselines, indicating that short-term dynamics and structural changes dominate over stable seasonal patterns.

## Project Summary
This project develops time-series models to forecast healthcare call demand using monthly call volume data from Bangladesh’s national health portal.

The analysis focuses on handling a major structural break during the COVID-19 pandemic, which disrupted historical patterns and reduced the effectiveness of traditional seasonal models.

Key findings:
- Models relying on year-over-year seasonality (ETS, Seasonal Naive) performed poorly
- A log-transformed ARIMA model achieved strong performance (~10% error)
- Forecasts suggest the series is relatively stable post-COVID, with limited predictable seasonal structure

## Problem Statement
Accurately forecasting healthcare call demand is critical for:
- staffing and resource allocation
- emergency response planning
- maintaining service accessibility

However, the COVID-19 pandemic introduced a structural break, making traditional forecasting approaches less reliable.

## Project Organization

```
├── LICENSE            <- Open-source license if one is chosen
├── Makefile           <- Makefile with convenience commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default mkdocs project; see www.mkdocs.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── pyproject.toml     <- Project configuration file with package metadata for 
│                         healthcare_demand and configuration for tools like black
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.cfg          <- Configuration file for flake8
│
└── healthcare_demand   <- Source code for use in this project.
    │
    ├── __init__.py             <- Makes healthcare_demand a Python module
    │
    ├── config.py               <- Store useful variables and configuration
    │
    ├── dataset.py              <- Scripts to download or generate data
    │
    ├── features.py             <- Code to create features for modeling
    │
    ├── modeling                
    │   ├── __init__.py 
    │   ├── predict.py          <- Code to run model inference with trained models          
    │   └── train.py            <- Code to train models
    │
    └── plots.py                <- Code to create visualizations
```

--------

