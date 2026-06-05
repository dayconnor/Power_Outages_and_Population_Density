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
FILL IN THE OUTPUT OF print(df.head().to_markdown())` HERE

**Univariate Analysis:**

The distribution of outage durations is heavily right-skewed, with most of the outages lasting under 5,000 minutes, with some extreme outliers exceeding 100,000 minutes. 

<iframe src> 

**Bivariate Analysis:**

Comparing outage durations across density groups, high density states appear to have slightly shorter outage durations, but both groups show a significant spread in their respective outage durations.

<iframe src>

