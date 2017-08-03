---
layout: post
title: "ANOVA, what really it is?"
---

## What really is the ANOVA?

ANOVA came from **AN**alysis **O**f **VA**rience which is statistical technique to inference about the sample data by focusing analysis on the varience; to be exact focusing on **Sum Square** \(SS\) analysis with equation

\[ \sum_{i=1}^n (y\_{i} - \bar{y})^2 \]

The methode of analysis could summarize in 3 steps

**1. Identify original state** or null hypothsis (\(H\_{0}\))
    For testing difference mean of 2 or more groups of data, we start with original state that all the data have **no** subgroup with their own _mean_ or _moment_, all the data varience could be best represented with \(SS\) that _minimized_ at grand _moment_, or **grand mean** \(\bar{Y}\bullet\bullet\).
    For the linear regression \(y \sim x\) the original state that we assume no effect (or correlation) of idependent varible \(x\) on \(SS\) of dependent varible \(y\), and the varience of \(y\) is best represented by the \(SS\) that _minimized_ at the _mean_ or _moment_ of \(y\) itself. (Quite confusing, yess.)

**2. Calculate the effect of _Treatment_** or alternate hypothesis (\(H\_{a}\))
    For testing difference mean of 2 or more group of data, then we guess that there are better _moments of each group_ that yield _more minimized_ \\(SS\\) when each data point gravitated towards its own mean \(\bar{Y}_{k}\bullet\).
    For the linear regression \(y \sim x\) the regression parameter \(\beta\_{i}\) yield better _minimized_ \\(SS\\) of \\(Y\\). 

3. Calcualate the test statistic **F-test** : with assumption that 

Test math jax
$$ \mathsf{Data = PCs} \times \mathsf{Loadings} $$

Test subscription \\( X\_{i} \\)
