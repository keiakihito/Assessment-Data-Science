# Assessment-Data-Science

This project is a technical interview submission for the PA Accelerator internship 2025. The PM Accelerator addresses their company mission **to break down financial barriers and achieve education fairness.**

---

## Table of Contents
1. [Overview](#overview)
2. [Methodology](#methodology)
    - [Step 1: Data Cleaning and Preprocessing](#step-1-data-cleaning-and-preprocessing)
    - [Step 2: Exploratory Data Analysis (EDA)](#step-2-exploratory-data-analysis-eda)
    - [Step 3: Building Forecasting Models](#step-3-building-forecasting-models)
3. [Benchmark Results](#benchmark-results)
4. [Visualizations](#visualizations)
5. [Conclusion](#conclusion)

---

## Overview
This project aims to:
1. Showcase basic data cleaning and preprocessing techniques.
2. Perform exploratory data analysis (EDA).
3. Build a basic forecasting model to predict temperature using LightGBM, ARIMA, and Prophet models. The models' performance is evaluated using metrics such as MAE, RMSE, and MAPE. An ensemble approach is employed to combine predictions and enhance accuracy.

### Dataset
The project uses data from the [World Weather Repository](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository/code) on Kaggle.

### Objectives
- Uncover patterns in weather data and test hypotheses.
- Create visualizations to aid in understanding key relationships.
- Build accurate forecasting models for temperature prediction.

---

## Methodology

### Step 1: Data Cleaning and Preprocessing
- **Data Summary**: Used `.describe()` to summarize the dataset.
- **NaN and Duplicates Check**: Verified there are no missing or duplicate values.
- **Standardization**: Standardized country names and reviewed unique values.

### Step 2: Exploratory Data Analysis (EDA)
To uncover trends and validate hypotheses:
- **Correlation Analysis**: Explored relationships between key features:
    - `temperature_celsius`
    - `precip_mm`
    - `humidity`
    - `cloud`

#### Hypotheses:
1. **High UV index is associated with warmer and drier weather.**
2. **Increased ozone levels reduce cloud cover and humidity, possibly leading to clearer skies.**

#### Visualizations:
- **UV Index Distribution by Temperature Range** (Figure 1)
    - Low UV index (0–2) is present across all temperature ranges.
    - Higher UV index values are correlated with warmer temperatures (20–40°C).

- **Air Quality Ozone Distribution by Humidity Range** (Figure 2)
    - At lower ozone levels (<100), distributions are consistent across humidity ranges.
    - At higher ozone levels (>100), low-humidity environments (0–50%) dominate.

- **Temperature and Precipitation Relationships**
    - Temperature data follows a normal distribution, peaking around 20–30°C.
    - Precipitation values are concentrated near 0–0.5mm, with a few outliers.

### Step 3: Building Forecasting Models
- The model predicts temperature for a specific country. In this model training, we chose the United States of America.
- The model predicts the `temperature_celsius`.

#### i. Data Preparation
- Extract necessary features to predict `temperature_celsius`.
- The features are chosen based on the correlation value in the Step 2 EDA phase.

#### ii. Feature Engineering
To enhance the model's performance, we extract and create meaningful features from the dataset. The following transformations are applied:

- **Time-Based Features**: Leverage the timestamp (`last_updated`) to capture potential temporal patterns.
- **Lag Features**: Include historical values of the target variable (`temperature_celsius`) to model temporal dependencies.
- **Rolling Statistics**: Capture trends and variability over a moving window.
- **Interaction Features**: Combine existing features to capture interactions.
- **Derived Features**: New features calculated from existing data.
- Handle missing values through imputation.

#### iii. Outlier Removal
**Visualize outliers with a box plot.**

![Outliers Before Capping](images/outlier_before_cap.png)

- Set lower boundary with 0.25 quantile.
- Set higher boundary with 0.75 quantile.
- Replace outliers with lower and higher boundary values instead of dropping them.

**Visualize outliers after capping.**

![Outliers After Capping](images/outlier_after_cap.png)

- Refine and polish training dataset:
    - Delete duplicated rows.
    - Sort oldest to newest for Time Series Training.
    - **Note: Sorting oldest to newest is crucial for time series data training.**

#### iv. Model Training
In this phase, we evaluate the performance of three models and their ensemble:

- **LightGBM**: Used tree-based boosting for regression.
- **ARIMA**: Auto-tuned ARIMA model for time-series forecasting.
- **Prophet**: Trend and seasonality-based model.
- **Ensemble**: Combined predictions using weighted averages.

The models are assessed based on the following metrics:

- **MAE**: Mean Absolute Error
- **RMSE**: Root Mean Squared Error
- **MAPE**: Mean Absolute Percentage Error

---

## Benchmark Results
### Metrics
| Model    | MAE  | RMSE | MAPE   |
|----------|------|------|--------|
| LightGBM | 0.52 | 1.18 | 14.90% |
| ARIMA    | 1.63 | 2.14 | 26.85% |
| Prophet  | 3.96 | 4.87 | 49.12% |
| Ensemble | 1.41 | 1.96 | 23.06% |

### Charts
**Actual vs Predicted Temperature with LightGBM**<br>
![LightGBM Chart](images/LightGBM.png)

**Actual vs Predicted Temperature with ARIMA**<br>
![ARIMA Chart](images/ARIMA.png)

**Actual vs Predicted Temperature with Prophet**<br>
![Prophet Chart](images/Prophet.png)

**Actual vs Predicted Temperature with Ensemble**<br>
![Ensemble Chart](images/Ensemble.png)

---

## Visualizations
### Figure 1: UV Index Distribution by Temperature Range
![UV Index Distribution by Temperature Range](images/UV_index_distribution_by_temperature_Range.png)

### Figure 2: Air Quality Ozone Distribution by Humidity Range
![Air Quality Ozone Distribution by Humidity Range](images/Air_Quality_Ozone_Distribution_by_Humidity_Range.png)

### Figure 3: Outliers Before Capping
![Outliers Before Capping](images/outlier_before_cap.png)

### Figure 4: Outliers After Capping
![Outliers After Capping](images/outlier_after_cap.png)

### Figure 5: Temperature Distribution
![Temperature Distribution](images/Temperature_Distribution.png)

### Figure 6: Log-Transformed Precipitation Distribution
![Log-Transformed Precipitation Distribution](images/Log-Tranceformed_Precipitation_Distribution.png)

### Figure 7: Temperature vs Precipitation
![Temperature vs Precipitation](images/Temperature_vs_Previpitation.png)

### Figure 8: Temperature and Precipitation Heatmap
![Temperature and Precipitation Heatmap](images/Temperature_and_Precipitation_Heatmap.png)

---

## Conclusion
We explored various models, including individual models (LightGBM, ARIMA, Prophet) and ensemble methods, and benchmarked their performance using three metrics: MAE, RMSE, and MAPE. Among these, LightGBM demonstrated the most accurate and consistent predictions, closely aligning with the actual data.

### For Future Enhancements
- **Feature Engineering**: Explore new features or transformations to improve predictive accuracy.
- **Hyperparameter Optimization**: Utilize tools such as Optuna for automated hyperparameter tuning to refine LightGBM's performance.

