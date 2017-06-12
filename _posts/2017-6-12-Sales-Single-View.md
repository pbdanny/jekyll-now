---
layout: post
title: Sales Single View
---

We usually heard about the Customer Single View. But this time we will do Sales Single View by using R programming and use it to find behavior of sales.

## Data example

![alt text](../images/Sales%20Single%20View%20Data%20Example.png)

  The data recorded by each time sales submit. The primary key is _Agent Code_ and the _system_date_ are the time of submitted. Before summarising data we need to defind the interval of analysis. The interval may be day, week, month or year. This time we use monthly basis.
  
  Then from _system_date_ data format in year-month-day we create new data _yyyymm_ to represent year and month part only.
  
  `yyyymm = format(system_date, "%Y%m")`
  
  The model we use to create Sales single view we use **R**(ecentcy) **F**(requency) **M**(onetary). Since we use monthly interval then we need to summarize data in form of _Agent Code_ and _yyyymm_.

```
agent_mth_tt <- da %>%
  filter(!(is.na(Agent_Code) | Agent_Code == "")) %>%
  mutate(yyyymm = format(system_date, "%Y%m")) %>%
  group_by(Agent_Code, yyyymm) %>%
  summarise(tt_rcvd = n()) %>%
  mutate(yyyymm_num = yyyymm_to_num(yyyymm))
```
  As the last line we create function to translate _yyyymm_ data into numerial data for sorting and calculation based on monthly basis. the function code are
  
```
yyyymm_to_num <- function(ym) {
  y <- as.integer(substr(ym, 1, 4))
  m <- as.integer(substr(ym, 5, 6))
  num <- (12*y)+m
  return(num)
}
```

## Find the Recency

  From dataframe _agent_mth_tt_ which was already summarize by monthly basis, then we do a little transformation to find recency from dataframe with a little help from **dplyr**
  
```
agent_last_mth <- agent_mth_tt %>%
  select(Agent_Code, yyyymm, yyyymm_num) %>%
  group_by(Agent_Code) %>%
  top_n(n = 1) %>%  # use top_n to find last (= max yyyymm_num)
  mutate(recent_mth = yyyymm)
```

## Find the Frequency
  
  Next, we calculate frequency, which use dataframe _agent_mth_tt_

```
active_mth <- agent_mth_tt %>%
  group_by(Agent_Code) %>%
  summarise(tt_activ_mth = sum(tt_rcvd > 0)) # Count only month with rcvd > 0, could translate to sum (no of obs. with rcvd > 0)
```

  However, to measure the direction of performance, we track time of submitting rcvd by different period; last 1 month, last 3 months, last 6 months and last 12 months. Then the final code look like this
  
```
# Use current system data and crate check point for each period

last_1mth_num <- yyyymm_to_num(format(Sys.Date(), "%Y%m")) - 1
last_3mth_num <- yyyymm_to_num(format(Sys.Date(), "%Y%m")) - 3
last_6mth_num <- yyyymm_to_num(format(Sys.Date(), "%Y%m")) - 6
last_12mth_num <- yyyymm_to_num(format(Sys.Date(), "%Y%m")) - 12

active_mth <- agent_mth_tt %>%
  group_by(Agent_Code) %>%
  summarise(tt_activ_mth = sum(tt_rcvd > 0), 
            last1mth_activ_mth = sum((yyyymm_num >= last_1mth_num) & tt_rcvd > 0),
            last3mth_activ_mth = sum((yyyymm_num >= last_3mth_num) & tt_rcvd > 0),
            last6mth_activ_mth = sum((yyyymm_num >= last_6mth_num) & tt_rcvd > 0),
            last12mth_activ_mth = sum((yyyymm_num >= last_12mth_num) & tt_rcvd > 0)
  )
```

## Find the switching behavior

  From business point of view, the consistency of performance is one major concern. To the more continuously performance, the more good to the business. The we profiling the no. of straight (consecutive) month by create function **find_consec_mth***

```
# Function find max consecutive month
find_consec_mth <- function(df) {
  df$ym <- yyyymm_to_num(df$yyyymm)  # Convert yyyymm to numeric ym
  df <- df[order(df$ym), ]  # Order by ym
  o.li <- vector()  # Create blank vector for store consecutive mo. profile
  m <- 1  # Initialized first month <- 1
  for (i in seq_along(1:nrow(df))) {  # Loop through 
    if (i < nrow(df)) {  # Check it not the last obs. 
      if ((df$ym[i]+1) == df$ym[i+1]) {  # Check if current obs == next obs (m = m+1)
        m <- m + 1  # Increase consecutive m
      } else {  # if not consecutive m
        o.li <- c(o.li, m)  # Store m in list of consecutive mo.
        m <- 1  # Reset m to 1
      }
    } else {  # For looping through the last obs
      o.li <- c(o.li, m)  # Store consecutive m
    }
  } # Output max consecutive m
  o.df <- data.frame('max_straight_mo' = max(o.li),
                     'min_straight_mo' = min(o.li),
                     'avg_straight_mo' = mean(o.li),
                     'count_straight_time' = length(o.li)
  )
  return(o.df)
}
```
  This function return the dataframe of consecutive month profile of each sales. The profiler consist of _max, min and average_ consecutive month, while _count_straight_time_ count **time** of having consecutive month from all along the data period.

  To calculate all the consecutive profile we use this function inconjunction with **lapply** function
  
```
# Create consecutive mth profile -----
l.tt <- split(agent_mth_tt, agent_mth_tt$Agent_Code)  # split by Agent_Code
o <- lapply(l.tt, FUN = find_consec_mth)  # apply find_consec_function
agent_consec_mth <- do.call(rbind, lapply(o, data.frame, stringsAsFactors = FALSE))  # Convert list of data frame to single dataframe
agent_consec_mth$Agent_Code <- rownames(agent_consec_mth)   # Re-create Agent_Codd from rownames
rownames(agent_consec_mth) <- NULL  # Reset rownames
rm(list = c("l.tt", "o"))  # Clear unused varibles
```

  From above calculation with a little join each features. We create the final Sales Single View with features as below

![alt text](../images/Final%20Sales%20Single%20View-1.png)
![alt text](../images/Final%20Sales%20Single%20View-2.png)
![alt text](../images/Final%20Sales%20Single%20View-3.png)

  
