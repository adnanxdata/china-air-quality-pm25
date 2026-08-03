## Project Summary — Beijing Air Quality Analysis

### Objective
Analyze air quality across 12 monitoring stations in Beijing (2013–2017),
identify patterns in pollution over time and by location, and build a model
to predict PM2.5 levels.

### Data
- Source: UCI Beijing Multi-Site Air-Quality Data
- 12 stations, hourly readings, March 2013 – Feb 2017
- Merged into a single dataset: 420,768 rows → cleaned to 404,364 rows
  (dropped ~3.9% of rows with unfillable long gaps in pollutant readings)
- Missing values handled via short-gap interpolation (≤6 hours) per station;
  longer gaps were left as NaN and later dropped rather than fabricated

### Key Findings (EDA)
1. **Seasonal pattern**: PM2.5 peaks in winter (Dec–Mar, ~90–105 µg/m³) due to
   coal heating season and stagnant, cold air trapping pollutants. Lowest in
   August (~52 µg/m³), aided by rain and stronger air mixing.
2. **Station comparison**: Urban-core stations (Nongzhanguan, Dongsi,
   Wanshouxigong, Gucheng) run dirtier than suburban/rural ones (Dingling,
   Huairou, Changping) — but the spread (~65–85 µg/m³) is modest compared to
   the seasonal swing.
3. **Correlations**: PM2.5 strongly correlates with PM10 (0.88), CO (0.79),
   and NO2 (0.67) — shared pollution sources. Wind speed correlates
   negatively with PM2.5 (-0.27), confirming dispersal effects. O3 behaves
   oppositely to other pollutants (positively correlated with temperature),
   consistent with it being a sunlight-driven summer pollutant rather than a
   heating-season one.

### Modeling
| Model | Inputs | MAE | R² |
|---|---|---|---|
| Linear Regression | All features | 20.4 | 0.856 |
| Random Forest | All features (incl. other pollutants) | 11.14 | 0.946 |
| Random Forest | Weather + station + month only | 21.26 | 0.804 |

- The full-feature Random Forest is the most accurate predictor, but ~90% of
  its decision-making relies on PM10 and CO — pollutants that move together
  with PM2.5 by nature, making this closer to a "measurement conversion"
  than a causal insight.
- The **weather-only model is the more meaningful result**: using only
  temperature, dew point, pressure, wind speed, rain, month, and station,
  it still explains ~80% of PM2.5's variation.

### Feature Importance (Weather-Only Model)
1. Dew point (DEWP) — strongest driver
2. Temperature (TEMP)
3. Month (seasonal effect)
4. Pressure (PRES)
5. Wind speed (WSPM)
6. Station identity — present but minor importance

### Conclusion
PM2.5 pollution in Beijing is driven primarily by **weather conditions**
rather than by station location. Cold, humid, still, high-pressure
conditions (typical of winter heating season) trap pollutants and drive
PM2.5 up; wind and rain disperse it. Location matters — urban stations run
consistently dirtier than suburban ones — but this effect is small relative
to the seasonal weather swing. A model using only weather and location data
can explain roughly 80% of PM2.5 behavior, without relying on other
pollutant readings.

### Limitations
- Dataset only covers Beijing (12 stations within one city), not multiple
  Chinese cities.
- ~3.9% of rows were dropped due to long missing-data gaps.
- The full-feature model's high accuracy is partly inflated by including
  correlated pollutants (PM10, CO) as inputs.
- Random Forest feature importance shows relative influence, not proven
  causation.