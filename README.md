# Beijing Air Quality Analysis & PM2.5 Prediction

An end-to-end data science project analyzing PM2.5 air pollution across 12 monitoring stations in Beijing from 2013 to 2017. The project combines exploratory data analysis, statistical relationships, and machine learning to understand pollution patterns and predict PM2.5 levels.

## 📌 Project Objective

The main objectives of this project are to:

- Analyze PM2.5 pollution patterns over time
- Investigate seasonal and station-level differences
- Examine relationships between pollutants and weather conditions
- Identify important factors associated with PM2.5 levels
- Build machine learning models to predict PM2.5
- Compare a full-feature model with a weather-only model

## 📊 Dataset

**Source:** UCI Beijing Multi-Site Air-Quality Data

The dataset contains hourly air-quality and meteorological measurements from 12 monitoring stations in Beijing.

- Period: March 2013 – February 2017
- Original observations: 420,768
- Cleaned observations: 404,364
- Monitoring stations: 12
- Target variable: PM2.5

### Main Variables

**Pollutants**
- PM2.5
- PM10
- SO2
- NO2
- CO
- O3

**Weather**
- Temperature (TEMP)
- Pressure (PRES)
- Dew Point (DEWP)
- Rainfall (RAIN)
- Wind Speed (WSPM)
- Wind Direction

## 🧹 Data Cleaning

The raw data contained missing observations, particularly in pollutant measurements.

The cleaning process included:

- Combining data from all 12 monitoring stations
- Handling short missing gaps using interpolation
- Interpolating gaps of up to 6 hours
- Leaving longer gaps as missing rather than fabricating values
- Removing remaining rows with unusable target values

Approximately 3.9% of the original observations were removed.

## 🔎 Exploratory Data Analysis

The analysis investigated:

- PM2.5 trends over time
- Monthly and seasonal pollution patterns
- Average PM2.5 levels by station
- Relationships between pollutants
- Relationships between pollution and weather conditions

### Key Findings

**Seasonality**

PM2.5 concentrations are substantially higher during winter and lower during summer. Winter monthly averages reach approximately 90–105 µg/m³, while August averages are around 52 µg/m³.

**Station Differences**

Urban stations such as Nongzhanguan, Dongsi, Wanshouxigong, and Gucheng generally show higher PM2.5 concentrations than suburban stations such as Dingling, Huairou, and Changping.

However, seasonal variation is considerably larger than the difference between stations.

**Pollutant Relationships**

PM2.5 shows strong positive correlations with:

- PM10: 0.88
- CO: 0.79
- NO2: 0.67

Wind speed has a negative relationship with PM2.5 (-0.27), consistent with the dispersal effect of stronger winds.

## 🤖 Machine Learning

Two Random Forest models were developed.

### Model 1 — Full Features

Uses pollutant, weather, station, and time-related features.

| Model | MAE | R² |
|---|---:|---:|
| Linear Regression | 20.40 | 0.856 |
| Random Forest — Full Features | 11.14 | 0.946 |
| Random Forest — Weather Only | 21.26 | 0.804 |

The full-feature Random Forest achieved the best predictive performance.

However, much of its predictive power comes from PM10 and CO, which are strongly correlated with PM2.5. Therefore, this model is highly accurate but is less useful for understanding PM2.5 from environmental conditions alone.

### Model 2 — Weather + Station Features

The second Random Forest uses only:

- Temperature
- Dew Point
- Pressure
- Wind Speed
- Rainfall
- Month
- Station

It achieved:

- **MAE: 21.26**
- **R²: 0.804**

This means the model explains approximately 80% of the variation in PM2.5 without using measurements of other pollutants.

## ⭐ Weather-Only Feature Importance

The most important features were:

1. Dew Point
2. Temperature
3. Month
4. Pressure
5. Wind Speed
6. Rainfall
7. Station-related features

The results indicate that weather and seasonal conditions play a major role in PM2.5 behavior.

## 📈 Main Conclusion

The analysis suggests that PM2.5 pollution in Beijing is strongly influenced by meteorological and seasonal conditions.

Cold, humid, relatively stagnant conditions associated with winter are linked with higher PM2.5 concentrations, while wind and rainfall contribute to pollutant dispersion.

Station location also matters, with urban stations generally showing higher pollution levels, but the effect is smaller than the overall seasonal variation.

The weather-only Random Forest demonstrates that approximately 80% of PM2.5 variation can be explained using weather, season, and station information without relying on other pollutant measurements.

## 📁 Project Structure

```text
Beijing Air Quality Analysis/
│
├── data/
│   ├── raw/
│   └── beijing_air_quality_cleaned.csv
│
├── figures/
│   ├── Avg PM2.5 by station.png
│   ├── Avg PM2.5 Over time (all stations).png
│   ├── Pollutants & weather variable correlation.png
│   ├── seasonal PM2.5 (all station, all year).png
│   ├── RF Actual vs Predicted PM2.5.png
│   ├── Top 10 Imp features (RF).png
│   └── Top10 features - Weather-only model.png
│
├── models/
│   └── Model files are excluded from GitHub because of their large size
│
├── notebooks/
│   └── explore.ipynb
│
├── journal.md
├── requirements.txt
├── LICENSE
├── .gitignore
└── README.md
