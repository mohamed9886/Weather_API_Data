# 🌤️ Weather Data ETL Pipeline - Egyptian Cities

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)
![API](https://img.shields.io/badge/OpenWeatherMap_API-E96E50?style=for-the-badge&logo=openweathermap&logoColor=white)

## 📌 Project Overview
This project is an automated **End-to-End ETL (Extract, Transform, Load) pipeline** built with Python. It extracts real-time weather data for 10 major Egyptian cities from the **OpenWeatherMap API**, performs data wrangling and categorical transformations using **Pandas**, and securely loads the final structured dataset into an **Azure SQL Server** database.

## ⚙️ ETL Pipeline Architecture

### 1. Extract 📥
- Connected to the OpenWeatherMap REST API using the Python `requests` library.
- Fetched real-time weather metrics (Temperature, Humidity, Pressure, Coordinates) for 10 targeted cities: *Cairo, Giza, Alexandria, Port Said, Suez, Ismailia, Mansoura, Tanta, Luxor, Aswan*.

### 2. Transform 🔄
Cleaned and enriched the raw JSON data using `pandas`:
- **Temperature Conversion:** Converted raw temperature from Kelvin to Celsius (°C).
- **Geographical Mapping:** Engineered a `region` column to map each city to its respective Egyptian geographical zone (e.g., Greater Cairo, Lower Egypt, Canal, Sinai, Red Sea, Upper Egypt).
- **Categorization:** 
  - `TemperatureCategory`: Classified as **Hot** (>= 30°C) or **Cold** (< 30°C).
  - `HumidityCategory`: Classified as **High** (>= 30%) or **Low** (< 30%).
- **Metadata Addition:** Appended an `IngestionTime` timestamp column to track when the data was processed.
- **Data Cleansing:** Dropped redundant columns (e.g., original Kelvin temperature) to optimize database storage.

### 3. Load 📤
- Established a secure connection to **Azure SQL Server** using `pyodbc`.
- Utilized `python-dotenv` to securely manage database credentials and API keys.
- Dynamically created the target SQL table and inserted the transformed data records using batch execution (`executemany`).

## 🛠️ Technologies & Libraries Used
- **Language:** Python 3
- **Data Manipulation:** `pandas`
- **API Integration:** `requests`
- **Database Connection:** `pyodbc`
- **Environment Management:** `python-dotenv`
- **Database:** Microsoft SQL Server (Azure)

## 🗂️ Data Dictionary (Final SQL Schema)

| Column Name | Data Type | Description |
|---|---|---|
| `country` | VARCHAR | Country name (Egypt) |
| `city` | VARCHAR | Name of the targeted city |
| `Longitude` | VARCHAR | City's geographical longitude |
| `Latitude` | VARCHAR | City's geographical latitude |
| `humidity` | VARCHAR | Humidity percentage (%) |
| `pressure` | VARCHAR | Atmospheric pressure (hPa) |
| `temp_in_cel` | VARCHAR | Temperature converted to Celsius (°C) |
| `region` | VARCHAR | Mapped geographical region |
| `TemperatureCategory`| VARCHAR | Categorical indicator (Hot / Cold) |
| `humidityCategory` | VARCHAR | Categorical indicator (High / Low) |
| `IngestionTime` | VARCHAR | Timestamp of the ETL execution |
