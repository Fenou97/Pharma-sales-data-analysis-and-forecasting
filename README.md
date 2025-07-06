# Pharma-sales-data-analysis-and-forecasting

## Table of contents
- [Project Overview](#project-overview)
- [Research Questions](#research-questions)
- [Data Sources](#data-sources)
- [Tools](#tools)


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



