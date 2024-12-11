# Analyzing and Predicting Power Outage Duration 
by - Khushi Raghuvanshi

## Introduction
The project analyses the data of major power outages in the US from January 2000 to July 2006. As defined by the Department of Energy, the major outages refer to those that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 MW. Besides major outage data, this article also presents data on the geographical location of the outages, date and time of the outages, regional climatic information, land-use characteristics, electricity consumption patterns, and economic characteristics of the states affected by the outages. The dataset was accessed from [science direct's website](https://www.sciencedirect.com/science/article/pii/S2352340918307182). 

I will perform data cleaning and exploratory analysis on the data, and explore the question: **What factors influence the duration of power outages, and can we predict outage duration based on these factors?**

Power outages are a common occurrence, especially during extreme weather events or infrastructure failures. These outages can disrupt daily life, impact businesses, and even pose safety risks. By predicting outage durations, stakeholders can plan for contingencies, optimize resources, and enhance the resilience of the power grid.

### Dataset Overview

The dataset has 1534 rows, corresponding to the number of outages, and 57 columns. I will only focus on the following columns for my analysis:

| Columns | Description |
| ----------- | ----------- |
| U.S._STATE | Represents all the states in the continental U.S. |
| NERC.REGION | The North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| CLIMATE.REGION | U.S. Climate regions as specified by National Centers for Environmental Information (9 regions)|
| CLIMATE.CATEGORY | The climate episodes corresponding to the years with categories—“Warm”, “Cold” or “Normal” episodes of the climate based on a threshold of ± 0.5 °C ONI |
| OUTAGE.START.DATE | This indicates the day of the year when the outage event started  |
| OUTAGE.START.TIME | This indicates the time of the day when the outage event started  |
| OUTAGE.RESTORATION.DATE | This indicates the day of the year when power was restored to all the customers |
| OUTAGE.RESTORATION.TIME | This indicates the time of the day when power was restored to all the customers |
| CAUSE.CATEGORY | Categories of all the events causing the major power outages |
| OUTAGE.DURATION | Duration of outage events (in minutes) |
| POPULATION |Population in the U.S. state in a year |

## Data Cleaning and Exploratory Data Analysis

### Dataset Cleaning

For the data cleaning part of the project, here are the following steps I took:

- The columns that I considered unlikely to contribute to my analysis or irrelevant were dropped from the dataset. Those were: `variables`, `OBS`, `YEAR`, `MONTH`, `POSTAL.CODE`, `ANOMALY.LEVEL`, `CAUSE.CATEGORY.DETAIL`, `HURRICANE.NAMES`, `DEMAND.LOSS.MW`, `CUSTOMERS.AFFECTED`, `RES.PRICE`, `COM.PRICE`, `IND.PRICE`, `RES.SALES`, `COM.SALES`, `IND.SALES`, `TOTAL.PRICE`, `TOTAL.SALES`, `RES.PERCEN`, `COM.PERCEN`, `IND.PERCEN`, `RES.CUSTOMERS`, `COM.CUSTOMERS`, `IND.CUSTOMERS`, `TOTAL.CUSTOMERS`, `RES.CUST.PCT`, `COM.CUST.PCT`, `IND.CUST.PCT`, `PC.REALGSP.STATE`, `PC.REALGSP.USA`, `PC.REALGSP.REL`, `PC.REALGSP.CHANGE`, `UTIL.REALGSP`, `TOTAL.REALGSP`, `UTIL.CONTRI`, `PI.UTIL.OFUSA`, `POPPCT_URBAN`, `POPPCT_UC`, `POPDEN_URBAN`, `POPDEN_UC`, `POPDEN_RURAL`, `AREAPCT_URBAN`, `AREAPCT_UC`, `PCT_LAND`, `PCT_WATER_TOT`, `PCT_WATER_INLAND`

- Next, I combined the necessary columns which included combining 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' columns into a single datetime column OUTAGE.START. Similarly, 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME' were combined into a single datetime column 'OUTAGE.RESTORATION'.

- After this, I applied a lambda function to replaced the 0 values with NaN. All the 0 values in 'OUTAGE.DURATION' and 'POPULATION' were replaced with NaN.

- Then I created a new feature, 'TIME.OF.DAY' from extracting the hour from 'OUTAGE.START' and categorizing it into 4 features: Morning (6 AM to 12 PM), Afternoon: 12 PM to 6 PM, Evening: 6 PM to 10 PM, and Night: 10 PM to 6 AM.

These steps enhanced the dataset's usability, improved data quality, and provided additional context for exploratory analysis and machine learning.

The first few rows of the dataset are shown below:

| U.S._STATE   | NERC.REGION   | CLIMATE.REGION     | CLIMATE.CATEGORY   | CAUSE.CATEGORY     |   OUTAGE.DURATION |   POPULATION | OUTAGE.START        | OUTAGE.RESTORATION   | TIME.OF.DAY   |
|:------------|:-------------|:------------------|:------------------:|:------------------:|------------------:|-------------:|:-------------------:|:--------------------:|:-------------:|
| Minnesota    | MRO           | East North Central | normal             | severe weather     |              3060 |      5348119 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  | Afternoon     |
| Minnesota    | MRO           | East North Central | normal             | intentional attack |                 1 |      5457125 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  | Evening       |
| Minnesota    | MRO           | East North Central | cold               | severe weather     |              3000 |      5310903 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  | Evening       |
| Minnesota    | MRO           | East North Central | normal             | severe weather     |              2550 |      5380443 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  | Night         |
| Minnesota    | MRO           | East North Central | warm               | severe weather     |              1740 |      5489594 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  | Night         |

## Assessment of Missingness
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis
