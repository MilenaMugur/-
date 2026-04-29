# Project Report: Real Estate Price Prediction (Moscow & Region)

## 1. Project Overview
This project aims to predict the market price of apartments in Moscow and the Moscow Region based on property characteristics. The goal is to build a regression model that accurately estimates housing costs using data scraped from a major real estate aggregator (Cian).

## 2. Data Collection and Description
- **Data Source:** Scraped from Cian.ru using the `cianparser` library.
- **Dataset:** Contains information about ~6500 apartments (Moscow and suburbs).
- **Key Features:**
  - `location` (City/District)
  - `floor`, `floors_count`
  - `rooms_count`, `total_meters` (Area)
  - `district`, `underground` (Metro station proximity)
  - `residential_complex`
- **Target Variable:** `price` (in Rubles).

## 3. Data Preprocessing & Cleaning
The data required significant cleaning before modeling:
1. **Handling Missing Values:** 
   - `district` was imputed based on the `underground` station mapping.
   - `residential_complex` and `underground` missing values were filled with "absent" category.
   - Rows with completely missing location data were dropped.
2. **Outlier Removal:** 
   - Removed anomalous prices (above 20 million Rubles) and unrealistic physical characteristics (e.g., negative floors, area < 15 sq.m).
   - Removed columns with constant values or high cardinality/low information (`url`, `deal_type`, `author`).
3. **Feature Engineering:** Created categories for `floor` and `floors_count`.

## 4. Exploratory Data Analysis (EDA) & Hypotheses
- **Distribution:** Price distribution is heavily skewed (right-tailed). 
- **Hypothesis Testing:**
  - *Normality:* Shapiro-Wilk test confirmed prices do not follow a normal distribution.
  - *Floor Impact:* We tested the hypothesis that apartments on the first or last floor are cheaper. The T-test (p-value < 0.05) allowed us to reject the null hypothesis, suggesting floor position does impact price in this dataset.
  - *Location:* Price difference between Moscow and MO was statistically significant.

## 5. Modeling and Evaluation
We compared several regression models to predict the price:
- **Linear Regression:** Baseline model.
- **Decision Tree:** Captures non-linear relationships but prone to overfitting.
- **Random Forest:** Ensemble method improving stability.
- **Gradient Boosting (CatBoost, XGBoost):** State-of-the-art approach for tabular data.

### Results
The **CatBoost Regressor** demonstrated the best performance after hyperparameter tuning:
- **Metric:** Mean Absolute Error (MAE)
- **Best Result:** ~0.95 million Rubles error on cross-validation.

## 6. Conclusion
The project successfully demonstrates a pipeline from web scraping to model deployment. CatBoost proved to be the most effective tool for handling categorical features like districts and metro stations without extensive preprocessing.

**Author:** Milena Mugur  
**HSE University**
