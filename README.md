# Power Outages and Population Density
** By Connor Day**

## Introduction

This dataset contains major power outage events in the continental United States, ranging from January 2000 to July 2016, which contains 1,534 rows and 56 columns which cover outage details, geographical information, climate data, electricity consumption, economic characteristics, and land-use features.

**Central question:** Are states with higher population density more vulnerable to prolonged power outages, or do they recover faster?

This question matters because the relationship between power outage durations and population densities directly affects the way resources can be distributed during emergencies. For example, if a high density urban area recovers faster than a low density urban area, more resources can be allocated to those in lower density areas so that they can get the support that they need. Inversely, this could question whether more people and resources directly lead to quicker restoration, and would need a more comprehensive review on resource allocation. The average person would be able to understand how long they might be without power, depending on the area that they live in, while utility companies can redistribute resources to areas accordingly. 

**Relevant Columns:**
| Column | Description |
|--------|-------------|
| `OUTAGE.DURATION` | Duration of outage events (in minutes) |
| `CUSTOMERS.AFFECTED` | Number of customers affected by the power outage event |
| `POPDEN_URBAN` | Population density of the urban areas (persons per square mile) |
| `POPDEN_RURAL` | Population density of the rural areas (persons per square mile) |
| `POPPCT_URBAN` | Percentage of the total population of the U.S. state represented by the urban population (in %) |
| `U.S._STATE` | Represents all the states in the continental U.S. |
| `CAUSE.CATEGORY` | Categories of all the events causing the major power outages |
| `CLIMATE.REGION` |U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.) |
