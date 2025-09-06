# Pharma-sales-data-analysis-and-forecasting

## Table of contents
- [Project Overview](#project-overview)
- [Research Questions](#research-questions)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Time series forecasting](#Time-series-forecasting)

## Project Overview

This project focuses on time series analysis and forecasting of daily sales data for the pharmaceutical products using R. The goal is to understand seasonal trends and generate short-term forecasts to support inventory and sales planning decisions.

## Research Questions

- What are the expected daily sales for each product over the next 3 months?
- Do sales of certain products increase on specific weekdays or during particular months?
- Which products are most similar in their sales behavior (based on clustering time series)?

## Dataset Overview

The dataset includes daily sales quantities from 2014 to 2019 for several drug categories, with timestamp features like: datum (date), , Temporal features: Year, Month, Hour, Weekday.Name, Product sales columns: M01ABsort (Sales volume of anti-inflammatory and antirheumatic products, non-steroids, Acetic acid derivatives and related), M01AEsort (Sales volume of anti-inflammatory and antirheumatic products, non-steroids, Propionic acid derivatives), N02BAsort (Sales volume of other analgesics and antipyretics, Salicylic acid and derivatives), N02BEsort (Sales volume of other analgesics and antipyretics, Pyrazolones and Anilides), N05Bsort (Sales volume of psycholeptics drugs, Anxiolytic drugs), N05Csort (Sales volume of psycholeptics drugs, Hypnotics and sedatives drugs), R03sort (Sales volume of drugs for obstructive airway diseases), R06sort (Sales volume of antihistamines for systemic use). 

## Tools
- Excel for data assessment 
- PowerBI for data summary and data distribution 
- R for data manipualting and Model bulding

## Exploratory Data Analysis

- ### Data Summary and Data Distribution

In PowerBI, Data profiling is used to examine the data and collect statistics informative summaries about it. It helped us to understand the  data structure, to assess data quality, and evaluate data distribution and summary statistics. 

*M01ABsort (Sales volume of anti-inflammatory and antirheumatic products, non-steroids, Acetic acid derivatives and related)*

![EDA 1](https://github.com/user-attachments/assets/6b78f043-9ec6-43b9-816f-3c9ea9cb290c)

*M01AEsort (Sales volume of anti-inflammatory and antirheumatic products, non-steroids, Propionic acid derivatives)*

![EDA2](https://github.com/user-attachments/assets/3f36063e-1f08-4851-8c09-ec631173f299)

*N05Bsort (Sales volume of psycholeptics drugs, Anxiolytic drugs)*

![EDA3](https://github.com/user-attachments/assets/b162819b-fb11-4ed5-81bf-00f5f27d1715)

*datum*

![ED4](https://github.com/user-attachments/assets/2886ad26-fb8a-41d7-bbeb-dbf1200552e8)

- ### Pattern Recognition and Seasonality

![EDA 5](https://github.com/user-attachments/assets/a93604d5-a22a-4e08-9692-abf384c9680f)

We are analyzing historical sales values (sums) for different drug categories across years (2014–2019), without using any external variables. This is purely time-driven pattern recognition.

|Drug Class|Pattern|Seasonality (visible?)|
|----------|-------|----------------------|
|N05C - Psycholeptics drugs, Hypnotics and sedatives drugs|Fluctuating decline|Not clear|
|R03 - Drugs for obstructive airway diseases|Rising with dip in 2019|Not obvious|
|R06 - Antihistamines for systemic use|Rising with clear 2018 peak|Possible seasonal peak in allergy years|
|N05B - Psycholeptics drugs, Anxiolytic drugs|Steady decline|No seasonality|
|N02BE- Other analgesics and antipyretics, Pyrazolones and Anilides|Two similar plots, fluctuating with peaks in 2016|Possibly seasonal or outbreak-driven|

### Time series forecasting

There are many different methods and approaches to time-series forecasting. **ARIMA** (AutoRegressive Integrated Moving Average) is a classical statistical model that forecasts time series data by combining past values, differencing to handle trends, and past errors. Its seasonal extension **(SARIMA)** is powerful for capturing recurring patterns like yearly or weekly seasonality. ARIMA works best on stable, well-structured series with clear trend or seasonal behavior, but it requires the data to be stationary and is less flexible when irregular events or sudden shifts occur.

In contrast, **Prophet**, developed by Facebook, is a decomposition-based model that represents time series as the sum of trend, seasonality, and holiday effects. It is more robust to missing data, outliers, and abrupt changes, while also making it easy to add external regressors such as holidays or outbreak indicators. Prophet excels when demand is influenced by strong seasonal patterns and external events, though it may be less precise than ARIMA for purely statistical, stable series.

|Drug Class|Observed Pattern|Better Model|Reason|
|----------|----------------|------------|------|
|N05B (Anxiolytics)|Steady decline, no clear seasonality|ARIMA / ETS|Trend-only series; ARIMA handles smooth declines better than Prophet|
|N05C (Hypnotics/Sedatives)|Fluctuating decline, weak seasonality|ARIMA|Mostly trend-driven, ARIMA captures trend shifts; Prophet adds little value here|
|R03 (Airway Diseases)|Rising trend with irregular spikes|Prophet|Robust to sudden shocks/outbreaks; can include external regressors (e.g., flu seasons)|
|R06 (Antihistamines)|Strong seasonal allergy pattern|SARIMA / Prophet|Both capture seasonality well; Prophet easier if adding holiday/weather regressors|
|N02BE (Analgesics/Antipyretics)|Seasonal with outbreak-driven peaks|Prophet|Handles irregular spikes and events better; ARIMA struggles with sudden jumps|

### Data Manipulation
- Properly convert the datum column into a Date object
```R
#Load the data
Pharma<-read.csv("C:/documents/Portoflio/pharma sales analysis and forecasting/salesdaily.csv", stringsAsFactors = FALSE)
View(Pharma)
# Convert date because date is in m/d/yyyy format
Pharma <- Pharma %>%
  mutate(datum = mdy(datum)) 
```
![Pharma Data](https://github.com/user-attachments/assets/091a0e93-42a0-4279-93c3-bdffb7878ca6)
![Date Object](https://github.com/user-attachments/assets/b2cce5f2-75aa-4336-92b9-d3a0c963ebb7)

- The  data is wide (many columns for different variables or categories). So, we convered it to long format (one column for variable names, one for values).
```R
Pharma_long <- Pharma %>%
  pivot_longer(cols = c(M01AB, M01AE, N02BA, N02BE, N05B, N05C, R03, R06),
               names_to = "DrugClass",
               values_to = "UnitsSold")
View (Pharma_long)
```
![Long Format data](https://github.com/user-attachments/assets/8126d82b-6b3d-4b1d-98ec-160e0d701788)

- **Aggregation**

The raw data is daily. Seasonality is usually observed over regular intervals like months, weeks, or quarters. We will combine all the daily observations in a month into a single value, which will make patterns easier to see. Without aggregation, the data will be too “noisy” to see trends or seasonal patterns.

```R
Pharma <- Pharma%>%
  mutate(total_sales = M01AB + M01AE + N02BA + N02BE + N05B + N05C + R03 + R06)
```
![Aggregate](https://github.com/user-attachments/assets/cf127e61-da58-4a89-80e9-87ffd09bbb40)

- **Convert to a time series (ts) object**
  
R’s time series functions (like decompose(), forecast()) require a ts object which will mark the data as ordered in time and tells R the frequency (12 for monthly, 4 for quarterly).

```R
# Create time series object
ts_data <- ts(Pharma$total_sales, start = c(2014, 1), frequency = 365)
#Visualize
autoplot(ts_data) + ggtitle("Daily Total Sales") + ylab("Sales")
```
![Time Series](https://github.com/user-attachments/assets/7f604d35-73fb-41ff-bdb8-6276fed8b5bc)
