# Power Outages and Population Density
**By Connor Day**

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

## Data Cleaning and Exploratory Data Analysis

**Cleaning Steps:**
- Combined 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' into a single 'OUTAGE.START' timestamp column, doing the same for 'OUTAGE.RESTORATION'. This makes it easier to work with outage times as one single variable, rather than having separate, unnecessary date and time columns. 
- Dropped the priginal four date and time columns since they became redundant after combining them. 
- Created a 'DENSITY_GROUP' column by categorizing states into "High Density" and "Low Density" based on the median urban population density (2,380 persons per square mile). This allows a direct comparison between the two groups, which is key for our research question.

A preview of the cleaned DataFrame:
<pre>
|   OBS |   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   | DENSITY_GROUP   | CUST_MISSING   |
|------:|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|:----------------|:---------------|
|     1 |   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  | Low Density     | False          |
|     2 |   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  | Low Density     | True           |
|     3 |   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  | Low Density     | False          |
|     4 |   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  | Low Density     | False          |
|     5 |   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  | Low Density     | False          |
</pre>

**Univariate Analysis:**

The distribution of outage durations is heavily right-skewed, with most of the outages lasting under 5,000 minutes, with some extreme outliers exceeding 100,000 minutes. 

<iframe src="assets/plot1.html" width="800" height="600" frameborder="0"></iframe>

**Bivariate Analysis:**

Comparing outage durations across density groups, high density states appear to have slightly shorter outage durations, but both groups show a significant spread in their respective outage durations.

<iframe src"assets/plot3_zoom.html" width="800" height="600" frameborder="0"></iframe>

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

## Assessment of Missingness

**NMAR Analysis:**

The column 'DEMAND.LOSS.MW' is likely NMAR (Not Missing at Random) because the values in this column represent the amount of peak demand lost during an outage (in units of megawatts), and whether or not that this value gets recorded likely depends on the value itself. Very small outages may not be reported if the demand loss is not significant enough to track. Inversely, extremely large outages may be impossible to make accurate measurements of, due to possible infrastructure damage to power outage monitoring systems. Additional data, such as utility company reporting standards or automated monitoring equipment could potentially explain the missingness, thus making it MAR (Missing at Random). 

**Missingness Dependency:**

We tested whether the missingness of 'CUSTOMERS.AFFECTED' depends on other columns, using permutation tests.

*Test 1:* The missingness of 'CUSTOMERS.AFFECTED' **does depend on** 'CAUSE.CATEGORY' (p < 0.001). Intuitively, this makes sense, because certain variants of outages (like intentional griefing/attacks), are far less likely to have customer counts reported. 

<iframe src="assets/fig_miss1.html" width="800" height="600" frameborder="0"></iframe>

*Test 2:* The missingness of 'CUSTOMERS.AFFECTED' **does not depend on** 'PC.REALGSP.CHANGE' (p ≈ 0.600). The year-over-year change in a state's GDP has no logical conenction to whether customer counts are reported. 

<iframe src="assets/fig_miss2.html" width="800" height="600" frameborder="0"></iframe>