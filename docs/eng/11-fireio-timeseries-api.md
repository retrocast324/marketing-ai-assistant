# FireIO - Time Series Forecasting & Analytics API

## Overview

FireIO is an end-to-end FastAPI backend powered by **Nixtla TimeGPT** for automated time-series forecasting, anomaly detection, real-time monitoring, and analytics with rich interactive Plotly charts. It is deployed as a separate microservice on Vercel.

- **Repository**: [mahdi1234-hub/fireio](https://github.com/mahdi1234-hub/fireio)
- **Live API**: [https://fireio.vercel.app](https://fireio.vercel.app)
- **Interactive Docs**: [https://fireio.vercel.app/docs](https://fireio.vercel.app/docs)
- **Health Check**: [https://fireio.vercel.app/health](https://fireio.vercel.app/health)

## Features

| Feature | Endpoint | Description |
|---------|----------|-------------|
| Forecasting | `POST /forecast` | Predictions with 80%/95% confidence intervals |
| Forecast Chart | `POST /forecast/chart` | Interactive HTML Plotly chart (6 panels: forecast+CI, distribution, trend, residuals, seasonal, ACF) |
| Anomaly Detection | `POST /anomaly-detect` | Identify unusual patterns in historical data |
| Anomaly Chart | `POST /anomaly-detect/chart` | HTML chart with anomaly markers and deviation bars |
| Real-Time Monitoring | `POST /monitor` | Online anomaly detection as new data arrives |
| Monitor Chart | `POST /monitor/chart` | HTML chart with alerts and severity bars |
| Analytics | `POST /analytics` | Summary statistics, trend, seasonality, feature analysis |
| Analytics Chart | `POST /analytics/chart` | HTML decomposition dashboard (observed+trend, seasonal, residuals, ACF, top exogenous features) |
| Forecast Plot JSON | `POST /forecast/plot` | Plotly JSON dict for embedding |
| Anomaly Plot JSON | `POST /anomaly-detect/plot` | Plotly JSON dict for embedding |

## Data Format

All endpoints accept time-series data with optional exogenous features:

```json
{
  "data": [
    {
      "timestamp": "2022-01-01 00:00:00",
      "value": -0.06,
      "features": {
        "Irradiation": 0.0,
        "Temperature ambiante": 6.2,
        "Humidite ambiante": 80.5,
        "Vitesse vent": 0.4,
        "Carbon monoxide": 186.79,
        "PM10": 11.0,
        "Pressure": 1020.0
      }
    }
  ],
  "freq": "30min",
  "time_col": "timestamp",
  "target_col": "value"
}
```

### Requirements
- **Minimum observations**: 144 for `timegpt-1` (30-min freq), 1008 for `timegpt-1-long-horizon`
- `timestamp`: ISO-8601 datetime string
- `value`: numeric target value
- `features`: optional dict of exogenous variables (numerical or categorical). Categorical values are auto-encoded to numeric codes.
- `unique_id`: optional series identifier for multi-series support
- `freq`: Pandas frequency string (`30min`, `h`, `D`, `MS`, etc.) -- auto-detected if omitted

## Exogenous Variables Support

Any number of numerical or categorical features can be passed in the `features` dict. These are automatically:

1. Flattened into separate DataFrame columns
2. Categorical values encoded to numeric codes
3. Passed to TimeGPT as exogenous regressors
4. Analyzed for correlation with the target variable

For forecasting with exogenous variables, you can optionally provide `future_features` (one row per horizon step) to supply known future values of the exogenous variables.

### Tested with Real-World Solar/Weather Data

Successfully tested with 27 exogenous features from a solar panel monitoring dataset:

| Top Features by Correlation | |r| with Y1t |
|---|---|
| Irradiation | 0.9975 |
| Humidite ambiante | 0.5021 |
| Temperature | 0.4414 |
| Vitesse vent | 0.3277 |
| Angle du vent | 0.2959 |
| Humidity | 0.2377 |
| Wind Bearing | 0.1828 |
| Visibility | 0.1340 |

## Endpoint Details

### POST /forecast

Generate time-series forecasts with confidence intervals.

**Parameters:**
- `horizon` (int): Number of steps to forecast (default: 12)
- `level` (list[int]): Confidence interval levels, e.g., [80, 95]
- `finetune_steps` (int): Fine-tune steps for TimeGPT (default: 0)
- `model` (string): `timegpt-1` or `timegpt-1-long-horizon`
- `future_features` (list): Optional exogenous values for future steps

**Response includes:**
- Point forecasts with confidence bounds
- Analytics: historical/forecast means, CI widths, trend direction
- List of exogenous features used

### POST /forecast/chart

Returns a standalone interactive HTML page with 6-panel Plotly chart:
1. Forecast with confidence interval bands
2. Forecast distribution histogram
3. Trend component
4. Residuals analysis
5. Seasonal/cyclical pattern
6. Autocorrelation function (ACF)

### POST /anomaly-detect

Detect anomalies in historical time-series data using TimeGPT confidence bounds.

**Response includes:**
- List of all data points with anomaly flags
- Total anomaly count and ratio
- Analytics summary

### POST /monitor

Real-time monitoring: compare new incoming data against forecasted bounds.

**Additional parameters:**
- `new_data`: New data points to check against historical forecast

**Response includes:**
- Alerts with actual vs expected values, bounds, deviation, and severity
- Total alert count

### POST /analytics

Comprehensive time-series analytics without calling TimeGPT.

**Response includes:**
- Summary statistics (mean, std, min, max, median, quartiles, skewness, kurtosis)
- Trend analysis (direction, rolling window, change percentage)
- Seasonality detection (estimated period, autocorrelation, confidence)
- Feature analysis (correlation with target, importance ranking) -- when exogenous features are present

### POST /analytics/chart

Returns an interactive HTML analytics dashboard with:
1. Observed data + trend overlay
2. Seasonal/cyclical component
3. Color-coded residuals with +/-2 std bands
4. ACF with significance bounds
5. Top 8 exogenous features by correlation (when features provided)

## Tech Stack

- **FastAPI** -- Python web framework with auto-generated OpenAPI docs
- **Nixtla TimeGPT** -- Foundation model for time-series forecasting
- **Plotly** -- Interactive multi-panel charts (HTML + JSON output)
- **Pandas / NumPy** -- Data manipulation and decomposition
- **Vercel** -- Serverless Python deployment
