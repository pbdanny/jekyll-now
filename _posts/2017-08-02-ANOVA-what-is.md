---
layout: post
title: "ANOVA, what really it is?"
---

## What really is the ANOVA?

ANOVA came from **AN**alysis **O**f **VA**rience which is statistical technique to inference about the sample data by focusing analysis on the varience; to be exact focusing on **Sum Square** \\(SS\\) analysis with equation

\\[ \sum_{i=1}^n (y\_{i} - \bar{y})^2 \\]

The methode of analysis could summarize in 3 steps

**1. Identify original state** or null hypothsis (\\(H\_{0}\\))
  For testing difference mean of 2 or more groups of data, we start with original state that all the data have **no** subgroup with their own _mean_ or _moment_, all the data varience could be best represented with \\(SS\\) that _minimized_ at grand _moment_, or **grand mean** \\(\bar{Y}\bullet\bullet\\). The result \\(SS\\) of original state will be \\(SS\\) on grand means with equation 
  \\[ SS_{Total} = \sum_{i=1}^n (y\_{i} - \bar{y})^2 \\]
    
  For the linear regression \(y \sim x\) the original state we start with the regression parameter \\(\beta\_{i}\\) of idependent varible \\(x\\) have **no** effect (or correlation) on \\(SS\\) of dependent varible \\(y\\), and the varience of \\(y\\) is best represented by the \\(SS\\) that _minimized_ at the _mean_ or _moment_ of \\(y\\) itself. (Quite confusing, yess.) The result \\(SS\\) oft original state will be \\(SS\\) on it's \\(y\\)'s mean it self with equaltion
  \\[ SS_{Total} = \sum_{i=1}^n (y\_{i} - \bar{y})^2 \\] 

**2. Calculate the effect of _Treatment_** or alternate hypothesis (\\(H\_{a}\\))
  For testing difference mean of 2 or more group of data, then we guess that there are better _moments of each group_ that yield _more minimized_ \\(SS\\) when each data point gravitated towards its own mean \\(\bar{Y}_{k}\bullet\\). The result \\(SS\\) of _treatment_ state will be \\(SS\\) of each data point on group mean 
  
  \\[ SS_{Treatment} = \sum_{j=1}^k \sum_{i=1}^n n_j(\bar{y}\_{j}\bullet - \bar{y}\bullet\bullet)^2 \\] to clarily we use \\(SS_{Group}\\}
  
  The left-over of \\(SS\\) or **residual** will be \\(SS_{resid}\\) = \\(SS_{total} - SS_{Treatment} \\)
    
  For the linear regression \\(y \sim x\\) , we guess that the regression parameter \\(\beta\_{i}\\) of idependent varible \\(x\\) have effect on the \\(SS\\) of \\(y\\), and the varience of \\(y\\) could be represented with the \\(SS\\) of _regression point_ \\(\hat{y}\\) or \\(\beta_{0} + \beta_{1}x\\).
  
  The \\(SS\\) of _regression line_ could be represented with quation
  \\[SS_{Treatment} = \sum_{i=1}^n(\hat{y}_{i} - y_{i})^2 \\] to clarify we use \\(SS_{Regression}\\) in the same meaning
  
  The left-over of \\(SS\\) or **residual** will be \\(SS_{resid}\\) = \\(SS_{total} - SS_{Regression} \\)

**3. Calcualate the Mean square** and find the _degree of freedom_
  the **Mean square** equation is
  \\[MS = \frac{SS}{degree of freedom} \\]
  
  The _Degree of freedom (df)_ is the penalties for using point estimator *(like sample mean \\(\bar{y} \\) in calculating varience (What?) \\( note: Var(y) = \sum_{i}^n (y\_{i} - \bar{y})^2 \\). In general the degree of freedom = the number of parameters - the nubmer of parameters used in calculate varience
  For example, the degree of freedom (df) of \\(SS_{Group}\\) (different group mean) = number of group (k) - 1, which the -1 come from the calculation of SS we use 1 parameter \\(\bar{y}_{j}\bullet\\) for calcualte \\(SS\\) then -1 deduct from k to be k-1).
  
  We use df. to caculate Mean Square by
  
  \\[\frac{SS}{df} \\]

**4. Calculate F-Statistic**
  The _F-statistic_ is the proportion of \\( \frac{MS_{Treatment}{MS_{residual}} \\). The motivation behind the F-statistic is if the \\(MS_{Treatment}\\) could be better represent the varience than the Residual varience \\(MS_{residual}\\) will be very little compare to the \\(MS_{Treatment}\\) result in the more right side p-value, then reject \\(H_{0}\\)
