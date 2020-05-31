---
layout: page
title: "PySpark SQL for Data Analytics"
date: 2020-05-31 16:05:00 -0000
categories: PySpark Bigdata DataAnalytics Python3
---

Usually, in Big Data world, the PySpark have only roles as midle man for query and aggregate data and handover to Python (or Pandas) for ML task.
    
Anyway, PySpark have very useful SparkSQL, which could do advance analytic like :- 

- Stats aggreation
- Windows
- Pivots
- Data Cube aggregration
ect.

Sadly, the Spark SQL API was written in concise manner. This post intent to extend the SparkSQL API based on real analytic applications.

#### 1. Create SparkDataframe for testing.

| spark.createDataFrame( | __List__ of data in tuple ( , ) |,  |__Column name__ in tuple ( , )| ) |
|----|----|----|----|----|
| spark.createDataFrame( | [ (  ,  ) , (  ,  )  , ( ,  ) ] | ,  |(  ,  )| ) |

Example

```python
df = spark.createDataFrame([('a', 1), ('b', 2), ('c', 3)], ('k','v'))
```

Anyway, for only 1 columne `( , )` still need to be used

```python
one_col = spark.createDataFrame([('a', ), ('b', ), ('c', )], ('k', ))
```
