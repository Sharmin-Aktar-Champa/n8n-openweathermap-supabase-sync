# 🌦️ n8n-openweathermap-supabase-sync

This n8n workflow automates weather monitoring by fetching data from **OpenWeatherMap**, processing alerts based on custom logic, logging results into **Supabase**, and sending formatted email summaries.

---

## 🛠 Configuration

### 1. API Setup & Key Config
*   **OpenWeatherMap**: 
    - Sign up at [OpenWeatherMap](openweathermap.org).
    - Generate an **API Key** (Current Weather Data).
    - In n8n, create a new **OpenWeatherMap Credential** and paste your API Key.
    - Set the **Unit** to `Metric` in the node settings to receive data in Celsius.
 
### 2. Supabase Details
*   **Database Setup**: Create a table named `weather_logs` by running the following SQL in your Supabase SQL Editor:


```sql
CREATE TABLE weather_logs (
  id SERIAL PRIMARY KEY,
  run_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  city TEXT,
  temperature FLOAT,
  temperature_unit TEXT DEFAULT 'C',
  condition TEXT,
  humidity INT,
  wind_speed FLOAT,
  alert_type TEXT,
  raw_response JSONB
);
```

### 🔑 n8n Credentials Setup
To connect via the **Supabase Node (REST API)**, use the following credentials from your Supabase project:

*   **Host (API URL)**: The `URL` found under **Project Settings > Data API**.
*   **Service Role Key**: The `service_role` (secret) API key found under **Project Settings > API**.
*   **Setup**: 
    1. In n8n, create a new **Supabase Credential**.
    2. Paste the **API URL** into the `Host` field and the **Service Role Key** into the `API Key` field.
    3. *Note: No port or database password is required for this REST API connection method.*

---

## 📧 3. Email Configuration
*   **Provider**: Gmail SMTP.
*   **Setup Steps**:
    1.  Enable **2-Step Verification** on your Google account.
    2.  Generate an [App Password](myaccount.google.com) from Google Security settings.
*   **n8n SMTP Settings**:
    *   **Host**: `smtp.gmail.com`
    *   **Port**: `465 (SSL)`
    *   **User**: *Your Gmail address*
    *   **Password**: *The 16-character App Password*

---

## ✨ Bonus Features Implemented

I have enhanced the workflow with the following features:

-   **Multi-City Processing (Looping)**: Instead of a single city, the workflow now iterates through a fixed list of cities, fetching weather data and generating reports for each one automatically.
-   **Automated Multi-Channel Reporting**: For every city in the list, the system performing two actions:
    -   Logging the data into **Supabase** for historical records.
    -   Sending a customized **Email Alert** for that specific city.
-   **Additional Weather Metrics**: The summary report now includes extra data points like **Visibility**, **Pressure**, **Wind Direction(in degrees)** and **Feels-like Temperature** for a more comprehensive overview.


## 📥 How to Import and Run
1.  **Download JSON**: Export your n8n workflow as a `.json` file.
2.  **Import**: In n8n, click **Workflow Menu (⋮) > Import from File**.
3.  **Connect Credentials**:
    *   Attach your **OpenWeatherMap** credentials.
    *   Attach your **Supabase** credentials.
    *   Attach your **SMTP** email credentials.
4.  **Trigger**:
    *   **Schedule Trigger**: Click **Execute Workflow** to test manually.

---

> **Date**: December 24, 2025  
> **Developer Note**: Ensure the `alertType` logic in the Code Node is tested against various temperatures (Heat > 32°C, Frost < 0°C) to verify accuracy.

