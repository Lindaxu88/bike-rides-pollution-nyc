# Bike Rides & Pollution in NYC

This project analyzes the relationship between **Citi Bike ride patterns** and **air quality (PM2.5 / AQI)** across New York City from **2017 to 2019**.  
It integrates trip data from Citi Bike with EPA air quality monitoring data, and processes them through a **Spark + DuckDB pipeline**, with final visualization in Tableau.

---

## Background

- How do **bike ride patterns** and **air quality** vary across NYC counties (Bronx, Kings, New York, Queens)?  
- Air Quality Index (AQI) levels follow U.S. Environmental Protection Agency (EPA) standards.  
- Citi Bike trip data was retrieved from the official [Citi Bike NYC system data](https://citibikenyc.com/system-data).  
- Study period: **Morning rush hours (7–9 AM), 2017–2019**.  

---

## Data Sources
- **Citi Bike NYC Trip Data (2017–2019)**: Ride ID, type, start/end stations, timestamps, user type, coordinates  
- **Air Quality Data (EPA)**: Date, county, PM2.5 concentration, AQI value, site coordinates  

---

## Data Cleaning
- Removed irrelevant columns (e.g., bike ID, site ID, source name)  
- Standardized date and time formats  
- Removed duplicate records and unrealistic values (e.g., outlier trip durations)  
- Dropped or handled missing values  

---

## Pipeline Overview

### 1. Spark ETL
- Load trip & air quality CSVs into Spark DataFrames  
- Convert dates, filter columns, assign ride IDs  
- Join on **date + nearest AQ monitoring site** (Euclidean distance)  
- Output saved as Parquet  

### 2. DuckDB
- Raw parquet loaded into `citibike_rides` table  
- Derived views:
  - `measures_by_station` → Aggregates station-level data  
  - `measure_by_county` → Daily trips per county  
  - `pollution_vs_bike_usage` → Bike usage vs pollution over time  
  - `aqi_trip_duration` → Categorizes AQI levels for each trip  

### 3. Visualization (Tableau)
- **Heatmap of AQI** at Citi Bike stations → highest pollution in Manhattan  
- **Bike ride density maps** → densest clusters in Manhattan, fewer in Queens/Bronx  
- **Weekly usage vs AQI trends** → weak correlation, weather & traffic stronger factors  
- **Boxplot of trip duration vs AQI** → trip duration variation under "Moderate" AQI, no strong causal link  

---

## Visualizations

### AQI Distribution by Station
![AQI Distribution by Station](images/AQI.jpg)

### Number of Rides by Station
![Number of Rides by Station](images/Rides-by-Station.jpg)

---

## Key Insights
- High bike usage does **not** guarantee better air quality.  
- New York County (Manhattan) has the highest bike rides but similar or worse AQI than other counties.  
- AQI remains high even in areas with more bike rides — congestion and emissions dominate.  
- Trip durations vary slightly across AQI categories, but external factors (traffic, weather, routes) likely drive differences.  

---

## Repository Structure

```
bike-rides-pollution-nyc/
│── data/              # Sample datasets
│── src/               # Spark scripts
│── notebooks/         # Jupyter notebooks
│── sql/               # DuckDB SQL transformations
│── scripts/           # Pipeline shell script
│── results/           # DuckDB outputs
│── presentation/      # Project slides
│── images/            # Visualizations
│── README.md
│── requirements.txt
```

---

## How to Run

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Run Pipeline
```bash
cd scripts
bash pipeline.sh
```

This will:
1. Run Spark to preprocess Citi Bike + Air Quality data  
2. Save outputs to Parquet  
3. Load data into DuckDB (`final.db`)  

---

## Requirements
- pyspark  
- duckdb  
- pandas  
- numpy  
- matplotlib  
- jupyter  
- seaborn  

---

## Author
**Yeman Xu**
