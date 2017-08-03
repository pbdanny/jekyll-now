---
layout: post
title: "ANOVA, what really it is?"
---

## What really is the ANOVA?

ANOVA came from **AN**alysis **O**f **VA**rience which is statistical technique to inference about the sample data by focusing analysis on the varience; to be exact focusing on **Sum Square** \\(SS\\) analysis with equation

\\( \sum_{i=1}^n (y\_{i} - \bar{y})^2 \\)

The methode of analysis could summarize in 3 steps
1. Identify original state; null hypothsis (\\(H\_{0}\\)) : if we do ANOVA to test if where is different mean within groups of data , orginal state is all group \\(SS\\) are _minimized_ at central _moment_, or **grand mean** \\(\bar{Y}\bullet\bullet\\). Whereas, if we test linear regression \\(y \sim x\\) the original state is there is no effect of \\(x\\) on \\(SS\\) of \\(y\\), or the \\(SS\\) of \\(y\\) are the \\(y\\) itself which, already _minimized_ with not effect from other factors.

2. Calculate the effect of **Treatment** on \\(SS\\): If we test a different mean in groups of data, then we are _insert_ new   
_moment_ and observed if yield _more minimized_ \\(SS\\) when data hang with each group mean \\(\bar{Y}\bullet\\). Whereas, test on linear regression \\(y \sim x\\) the regression parameter \\(\beta\_{i}\\) yield better _minimized_ \\(SS\\) of \\(Y\\). 

3. Calcualate the test statistic **F-test** : with assumption that 

Test math jax
$$ \mathsf{Data = PCs} \times \mathsf{Loadings} $$

Test subscription \\( X\_{i} \\)
