# InfluxDB Schema Design

## Database
- Name: aqi_db

## Measurement
- daily_aqi

## Tags
| Tag | Description |
|----|------------|
| city | French city name |

## Fields
| Field | Type | Description |
|------|------|-------------|
| avg_aqi | float | Daily average AQI |
| avg_pm25 | float | Daily average PM2.5 |

## Time
- Nanosecond precision
- Normalized to daily UTC midnight
