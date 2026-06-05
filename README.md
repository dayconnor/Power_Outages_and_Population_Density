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
