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


### 🔑 n8n Credentials Setup
To connect to your database, use the **Supabase Node** with the following details:
*   **Required Fields**: `Host`, `Database name`, `User`, `Password`, and `Port (5432)`.
*   **Location**: Found under **Project Settings > Database** in your Supabase dashboard.

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

## 📥 How to Import and Run
1.  **Download JSON**: Export your n8n workflow as a `.json` file.
2.  **Import**: In n8n, click **Workflow Menu (⋮) > Import from File**.
3.  **Connect Credentials**:
    *   Attach your **OpenWeatherMap** credentials.
    *   Attach your **Supabase** credentials.
    *   Attach your **SMTP** email credentials.
4.  **Trigger**:
    *   **Form Trigger**: Open the form URL, select a city, and submit.
    *   **Schedule Trigger**: Click **Execute Workflow** to test manually.

---

> **Date**: December 24, 2025  
> **Developer Note**: Ensure the `alertType` logic in the Code Node is tested against various temperatures (Heat > 32°C, Frost < 0°C) to verify accuracy.

