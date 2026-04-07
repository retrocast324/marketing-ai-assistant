# FireIO - Time Series Forecasting & Analytics API

## Overview

FireIO is an end-to-end FastAPI backend powered by **Nixtla TimeGPT** for automated time-series forecasting, anomaly detection, and real-time monitoring. It is deployed as a separate microservice on Vercel.

- **Repository**: [mahdi1234-hub/fireio](https://github.com/mahdi1234-hub/fireio)
- **Live API**: [https://fireio.vercel.app](https://fireio.vercel.app)
- **Health Check**: [https://fireio.vercel.app/health](https://fireio.vercel.app/health)

## Features

| Feature | Endpoint | Description |
|---------|----------|-------------|
| Forecasting | `POST /forecast` | Generate accurate predictions with confidence intervals (80%, 95%) |
| Anomaly Detection | `POST /anomaly-detect` | Identify unusual patterns in historical data |
| Real-Time Monitoring | `POST /monitor` | Detect anomalies as new data arrives (online detection) |
| Analytics | `POST /analytics` | Summary statistics, trend & seasonality analysis |
| Forecast Plot | `POST /forecast/plot` | Plotly charts with confidence interval bands |
| Anomaly Plot | `POST /anomaly-detect/plot` | Plotly charts with anomaly markers |

## Data Format

All endpoints accept any time-series data with datetime, features, and values:

```json
{
  "data": [
    {"timestamp": "2024-01-01T00:00:00", "value": 42.0, "unique_id": "series_1"},
    {"timestamp": "2024-01-02T00:00:00", "value": 45.0, "unique_id": "series_1"}
  ],
  "freq": "D",
  "time_col": "timestamp",
  "target_col": "value"
}
```

### Requirements
- Minimum 35 data points per series (TimeGPT requirement)
- `timestamp`: ISO-8601 datetime string
- `value`: numeric target value
- `unique_id`: optional series identifier for multi-series support
- `freq`: Pandas frequency string (D, h, MS, etc.) -- auto-detected if omitted

## Endpoint Details

### POST /forecast

Generate time-series forecasts with confidence intervals.

**Parameters:**
- `horizon` (int): Number of steps to forecast (default: 12)
- `level` (list[int]): Confidence interval levels, e.g., [80, 95]
- `finetune_steps` (int): Fine-tune steps for TimeGPT (default: 0)
- `model` (string): `timegpt-1` or `timegpt-1-long-horizon`

**Response includes:**
- Point forecasts with confidence bounds
- Analytics: historical/forecast means, CI widths, trend direction
- Plotly-compatible chart data (via /forecast/plot)

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

## Tech Stack

- **FastAPI** -- Python web framework
- **Nixtla TimeGPT** -- Foundation model for time-series
- **Plotly** -- Interactive charts with confidence bands
- **Pandas / NumPy** -- Data manipulation
- **Vercel** -- Serverless deployment
