
# Real-Time IoT Data Processing and Anomaly Detection using GCP

Built using ESP32 + MQ135 Sensor, Google Apps Script, Google Sheets, BigQuery ML, and Looker Studio.

---

## Project Overview

This project demonstrates a real-time IoT pipeline that captures air quality sensor data using ESP32, stores it in Google Sheets, pushes it to BigQuery, and performs anomaly detection using BigQuery ML's `ARIMA_PLUS` model. A live Looker Studio dashboard visualizes real-time and anomalous values.

---

## Key Features

- Real-time sensor integration via ESP32 + MQ135
- Google Apps Script webhook for ingestion
- Google Sheet for raw data storage
- BigQuery for querying and data warehousing
- BigQuery ML ARIMA_PLUS model for forecasting
- Anomaly detection pipeline with threshold logic
- Interactive Looker Studio visualization dashboard

---

## System Architecture



---

## Tech Stack

| Layer              | Technology Used                         |
|--------------------|------------------------------------------|
| Hardware           | ESP32, MQ135 Gas Sensor                  |
| Communication      | HTTP (Webhook from ESP32 to Apps Script) |
| Data Storage       | Google Sheets, BigQuery                  |
| Data Processing    | BigQuery SQL, BigQuery ML (ARIMA_PLUS)   |
| Visualization      | Looker Studio                            |
| Programming        | Arduino C++, Google Apps Script (JS)     |

---

## Data Flow Pipeline

1. ESP32 reads air quality data from MQ135.
2. Sends data as a POST request to Google Apps Script.
3. Apps Script appends data (ppm + timestamp) to Google Sheets.
4. Periodic upload from Google Sheets to BigQuery via script.
5. BigQuery ML trains ARIMA_PLUS model on training data.
6. Real-time data is forecasted and compared against actuals.
7. Anomalies are inserted into `anomalies_detected` table.
8. Looker Studio visualizes both real-time and anomalous data.

---

## Setup Instructions

### 1. ESP32 Arduino Code
- Configure WiFi credentials and POST request to Apps Script Web App URL.
- Upload code to ESP32 board.
- Ensure MQ135 is connected to correct analog pin.

### 2. Google Apps Script
- Create a new script bound to a Google Sheet.
- Paste webhook receiver logic.
- Deploy as Web App (Deploy → New Version → Anyone can access).

### 3. BigQuery Setup
- Create dataset: `iot_data`
- Create tables: `sensor_readings`, `realtime_data`, `training_data`, `anomalies_detected`
- Use scheduled queries / manual scripts to move data from Sheets to BigQuery.

### 4. BigQuery ML
- Train ARIMA_PLUS on `training_data` with timestamp and ppm columns.
- Forecast real-time sensor values and detect anomalies.

### 5. Looker Studio
- Connect BigQuery tables to Looker.
- Build dashboards for:
  - Real-time PPM readings
  - Forecasted vs actual
  - Highlighted anomalies

---

## Screenshots



- **Real-Time Dashboard**  
  ![Dashboard Screenshot](screenshots/dashboard.png)

- **Forecast vs Actual (Anomaly Highlighted)**  
  ![Anomaly Detection](screenshots/anomalies.png)

- **BigQuery ML Query Example**  
  ![BigQuery Forecast](screenshots/bigquery_forecast.png)

---

## 📁 Project Structure

```bash
├── esp32/                         # Arduino code for ESP32 + MQ135
│   └── esp32_mq135_sensor.ino
├── apps_script/                  # Google Apps Script code
│   └── code.gs
├── bigquery/
│   ├── create_tables.sql
│   ├── train_model.sql
│   ├── forecast.sql
│   └── detect_anomaly.sql
├── looker_studio/                # Screenshot references or exported reports
├── screenshots/                  # Screenshots for report
├── README.md
└── LICENSE
```

---

## Sample BigQuery Forecast Query

```sql
SELECT *
FROM ML.FORECAST(MODEL `iot_data.anomaly_model`,
  STRUCT(1 AS horizon, 0.8 AS confidence_level))
ORDER BY forecast_timestamp DESC
```

---


## Contributors

- Sachin Singh [M24CSE033]
- Vipul Kumar [M24CSE028]
- Alok Dutta [M24CSE032]

