# Hypothesis-Testing-Driven-Demand-Analysis-Riding-the-Demand-Wave-for-Yulu-s-Shared-E-Bikes



This README highlights the **data analytics and data science techniques** used to explore and model demand for Yulu’s shared electric cycles, based on an hourly bike-sharing dataset.

---

## 1. Project Objective

* **Business Questions**

  * Which variables significantly predict the demand for shared e-bikes in the Indian market?
  * How well do these variables explain demand fluctuations?
* **Analytical Goals**

  * Perform rigorous **Exploratory Data Analysis (EDA)**.
  * Conduct **statistical hypothesis tests** to understand group differences and associations.
  * Identify **key demand drivers** and translate them into **actionable recommendations**.

---

## 2. Dataset Overview

* **Rows / Columns**: 10,886 rows × 12 columns
* **Granularity**: Hourly rentals
* **Key Features**: `datetime`, `season`, `holiday`, `workingday`, `weather`, `temp`, `atemp`, `humidity`, `windspeed`, `casual`, `registered`, `count`

---

## 3. Tech Stack & Libraries

* **Python**: `pandas`, `numpy`
* **Visualization**: `matplotlib`, `seaborn`
* **Statistics & Tests**: `scipy.stats`, `statsmodels`

---

## 4. End-to-End Analytics Pipeline

### 4.1 Data Ingestion & Type Casting

* Loaded CSV via `pandas.read_csv()`.
* Converted categorical codes to labels (`season`, `holiday`, `workingday`, `weather`).
* Parsed `datetime` to `datetime64[ns]` and engineered `hour`, `year_month`, `date_numeric`.

### 4.2 Data Quality Checks

* **Shape Check**: `df.shape`
* **Duplicates**: `df.duplicated()`
* **Missing Values**: `df.isna().sum()`
* **Descriptives**: `df.describe()` for numerics, `df.describe(include=['object'])` for categoricals.

### 4.3 Outlier Detection & Treatment

* Visual **boxplots** for each numeric feature.
* Calculated **IQR-based fences** and used `np.clip()` to winsorize `humidity`, `windspeed`, `casual`, `registered`, and `totalcount`.
* Identified specific anomaly clusters (e.g., humidity = 0 on 2011‑10‑03).

### 4.4 Exploratory Data Analysis (EDA)

* **Univariate**: Histograms (`sns.histplot`), pie charts for categorical share.
* **Bivariate/Multivariate**:

  * **Time series** (`sns.lineplot`) by month/hour.
  * **Grouped bar charts** for season/holiday/workingday/weather splits.
  * **Heatmaps** for correlation matrix and pivot tables.
  * **Pairplot** to visually inspect relationships on a sample.

### 4.5 Correlation & Multicollinearity Handling

* Pearson **correlation heatmap** showed:

  * `temp` ≈ `atemp` (near-perfect collinearity)
  * `registered` ≈ `totalcount`
* **Feature Reduction**: Dropped `atemp` and `registered` to avoid redundancy.

### 4.6 Statistical Inference & Hypothesis Testing

| Business Question                 | Test Used                              | Why This Test?                              | Key Assumptions Checked                                                | Result                                        |
| --------------------------------- | -------------------------------------- | ------------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------------- |
| Weekday vs Weekend mean rentals   | **Two-sample t-test (independent)**    | Compare two group means                     | Normality (Shapiro/QQ), Equal variances (Levene)                       | Fail to reject H₀ → No significant difference |
| Rentals across Weather categories | **One-way ANOVA** & **Kruskal–Wallis** | Multiple groups comparison; non-normal data | Normality (Shapiro), Homogeneity (Levene) failed → used Kruskal–Wallis | Reject H₀ → Significant differences           |
| Rentals across Seasons            | **One-way ANOVA** & **Kruskal–Wallis** | Same rationale as above                     | Same checks; switched to non-parametric                                | Reject H₀ → Significant differences           |
| Weather vs Season association     | **Chi-square test of independence**    | Categorical association                     | Expected frequencies sufficient                                        | Reject H₀ → Significant dependency            |

### 4.7 Insight Synthesis & Business Translation

* Converted statistical outputs into concrete insights (e.g., which seasons/hours/weather drive demand) and **recommendations** (marketing, inventory planning, pricing).

---

## 5. Key Insights (from the Analysis)

* **Temporal Patterns**: Demand spikes 08–09 AM & 17–19 PM; November/December dips.
* **Seasonality**: Fall highest (\~30%), Spring lowest (\~15%).
* **Weather Sensitivity**: Clear/partly cloudy (>70% bookings); extreme weather negligible.
* **User Mix**: \~81% registered vs \~19% casual.
* **Environment Variables**: Demand highest with temp 10–30°C, humidity 40–90%, windspeed 5–20.
* **No Weekday/Weekend Mean Difference**, but total share is higher on working days (\~68%).
* **Significant Differences** across seasons & weather; strong dependency between the two.

---

## 6. Recommendations Snapshot

* Boost marketing & supply during **Fall & Summer**; offer **Spring discounts**.
* Trigger **weather-based notifications/ads** during optimal conditions.
* Stock **rain gear / all-weather bikes** for adverse Spring weather (condition 3).
* Ensure peak-hour availability (07–09 & 17–19) and address year-end dips (Nov/Dec) with promos.
* Leverage the **registered user base** for personalized offers and continuous feedback.



