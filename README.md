# Power Outages and Population Density
**By Connor Day**

<br>

## Introduction

This dataset contains major power outage events in the continental United States, ranging from January 2000 to July 2016, which contains 1,534 rows and 56 columns which cover outage details, geographical information, climate data, electricity consumption, economic characteristics, and land-use features.

**Central question:** Are states with higher population density more vulnerable to prolonged power outages, or do they recover faster?

This question matters because the relationship between power outage durations and population densities directly affects the way resources can be distributed during emergencies. For example, if a high density urban area recovers faster than a low density urban area, more resources can be allocated to those in lower density areas so that they can get the support that they need. Inversely, this could question whether more people and resources directly lead to quicker restoration, and would need a more comprehensive review on resource allocation. The average person would be able to understand how long they might be without power, depending on the area that they live in, while utility companies can redistribute resources to areas accordingly. 

**Relevant Columns:**

<table>
  <thead>
    <tr>
      <th>Column</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>OUTAGE.DURATION</code></td>
      <td>Duration of outage events (in minutes)</td>
    </tr>
    <tr>
      <td><code>CUSTOMERS.AFFECTED</code></td>
      <td>Number of customers affected by the power outage event</td>
    </tr>
    <tr>
      <td><code>POPDEN_URBAN</code></td>
      <td>Population density of urban areas (persons per square mile)</td>
    </tr>
    <tr>
      <td><code>POPDEN_RURAL</code></td>
      <td>Population density of rural areas (persons per square mile)</td>
    </tr>
    <tr>
      <td><code>POPPCT_URBAN</code></td>
      <td>Percentage of total population represented by urban population (in %)</td>
    </tr>
    <tr>
      <td><code>U.S._STATE</code></td>
      <td>Represents all the states in the continental U.S.</td>
    </tr>
    <tr>
      <td><code>CAUSE.CATEGORY</code></td>
      <td>Categories of all the events causing the major power outages</td>
    </tr>
    <tr>
      <td><code>CLIMATE.REGION</code></td>
      <td>U.S. Climate regions (nine climatically consistent regions)</td>
    </tr>
  </tbody>
</table>

<br>

## Data Cleaning and Exploratory Data Analysis

**Cleaning Steps:**
- Combined 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' into a single 'OUTAGE.START' timestamp column, doing the same for 'OUTAGE.RESTORATION'. This makes it easier to work with outage times as one single variable, rather than having separate, unnecessary date and time columns. 
- Dropped the priginal four date and time columns since they became redundant after combining them. 
- Created a 'DENSITY_GROUP' column by categorizing states into "High Density" and "Low Density" based on the median urban population density (2,380 persons per square mile). This allows a direct comparison between the two groups, which is key for our research question.

A preview of the cleaned DataFrame:
<pre>
| U.S._STATE   | OUTAGE.START        |   OUTAGE.DURATION | CAUSE.CATEGORY     |   CUSTOMERS.AFFECTED |   POPDEN_URBAN | DENSITY_GROUP   |
|:-------------|:--------------------|------------------:|:-------------------|---------------------:|---------------:|:----------------|
| Minnesota    | 2011-07-01 17:00:00 |              3060 | severe weather     |                70000 |           2279 | Low Density     |
| Minnesota    | 2014-05-11 18:38:00 |                 1 | intentional attack |                  nan |           2279 | Low Density     |
| Minnesota    | 2010-10-26 20:00:00 |              3000 | severe weather     |                70000 |           2279 | Low Density     |
| Minnesota    | 2012-06-19 04:30:00 |              2550 | severe weather     |                68200 |           2279 | Low Density     |
| Minnesota    | 2015-07-18 02:00:00 |              1740 | severe weather     |               250000 |           2279 | Low Density     |
</pre>

**Univariate Analysis:**

The distribution of outage durations is heavily right-skewed, with most of the outages lasting under 5,000 minutes, with some extreme outliers exceeding 100,000 minutes. 

<iframe src="assets/plot1.html" width="800" height="450" frameborder="0"></iframe>

**Bivariate Analysis:**

Comparing outage durations across density groups, high density states appear to have slightly shorter outage durations, but both groups show a significant spread in their respective outage durations.

<iframe src="assets/plot3_zoom.html" width="800" height="450" frameborder="0"></iframe>

**Interesting Aggregates:**

<pre>
| CAUSE.CATEGORY                |   High Density |   Low Density |
|:------------------------------|---------------:|--------------:|
| equipment failure             |          427.7 |        4672.6 |
| fuel supply emergency         |        11345   |       18734.4 |
| intentional attack            |          388   |         483.5 |
| islanding                     |          194.9 |         215.5 |
| public appeal                 |         1501.8 |        1436   |
| severe weather                |         4109.1 |        3724.1 |
| system operability disruption |          552.7 |        1045.2 |
</pre>

<br>

## Assessment of Missingness

**NMAR Analysis:**

The column 'DEMAND.LOSS.MW' is likely NMAR (Not Missing at Random) because the values in this column represent the amount of peak demand lost during an outage (in units of megawatts), and whether or not that this value gets recorded likely depends on the value itself. Very small outages may not be reported if the demand loss is not significant enough to track. Inversely, extremely large outages may be impossible to make accurate measurements of, due to possible infrastructure damage to power outage monitoring systems. Additional data, such as utility company reporting standards or automated monitoring equipment could potentially explain the missingness, thus making it MAR (Missing at Random). 

**Missingness Dependency:**

We tested whether the missingness of 'CUSTOMERS.AFFECTED' depends on other columns, using permutation tests.

**Test 1:** The missingness of 'CUSTOMERS.AFFECTED' *does depend on* 'CAUSE.CATEGORY' (p < 0.001). Intuitively, this makes sense, because certain variants of outages (like intentional griefing/attacks), are far less likely to have customer counts reported or not.

<iframe src="assets/fig_miss1.html" width="800" height="450" frameborder="0"></iframe>

**Test 2:** The missingness of 'CUSTOMERS.AFFECTED' *does not depend on* 'PC.REALGSP.CHANGE' (p ≈ 0.600). The year-over-year change in a state's GDP has no logical conenction to whether customer counts are reported. 

<iframe src="assets/fig_miss2.html" width="800" height="450" frameborder="0"></iframe>

<br>

## Hypothesis Testing

**Null Hypothesis:** Outage duration is the same for high density and low density population states. Any observed difference is due to random chance alone. 

**Alternative Hypothesis:** High density states have shorter outage durations than low density states. 

**Test Statistic:** Difference in means (Low Density - High Density)

**Significance Level:** 0.05

We performed a permutation test with 10,000 permutations and obtained a p-value of approximately 0.065. At the 0.05 significance level, we *fail to reject the null hypothesis*. While high density areas do appear to have slightyly shorter outage durations, the difference is not statistically significant enough to rule out random chance. 

<iframe src="assets/fig_hyp.html" width="800" height="450" frameborder="0"></iframe>

<br>

## Framing a Prediction Problem

**Prediction Problem:** Predict the duration of a major power outage (in minutes) based on information available at the start of the outage. 

**Type:** Regression 

**Response Variable:** 'OUTAGE.DURATION' - chosen because outage duration directly measures severity and is the most actionable metric for utility companies planning emergency response.

**Evaluation Metric:** RMSE (Root Mean Squared Error) - chosen over MAE because RMSE penalizes large errors more heavily. For outage prediction, being very wrong about a long, severe outage is more costly than small errors on short outage. 

**Time of Prediction:** At the start of an outage, we would know the location (e.g. state, region, population density), the cause category, and the time of year. We would not know 'OUTAGE.RESTORATION', 'CUSTOMERS.AFFECTED', or 'DEMAND.LOSS.MW'. 

<br>

## Baseline Model

Our baseline model is a *Decision Tree Regressor* predicting 'OUTAGE.DURATION' using two features: 
- 'POPDEN_URBAN' (quantitative) - passed through as-is
- 'CAUSE.CATEGORY' (nominal) - one-hot encoded

Both transformations are implemented in a single sklearn Pipeline. The model achieved a test RMSE of 7418.97 minutes compared to a mean-only baseline of 7838.99 minutes, an improvement of 420.02 minutes. 

This baseline model's test RMSE 7418.97 minutes showed little improvement over the mean-only baseline of 7838.99, suggesting that the population density and cause category do provide a predictive value for outage duration. However, since the error value is still large, relative to the average outage duration, two features alone are not necessarily sufficient in accurately predicting outage duration, leading us to a final model consisting of more features.

<br>

## Final Model 

We improved our baseline by adding five new features: 
- 'CLIMATE.REGION' (nominal, one-hot encoded) - different climate regions experience different types of weather events, directly affecting outage duration
- 'POPPCT_URBAN' (quantitative, standardized) - captures urbanization beyond raw density, reflecting infrastructure complexity
- 'MONTH' (quantitative, standardized) - seasonal patterns affect both outage causes and restoration speed
- 'ANOMALY.LEVEL' (quantitative, standardized) - El Nino/La Nina conditions influence severe weather frequency
- 'NERC.REGION' (nominal, one-hot encoded) - different electrical reliability regions have different infrastructure and response capabilities 

We switched to a *Random Forest Regressor* and used *GridSearchCV* with 5-fold cross validation to tune hyperparameters. We found that the best parameters were: 
- *max_depth*: Prevents overfitting using tree depth. Best value: 10
- *n_estimators*: The number of trees in the forest. Best value: 100
- *min_samples_split*: Minimum samples required to split a node. Best value: 10

**Results:**
- Baseline Test RMSE: 7418.97 minutes
- Final Model Test RMSE: 6888.27 minutes
- Improvement: 530.70 minutes

<br>

## Fairness Analysis

**Group X:** Low Density states | **Group Y:** High Density states

**Evaluation Metric:** RMSE 

**Null Hypothesis:** The model is fair. Its RMSE for high density and low density states is roughly the same, and any difference is due to random chance alone. 

**Alternative Hypothesis:** The model is unfair. Its RMSE for low density states is higher than for high density states. 

**Test Statistic:** RMSE(Low Density) - RMSE(High Density)

**Significance Level:** 0.05

**Result:** With a p-value of 0.35, we fail to reject the null hypothesis. The model's performance difference between high density and low density states is not statistically significant, which suggests that the model is fair across both groups. 

<iframe src="assets/fig_fair.html" width="800" height="450" frameborder="0"></iframe>