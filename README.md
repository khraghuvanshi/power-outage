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

- Next, I combined the necessary columns which included combining   OUTAGE.START.DATE` and `OUTAGE.START.TIME` columns into a single datetime column `OUTAGE.START`. Similarly, `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` were combined into a single datetime column `OUTAGE.RESTORATION`.

- After this, I applied a lambda function to replaced the 0 values with NaN. All the 0 values in `OUTAGE.DURATION` and `POPULATION` were replaced with NaN.

- Then I created a new feature, `TIME.OF.DAY` from extracting the hour from `OUTAGE.START` and categorizing it into 4 features: Morning (6 AM to 12 PM), Afternoon: 12 PM to 6 PM, Evening: 6 PM to 10 PM, and Night: 10 PM to 6 AM.

These steps enhanced the dataset's usability, improved data quality, and provided additional context for exploratory analysis and machine learning.

The first few rows of the dataset are shown below:

| U.S._STATE   | NERC.REGION   | CLIMATE.REGION     | CLIMATE.CATEGORY   | CAUSE.CATEGORY     |   OUTAGE.DURATION |   POPULATION | OUTAGE.START        | OUTAGE.RESTORATION   | TIME.OF.DAY   |
|:-------------|:--------------|:-------------------|:-------------------|:-------------------|------------------:|-------------:|:--------------------|:---------------------|:--------------|
| Minnesota    | MRO           | East North Central | normal             | severe weather     |              3060 |      5348119 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  | Afternoon     |
| Minnesota    | MRO           | East North Central | normal             | intentional attack |                 1 |      5457125 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  | Evening       |
| Minnesota    | MRO           | East North Central | cold               | severe weather     |              3000 |      5310903 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  | Evening       |
| Minnesota    | MRO           | East North Central | normal             | severe weather     |              2550 |      5380443 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  | Night         |
| Minnesota    | MRO           | East North Central | warm               | severe weather     |              1740 |      5489594 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  | Night         |

### Univariate Analysis
In my exploratory data analysis, I first plot a scatterplot of the duration and its count to see any outliers and trends.We can see some outliers where the outage duration was above 60,000. There doesn't seem to be much pattern, most points are concentrated at lower outage durations. 
<iframe
  src="assets/out_duration_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Then I plotted the proportions of `CAUSE.CATEGORY` to see what causes outages the most. We can see that several weather causes the most outages, and islanding causes the least.
<iframe
  src="assets/out_cause_cat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis
Initially, I plotted multiple plots using 2 features. The most significant ones are as follows:

I want to see the distribution of `U.S_STATE` and `OUTAGE.DURATION` on average, and if some regions had longer outages compared to others. To do this, I first grouped the dataframe by `U.S._STATE` and then calculated the average of `OUTAGE.DURATION`. Then I plotted a Geomap. We can see that eastern regions have longer outage duration.
<iframe
  src="assets/outage_duration_map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

And so, I grouped the data by  `CLIMATE.REGION` and calculated the average of `OUTAGE.DURATION`. Then I plotted a barplot of the average outage duration, and we can see that East North Central region has the highest outage duration.
<iframe
  src="assets/clim_reg_bar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I also wanted to see the distribution of `TIME.OF.DAY` and `OUTAGE.DURATION`, and so I plotted a heatmap for it. We can see that **Afternoon** and **Morning** have a higher duration than **Evening** and **Night**. 
<iframe
  src="assets/time_heapmap.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Grouping
For grouping, I wanted to To identify the causes of power outages that lead to the longest durations in specific climate regions. Through this table, we can see what were the major causes for each `CLIMATE.REGION`.

| CLIMATE.REGION     |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|:-------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
| Central            |             322     |                 10035.2 |              490.25  |    125.333  |         1410    |          3299.63 |                        2695.2   |
| East North Central |           26435.3   |                 33971.2 |             2501.11  |      1      |          733    |          4434.82 |                        2610     |
| Northeast          |             269.75  |                 14629.6 |              264.68  |    881      |         2655    |          4429.9  |                         773.5   |
| Northwest          |             702     |                     1   |              488.831 |     73.3333 |          898    |          4838    |                         141     |
| South              |             295.778 |                 17482.5 |              337.667 |    493.5    |         1163.98 |          4391.35 |                         899.385 |
| Southeast          |             554.5   |                   nan   |              504.667 |    nan      |         2865.4  |          2685.71 |                         180.6   |
| Southwest          |             113.8   |                    76   |              274.678 |      2      |         2275    |         11572.9  |                         370.375 |
| West               |             524.81  |                  6154.6 |              886.267 |    214.857  |         2028.11 |          2928.37 |                         363.667 |
| West North Central |              61     |                   nan   |               47     |     68.2    |          439.5  |          2442.5  |                         nan     |

## Assessment of Missingness
Following are the columns that have missing values: `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `OUTAGE.DURATION`, `OUTAGE.START`, `OUTAGE.RESTORATION`
### NMAR Analysis
Out of the columns with missing values, the only plausible one to be NMAR is `OUTAGE.DURATION` because  it might depend directly on the true values of the column. For instance, extremely short outages might not be recorded because they are considered insignificant or overlooked, while extremely long outages might be omitted due to data censoring, reporting biases, or logistical challenges in accurately documenting prolonged disruptions. But I will do permutation tests to see if it depends on any other column before concluding this.

### MAR Analysis
Null Hypothesis: The missingness of `OUTAGE.DURATION` column is independent on the `CLIMATE.REGION` column.

Alternate Hypothesis: The missingness of `OUTAGE.DURATION` column is dependent on the `CLIMATE.REGION` column.

I will perform a permutation test on `OUTAGE.DURATION` using the test statistic of Total Variation Distance (TVD). 
After doing the test, I got a P-value of 0.0. With a significant level of 0.05, we can say that we reject the null. Therefore the missingness of `OUTAGE.DURATION` is dependent on `CLIMATE.REGION`, and its MAR.
<iframe
  src="assets/mar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing
I will be testing whether the outage duration is greater on average for outages occurring during the morning and night compared to those occurring in the afternoon and evening. The relevant columns for this test are `OUTAGE.DURATION` and `TIME.OF.DAY`. I will group the data into two categories: one containing outages during the morning and night, and the other containing outages during the afternoon and evening. Only outages with non-missing values for these columns will be used in the analysis.

Null Hypothesis: On average, the outage duration in the morning and night is the same as afternoon and evening.

Alternate Hypothesis: On average, the outage duration in the morning and night is more than afternoon and evening.

Test Statistic: The test statistic used is the difference in mean outage durations between the two groups: Mean Duration (Morning/Night) − Mean Duration (Afternoon/Evening)
This choice of test statistic is appropriate because it directly measures the difference in the central tendency (mean) of outage durations between the two groups, which aligns with the research question.

Significance Level:
A significance level of 0.05 is chosen, which is standard for hypothesis testing.
After performing our hypothesis test, our P-value is 0.0021, and so we reject the null hypothesis. There is evidence that outage duration is more in the morning and night than in the afternoon and evening.
<iframe
  src="assets/hypothesis.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem
My goal with my prediction model is to predict duration of outages (categorized into three levels: Short, Medium, and Long) based on several features such as `CLIMATE.REGION`, `CAUSE.CATEGORY`, `CLIMATE.CATEGORY`, `TIME.OF.DAY`, `U.S._STATE`, `NERC.REGION`, and `POPULATION`. These features were chosen because they are available at the time of prediction and are likely to influence the duration of power outages.

This is a classification problem, specifically a multiclass classification problem, as the target variable, `OUTAGE_DURATION_RANGE`, has three possible categories. The target variable is  `OUTAGE_DURATION_RANGE`. 

The F1-score is chosen as the primary evaluation metric because the target classes (Short, Medium, Long) may not be balanced. The F1-score accounts for both precision and recall, ensuring that the model performs well across all classes rather than favoring the most frequent ones. 

## Baseline Model
The baseline model is a Random Forest Classifier designed to predict the OUTAGE_DURATION_RANGE (Short, Medium, Long) using key features from the dataset. Random Forest was chosen for its robustness and ability to handle categorical and numerical data. 

Model Features:

- Nominal Features: `CAUSE.CATEGORY`, `U.S._STATE`, `CLIMATE.REGION`, `TIME.OF.DAY`
- Ordinal Features: `CLIMATE.CATEGORY`
  
To prepare the data for the model, One-Hot Encoding was applied to all nominal features using a ColumnTransformer to convert these into binary indicator variables.

The dataset was split into a training set (80%) and a test set (20%) using train_test_split with a random seed of 42 to ensure reproducibility. The baseline model was evaluated using the test set, and the performance was summarized using the classification report, which includes precision, recall, F1-score, and support for each class.

The baseline model performed poorer than I expected, with an accuracy of 49%, and the following Classification Report:

| Class    | Precision | Recall | F1-Score | Support |
|----------|-----------|--------|----------|---------|
| Long     | 0.37      | 0.51   | 0.43     | 76      |
| Medium   | 0.54      | 0.54   | 0.54     | 106     |
| Short    | 0.59      | 0.42   | 0.49     | 98      |
| **Accuracy**       |           |        |          | **0.49** |
| **Macro Avg**      | 0.50      | 0.49   | 0.49     | 280     |
| **Weighted Avg**   | 0.51      | 0.49   | 0.49     | 280     |

## Final Model
For my final model, I added `SEASON` as a feature by extracting the month from `OUTAGE.START`.  this feature captures the season in which the outage occurred (Winter, Spring, Summer, Fall). Seasonal variations often correlate with specific outage causes, such as severe weather in Winter or Summer storms. I used an additional feature `POPULATION` for this model as well.

I also removed the outliers from `OUTAGE.DURATION` to not have baises in the prediction. 

The final model used a Random Forest Classifier, an ensemble-based algorithm that combines multiple decision trees to improve predictive accuracy and reduce overfitting. Random Forest was chosen because:

- It handles mixed data types (categorical and numerical) without extensive preprocessing.
- It is robust to outliers and can model complex interactions between features.

Hyperparameter tuning was performed using GridSearchCV with 5-fold cross-validation to optimize the following parameters:

- n_estimators: Number of trees in the forest. Tested values: [100, 200].
- max_depth: Maximum depth of the trees. Tested values: [None, 10, 20].
- min_samples_split: Minimum number of samples required to split an internal node. Default value was used.
The best hyperparameters identified:

- n_estimators: 200
- max_depth: 10

The final model outperformed the baseline model with a new accuracy of 67%.
In terms of overall metrics, here's the Classification Report:

| Class    | Precision | Recall | F1-Score | Support |
|----------|-----------|--------|----------|---------|
| Long     | 0.66      | 0.74   | 0.70     | 84      |
| Medium   | 0.63      | 0.47   | 0.54     | 96      |
| Short    | 0.69      | 0.80   | 0.74     | 99      |
| **Accuracy**       |           |        |          | **0.67** |
| **Macro Avg**      | 0.66      | 0.67   | 0.66     | 279     |
| **Weighted Avg**   | 0.66      | 0.67   | 0.66     | 279     |

## Fairness Analysis
