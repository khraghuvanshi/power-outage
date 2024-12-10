# Analyzing and Predicting Power Outage Duration 
by - Khushi Raghuvanshi

## Introduction
The project analyses the data of major power outages in the US from January 2000 to July 2006. As defined by the Department of Energy, the major outages refer to those that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 MW. Besides major outage data, this article also presents data on the geographical location of the outages, date and time of the outages, regional climatic information, land-use characteristics, electricity consumption patterns, and economic characteristics of the states affected by the outages. The dataset was accessed from [science direct's website](https://www.sciencedirect.com/science/article/pii/S2352340918307182). 

I will perform data cleaning and exploratory analysis on the data, and explore the question: **What factors influence the duration of power outages, and can we predict outage duration based on these factors?**

Power outages are a common occurrence, especially during extreme weather events or infrastructure failures. These outages can disrupt daily life, impact businesses, and even pose safety risks. By predicting outage durations, stakeholders can plan for contingencies, optimize resources, and enhance the resilience of the power grid.

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

### Dataset Overview

## Data Cleaning and Exploratory Data Analysis
## Assessment of Missingness
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis
