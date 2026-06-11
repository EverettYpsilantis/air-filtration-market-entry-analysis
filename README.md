# air-filtration-market-entry-analysis
XGBoost PM2.5 forecasting and market opportunity scoring for U.S. city prioritization using EPA, NOAA, and Census data

# Air Filtration Market Entry Analysis

XGBoost-powered PM2.5 forecasting and weighted market opportunity scoring 
to identify U.S. city launch priorities for an industrial air filtration company.

## Business Question

Which of five major U.S. cities: Los Angeles, New York, Houston, Phoenix, 
and Chicago, should be prioritized for initial product launch based on 
projected air quality burden and population exposure?

## Notebooks

### 1. `DSBI_EDA.ipynb`: Exploratory Data Analysis & Business Intelligence
Pulls and merges raw data from three sources: EPA AQS API (PM2.5, O3, CO, NO2 
across five cities, 2015–2024), NOAA Climate Data Online (monthly temperature 
and precipitation from airport weather stations), and U.S. Census annual 
population estimates. Produces city-level pollution burden comparisons, 
seasonality analysis, and climate context visuals. Output: a merged monthly 
panel dataset of 600 rows × 10 features used as model input.

### 2. `Model_Preparation_DSBI.ipynb`: Feature Engineering, Modeling & Scoring
Engineers temporal predictors including lag features (1, 3, 12-month), 
rolling averages (3 and 12-month), and cyclical seasonality encoding 
(sine/cosine month transforms). Trains and compares Linear Regression, 
Random Forest, and XGBoost using a time-based train/test split to prevent 
data leakage. XGBoost selected as final model (MAE ~1.40, RMSE ~1.83, R² ~0.67). 
Generates recursive 12-month PM2.5 forecasts per city and computes a weighted 
Market Opportunity Score translating forecasts into a business prioritization ranking.

## Market Opportunity Score
Score = 0.40 × Predicted Avg PM2.5
+ 0.25 × Population Exposure
+ 0.20 × High-Risk Months (forecast > 12 µg/m³)
+ 0.15 × Historical Persistence

## Final City Rankings

| Rank | City | Score | Rationale |
|------|------|-------|-----------|
| 1 | Los Angeles | 0.82 | Highest forecasted PM2.5, strong historical persistence, structural climate risk |
| 2 | New York | 0.48 | Largest population exposure scales total burden despite moderate pollution |
| 3 | Houston | 0.48 | Elevated PM2.5 tied to petrochemical industry; Phase 2 target |
| 4 | Phoenix | 0.19 | Desert heat drives high-risk months; smaller market scales score down |
| 5 | Chicago | 0.18 | Lowest forecasted PM2.5 and declining historical trend |

## Key Modeling Decisions

- **Time-based split** over random split to preserve real-world forecasting conditions
- **Cyclical encoding** (sin/cos) for month to capture seasonal PM2.5 patterns
- **Recursive forecasting**: each predicted month fed back as input for the next
- **Top features**: month_sin (0.160), PM2.5_lag_12 (0.138), population (0.092), month_cos (0.090)

## Data Sources
- EPA Air Quality System (AQS) API — ~66,000 daily records, 2015–2024
- NOAA Climate Data Online (CDO), monthly airport weather station data
- U.S. Census Bureau, annual city-level population estimates

## Tech Stack
Python, XGBoost, scikit-learn, pandas, numpy, matplotlib, seaborn, requests

## Presentation
See `City_Prioritization_for_Air_Filtration_Market_Entry_Slides.pdf` for 
the full client-facing deliverable.

## Notes
Raw API data files are excluded. EPA AQS and NOAA CDO data are publicly 
accessible via their respective developer portals.
