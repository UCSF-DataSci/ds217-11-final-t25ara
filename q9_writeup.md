# Q9: Writeup

**Phase 9:** Written Report  
**Points: 40 points**

**Focus:** Create a comprehensive written report documenting your analysis.

**Lecture Reference:** Lecture 11, Notebook 4 ([`11/demo/04_modeling_results.ipynb`](https://github.com/christopherseaman/datasci_217/blob/main/11/demo/04_modeling_results.ipynb)), Phase 9. Also see `example_report/report.md` for structure and level of detail.

---

## Objective

Create a comprehensive written report documenting your complete 9-phase data science workflow analysis.

---

## Deliverable

**File:** `report.md` - A comprehensive markdown report

**Location:** Save in the assignment root directory (same level as `q1_setup_exploration.md`, `q2_data_cleaning.md`, etc.)

**Note:** Focus on including all required sections (see below) and providing clear, comprehensive documentation. See the example report in `example_report/report.md` for structure and level of detail.

---

## Required Sections

Your report must include all of the following sections:

### 1. Executive Summary (1 paragraph)
This project used weather sensor data from Chicago beaches on Lake Michigan from April 1015 to November 2025. The analysis was divided into a 9-part work flow to understand seasonal and temporal patterns in weather in order to build a predictice model for air temperature. Mulitple models were tested:linear regression, random forest, and XGBoost. Out of all the models XGBoost had the best performance (R^2 = 0.2844), with linear regression coming in second (R^2 = 0.1790). 

### 2. Phase-by-Phase Findings
Document findings for each of the 9 phases:
- **Phase 1-2 (Q1):** Exploration findings, data quality issues
Data started with 196,381 rows and 18 columns with various measurements including: humidity, air temperature, barometric pressure, solar radiatio, wind speed, wind direction, and more. The data includes hourly measurements from April 2015 to November 2025 taken at three weather stations. Data quality issues were identified. The largest amount of missing values were in rain/percipitation related measurements, with ~37% of their values missing. Rain intensity, Total rain, Percipitation Type, Wet Bulb Temperature, and Heading all had 75,981 values missing. Other columns also had values missing, but overal less than 10% of the their total rows. Depending on value missing data was either forward filled, mean filled, or filled with 0. No duplicates were found. 
![Figure 1: Initial Data Exploration](output/q1_visualizations.png)
*Figure 1: Initial exploration visualizations showing distributions of air temperature, air temperature time series*
- **Phase 3 (Q2):** What was cleaned, how missing data/outliers were handled

Air Temperature: 75 missing values
  Method: Forward Fill, likely similar to previous measurement
  Result: All missing values filled
Barometric Pressure: 146 missing values
  Method: Forward Fill, likely similar to previous measurement
  Result: All missing values filled
Rain Intensity: 75981 missing values
  Method: Filled with 0 (absence of rain is expected)
  Result: All missing values filled
Total Rain: 75981 missing values
  Method: Filled with 0 (absence of rain is expected)
  Result: All missing values filled
Interval Rain: 75981 missing values
  Method: Filled with 0 (absence of rain is expected)
  Result: All missing values filled
Precipitation Type: 75981 missing values
  Method: Filled with 0 (no precipitation)
  Result: All missing values filled
Wet Bulb Temperature: 75981 missing values
  Method: Filled with 0 (no rain no wet bulb)
  Result: All missing values filled
Heading: 75981 missing values
  Method: Forward Fill, likely similar to previous measurement
  Result: All missing values filled
Wind Speed: 5 missing values
  Method: Forward Fill, likley similar to previous measurement
  Result: All missing values filled
Maximum Wind Speed: 5 missing values
  Method: Forward Fill, likley similar to previous measurement
  Result: All missing values filled
Solar Radiation: 13425 missing values
  Method: Mean, there was too many to forward fill so hope mean will suffice
  Result: All missing values filled

  Outlier Handling:
Solar Radiation: Detected values < 0
  Method: Replaced with NaN, then interpolated
  Invalid values: Negative values including -100000
  Result: Invalid values corrected by using mean imputation
  Method: Replaced with NaN, then filled with 0
  Invalid values: Negative values
Wind Speed: Detected values > 200 mph
  Method: Replaced with NaN, then interpolated with forward fill
Maximum Wind Speed: Detected values > 200 mph
  Method: Replaced with NaN, then interpolated with forward fill
  Result: Invalid values corrected
Barometric Pressure: Detected values outside [900, 1100] hPa
  Method: Replaced with NaN, then interpolated with forward fill
  Invalid values: Values 0.0 and 3098.5
  
  We were able to maintain the full data set while addressing the large number of missing values in many measurements. The missing 37% of data suggests there was some error in sensor collecting data specific to rain/precipitiation. 
- **Phase 4 (Q3):** Datetime parsing, temporal features extracted
Datetime parsing and temporal feature extraction was conducted for analysis. The column 'Measurement Timestamp' was set as index and used to extract temporal features and preform temporal analysis. Date Range After Datetime Parsin starts at 2015-04-25 09:00:00 and ends at 2025-12-04 20:00:00 for a duration of 3876 days.
Temporal features extracted include: Measurement Timestamp,hour,day_name(monday-sunday),day_of_week (0-6),month,year,is_weekend(binary, 0 = no, 1 = yes).

After datetime parsing 196381 rows remained.
- **Phase 5 (Q4):** Derived features created, rolling windows calculated
Features with rolling window statistics were created to understand seasonal and temporal relationships
Created Features Include:
total_rain_rolling_24hr_mean
total_rain_rolling_24hr_std
wind_speed_rolling_24_mean
wind_speed_rolling_24_std
Humidity_rolling_24_mean
Humidity_rolling_24_std
Pressure_rolling_24_mean
Pressure_rolling_24_std

Target variable was Air Temperature. Only rolling windows of predictor variables were created. 

- **Phase 6 (Q5):** Trends identified, seasonal patterns, correlations
TEMPORAL TRENDS:
Air temperature shows clear seasonal patterns with strong yearly cycles, with higher temperatures in summer months, peaking around 20-25°C. Winter saw lower temperatures, dropping to approximately 0-5°C
Wind speed shows a gradual decreasing trend over the 10-year period. Wind patterns show moderate seasonal variation with slightly higher speeds in winter months. Total rainfall is highly variable, with several extreme rainfall events visible, particularly in 2016-2018 period (400+ mm).However, rainfall frequency and intensity appear to decrease in later years (2020-2026).Barometric pressure remains relatively stable throughout the period, fluctuating within a narrow range of approximately 990-1000 hPa

SEASONAL PATTERNS:
Air temperature has strong annual cyclicity with consistent amplitude, the oscillations show predictable seasonal pattern repeating yearly.Wind speed shows weaker seasonal patterns compared to temperature. Barometric pressure shows minor fluctuations without strong seasonal patterns.

CORRELATIONS:
Strongest Positive Correlations:
-Air Temperature vs Total Rain: 0.33 (moderate positive correlation)
-Humidity vs Total Rain: 0.14 (weak positive correlation)

Strongest Negative Correlations:
-Air Temperature vs Barometric Pressure: -0.25 (weak-moderate negative correlation)
  Lower pressure tends to occur with higher temperatures
-Air Temperature vs Wind Speed: -0.22 (weak-moderate negative correlation)
  Wind speeds slightly decrease as temperatures increase
-Humidity vs Barometric Pressure: -0.19 (weak negative correlation)
![Figure 2: Pattern Analysis](output/q5_patterns.png)
*Figure 2: Monthly air temperature trends line plot. Monthly wind speed trends line plot. Monthly total rain trends. Weekly Air temperature trends. Monthly wind speed trends bar plot. Barometric pressure line plot.Correlation heatmap with features of interest.*

- **Phase 7 (Q6):** Train/test split approach, features selected
Features were filtered to numeric measurements. Features that were derived from target or too strongly correlated were excluded from analysis. 
Excluded variables include: Wet Bulb Temperature, Station Name, Wind Category

Split Method: Temporal (80/20 split by time)
Training Set Size: 400008 samples
Test Set Size: 100002 samples
Training date range:2015-04-25 09:00:00 to 2022-11-03 03:00:00
Test date range:2022-11-03 05:00:00 to 2025-12-04 20:00:00
Number of Features: 8
Target Variable: Air Temperature

Features Selected included:'Wind Speed', 'Total Rain', 'Barometric Pressure', 'Solar Radiation','Rain Intensity','Wind Direction','Maximum Wind Speed', 'month'

- **Phase 8 (Q7):** Models trained, performance metrics, feature importance
Three models were trained and evaluated: Linear Regression, Random Forest, and XGBoost. 

Model Performance: 

LINEAR REGRESSION:
              Metric Training    Test
0   RMSE     8.30    8.94
1    MAE     6.76    7.35
2     R²   0.3765  0.2285
XG Boost:
              Metric Training    Test
0   RMSE     4.48    4.93
1    MAE     3.45    3.75
2     R²   0.8183  0.7653
RANDOM FOREST:
              Metric Training    Test
0   RMSE     6.27    6.16
1    MAE     4.86    4.77
2     R²   0.6442  0.6337

Feature Importance:
1.Month - 0.6699236
2.Total Rain - 0.13293962
3.Barometric Pressure - 0.08859083
4.Solar Radiation - 0.054000244
5.Wind Direction - 0.034771092
6.Wind Speed - 0.019774612
7.Rain Intensity - 0.0
8.Maximum Wind Speed - 0.0

Month feature accounts for majority of feature importance (67%). This naturally is understood as months are indicative of the season,and therefore strong predictors of temperature. The top 3 features account for 89% of total importance

![Figure 3: Model Performance](output/q8_final_visualizations.png)
*Figure 3: Final visualizations showing model performance comparison, Linear Regression predictions vs actual values, feature importance, and prediction vs actual for best-performing XGBoost model.*
- **Phase 9 (Q8):** Final visualizations, summary of key findings

### 3. Visualizations (at least 5 figures with captions)
- Embed visualizations from your analysis
- Each figure must have:
  - Image embedded using: `![Figure N: Description](output/filename.png)`
  - Caption explaining what the figure shows
- Required visualizations:
  - At least 2 time series plots (from Q1, Q5, or Q8)
  - At least 3 additional plots (distributions, correlations, model performance, etc.)

### 4. Model Results
- Performance metrics table (use markdown table format)
- Feature importance discussion
- Model interpretation (what do R², RMSE, MAE mean in context?)
- Model comparison

### 5. Time Series Patterns
- Trends over time (increasing/decreasing/stable)
- Seasonal patterns (daily, weekly, monthly cycles)
- Temporal relationships between variables
- Any anomalies or interesting temporal features

### 6. Limitations & Next Steps
- Data quality issues that couldn't be fully addressed
- Model limitations
- Additional features that could be created
- Additional analysis that would be valuable
- How results could be validated or extended

---

## Format Requirements

### File Format
- Markdown (`.md`) with embedded images
- Professional presentation
- Error-free writing

### Image Embedding
- Save visualizations to `output/` directory
- Embed using: `![Figure 1: Description](output/figure1.png)`
- All images must have captions (either in alt text or as separate text)

### Tables
- Use markdown table format (recommended for model results)
- Example:
  ```markdown
  | Model | R² | RMSE | MAE |
  |-------|----|----|----|
  | Linear Regression | 0.XX | X.XX | X.XX |
  | Random Forest | 0.XX | X.XX | X.XX |
  ```

### Structure
- Include all required sections (see Required Sections above)
- Focus on quality over quantity
- See `example_report/report.md` for structure and level of detail

---

## Requirements Checklist

- [ ] Executive summary written (1 paragraph)
- [ ] Phase-by-phase findings documented (all 9 phases)
- [ ] At least 5 visualizations included with captions
- [ ] Model results presented (metrics, feature importance, interpretation)
- [ ] Time series patterns identified and explained
- [ ] Limitations and next steps discussed
- [ ] Professional formatting and presentation
- [ ] File saved as `report.md` in assignment root directory

---

## Grading Rubric

Your writeup will be evaluated on:

**Documentation Quality (12 points)**
- Process Explanation (4 points): Clear, step-by-step description of entire workflow
- Decision Rationale (4 points): All major decisions explained with reasoning
- Professional Presentation (4 points): Well-formatted markdown, error-free

**Visualizations & Tables (14 points)**
- Time Series Visualizations (5 points): At least 2 time series plots with clear labels
- Other Visualizations (5 points): At least 3 additional plots with appropriate choices
- Tables (2 points): Model results and key findings in well-formatted tables
- Best Practices (2 points): All visualizations have titles, axis labels, legends, captions

**Interpretation & Insights (14 points)**
- EDA Findings (5 points): Key patterns from exploration phase clearly summarized
- Time Series Patterns (5 points): Trends, seasonality, temporal relationships identified
- Model Interpretation (2 points): Model performance metrics interpreted correctly
- Limitations & Conclusions (2 points): Honest assessment of limitations and conclusions


## Checkpoint

After Q9, you should have:
- [ ] Complete written report (`report.md`)
- [ ] All required sections included
- [ ] At least 5 visualizations with captions
- [ ] Professional formatting
- [ ] Report saved in assignment root directory

---

**Congratulations!** You've completed the full 9-phase data science workflow. Review the submission checklist in `README.md` before submitting.

