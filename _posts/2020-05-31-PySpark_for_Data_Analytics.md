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

spark.createDataFrame( __ [List of data in tuple ( , )]__  ,   __Column name in tuple ( , )__) 

Example

```python
df = spark.createDataFrame([('a', 1), ('b', 2), ('c', 3)], ('k','v'))
df.show()
+---+---+
|  k|  v|
+---+---+
|  a|  1|
|  b|  2|
|  c|  3|
+---+---+
```

Anyway, for only 1 columne `( , )` still need to be used

```python
one_col = spark.createDataFrame([('a', ), ('b', ), ('c', )], ('k', ))
one_col.show()
+---+
|  k|
+---+
|  a|
|  b|
|  c|
+---+
```

#### 2. Many ways to SELECT SparkDataFrame.

SparkDataFrame have many ways for `SELECT` statesment.

- Sting of column name or list of column name.  
```python
df.select(['k', 'v']).show()
df.select('k', 'v').show()
# both will yield same result
+---+---+
|  k|  v|
+---+---+
|  a|  1|
|  b|  2|
|  c|  3|
+---+---+
```

This method, could programatically select statement with Python list unpack (\*) operator

Example

```python
col = ['v', 'k']
df.select(*col, *col[::-1], *col*2).show()
# *col  |*col[::-1]|   *col*2   |
+---+---+---+---+---+---+---+---+
|  v|  k|  k|  v|  v|  k|  v|  k|
+---+---+---+---+---+---+---+---+
|  1|  a|  a|  1|  1|  a|  1|  a|
|  2|  b|  b|  2|  2|  b|  2|  b|
|  3|  c|  c|  3|  3|  c|  3|  c|
+---+---+---+---+---+---+---+---+
```
- Use with SparkSQL function `col`. This method could add `alias` for rename select statement result (like `AS` in SQL).  

```python
df.select(F.col('v'), F.col('v').alias('2nd_v')).show()
+---+-----+
|  v|2nd_v|
+---+-----+
|  1|    1|
|  2|    2|
|  3|    3|
+---+-----+
```

- Pandas like with dot(.) notation.  
Anyway, SparkDataFrame not support auto-complete column name as in Pandas. But this method also could use `.alias` to rename select statement result.

```python
df.select(df.v, df.k, df.v.alias('2nd_v'), df.k.alias('2nd_k')).show()
+---+---+-----+-----+
|  v|  k|2nd_v|2nd_k|
+---+---+-----+-----+
|  1|  a|    1|    a|
|  2|  b|    2|    b|
|  3|  c|    3|    c|
+---+---+-----+-----+
```


#### 3. Spark Case-When is short curcuit.

Like SQL CASE-WHEN-ELSE SparkSQL have function `.when().when().otherwise()` both of them have short curcit behaviour.  

Example

```python
import pyspark.sql.functions as F
df.withColumn('short_circuit', 
    F.when(F.col('v') <= 2, F.lit('lessthan_2')).when(F.col('v') <= 1, F.lit('lessthan_1')).otherwise('morethan_3')
             ).show()
             
+---+---+-------------+
|  k|  v|short_circuit|
+---+---+-------------+
|  a|  1|   lessthan_2|  <= 'Short circuit'
|  b|  2|   lessthan_2|
|  c|  3|   morethan_3|
+---+---+-------------+
```

In first row, column 'short_circuit' got values from 1st condition (<=2), without re-ealuate the 2nd contions (<=1)  
