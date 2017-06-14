---
layout: post
title: Sales Switching Profile
---

  From last post we create _Sales Single View_, in this post we analysis the swiching profiles.
  
## Motivation : Profiling switching behavior

  In performance management, one key good indicator is the continuously working of the sales person. That show how best we could attract the representatives loyalties. Since the non-obligation representative model, sales person could work or not work for us anytime. Then profiling switching behavior could help in campaign effectiveness, especially on period of motivation.

  First we start off with **mob** (Month On Book) of sales
```
load("da.RData")
library(ggplot2)
library(dplyr)
library(gridExtra)
library(tidyr)

ggplot(data = os_agent_s_view) +
  geom_histogram(mapping = aes(x = mob), fill = 'blue', binwidth = 1)
```
  We count no. of new sales registered look back on mob; mob = 1 = first month, mob = 2 = secound month, onward. However, the histogram so fuzzy and couldn't extract any information from graph. 

![alt_text](../images/Sales_Single_View_0.png)


