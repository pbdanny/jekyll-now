---
layout: post
title: Sales Single View
---

We usually heard about the Customer Single View. But this time we will do Sales Single View by using R programming and also find some behavior of sales.

## Data example

![alt text](https://github.com/pbdanny/pbdanny.github.io/blob/master/images/Sales%20Single%20View%20Data%20Example.png)

  The data recored by each time sales submit. The primary key is _Agent Code_ and the _system_date_ are the time of submitted. Before summarising data we need to defind the interval of analysis. The interval may be day, week, month or year. This time we use monthly basis.
  
  Then from _system_date_ data format in year-month-day we create new data _yyyymm_ to represent year and month part only.
  
  `yyyymm = format(system_date, "%Y%m")`
  
  The model we use to create Sales single view we use **R**(ecentcy) **F**(requency) **M**(onetary). Since we use monthly interval then we need to summarize data inform of _Agent Code_ and _yyyymm_

```
agent_mth_tt <- da %>%
  filter(!(is.na(Agent_Code) | Agent_Code == "")) %>%
  mutate(yyyymm = format(system_date, "%Y%m")) %>%
  group_by(Agent_Code, yyyymm) %>%
  summarise(tt_rcvd = n()) %>%
  mutate(yyyymm_num = yyyymm_to_num(yyyymm))
```
  As the last line we create function to translate _yyyymm_ data into numerial data for sorting and calculation based on monhtly basis. the function code are.
  
```
yyyymm_to_num <- function(ym) {
  y <- as.integer(substr(ym, 1, 4))
  m <- as.integer(substr(ym, 5, 6))
  num <- (12*y)+m
  return(num)
}
```
