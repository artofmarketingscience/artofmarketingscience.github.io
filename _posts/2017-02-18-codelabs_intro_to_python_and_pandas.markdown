---
title: "Intro To Python and Pandas?"
layout: post
date: 2017-02-18
image: /assets/images/markdown.jpg
headerImage: false
projects: true
blog: false
hidden: true
tag:
- pandas
- introduction
star: true
category: blog
author: jason fong
description: Introduction to Python and pandas
---

Python is one of the most popular and fastest growing software languages used in data analytics today and there are many reasons for that. I will cover majority of the topics in this blog in Python and occasionally R.

For those who are completely new to Python and programming, I encourage you to check out Codeacademy and do their free Python course. It’s important to get a good feel for this language and understand some fundamental concepts like syntax, libraries, data structures etc. The course is very straight forward and suitable for all level.

pandas is a Python library (prewritten code for us to use) and is the bread & butter for data analysis. I’m assuming everyone that’s reading this used Excel or some sort of spreadsheet before. Pandas essentially stores data in a tabular format (rows and columns like a spreadsheet) with predefined functionalities to manipulate & analyze data. After learning about basic Python, I encourage you to check out this tutorial from Greg Reda on how to use pandas.

In this series of blog articles, I will instead focus on this Panda’s cheatsheet and additional common functionalities, walking through some examples and relating to how you would use it on a day to day task. Lets start with creating Series an DataFrames, getting some data and removing some data.

```python
import pandas as pd
```

## Creating Series and DataFrame

Lets first define a Series. A Series is simply a one dimensional array, kind of like your Excel spreadsheet but with one column.


```python
pd.Series([3, -5, 7, 4])
```




    0    3
    1   -5
    2    7
    3    4
    dtype: int64



The 0, 1, 2, 3 here is the index of the series. They are like the row #s in excel. We can also have predefined index labels for our Series. Lets label it a, b, c, d


```python
s = pd.Series([3, -5, 7, 4], index=['a', 'b', 'c', 'd'])
s
```




    a    3
    b   -5
    c    7
    d    4
    dtype: int64



DataFrame on the other hand is like a Series but two dimensional, so like your full Excel sheet instead of one column. There are many ways to create a DataFrame with your data, its quite flexible and up to you what is the easiest based on the data set you have.


```python
#if your data is already in dictionary form
data = {'Country': ['Belgium', 'India', 'Brazil'],
        'Capital': ['Brussels', 'New Delhi', 'Brasília'],
        'Population': [11190846, 1303171035, 207847528]}
df = pd.DataFrame(data)
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Capital</th>
      <th>Country</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Brussels</td>
      <td>Belgium</td>
      <td>11190846</td>
    </tr>
    <tr>
      <th>1</th>
      <td>New Delhi</td>
      <td>India</td>
      <td>1303171035</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Brasília</td>
      <td>Brazil</td>
      <td>207847528</td>
    </tr>
  </tbody>
</table>
</div>



Or you could have some Series you already defined and want to put them together into a DataFrame. The NaN here means there is unknown value.


```python
s1 = pd.Series([3, -5, 7, 4])
s2 = pd.Series([1, 3, 6])
pd.DataFrame({
        's1': s1,
        's2': s2})
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>s1</th>
      <th>s2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-5</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Retrieving Data

Remember previously your Series by default is indexed by number (0,1,2,3) and you also labeled the index. You can get information either way, using index number or your label


```python
print s['a']
print s[0]
```

    3
    3


DataFrame is similar. Earlier we created a DataFrame with country information. We can get a column by specifying the column name


```python
df['Country']
```




    0    Belgium
    1      India
    2     Brazil
    Name: Country, dtype: object



Or multiple columns


```python
df[['Country', 'Capital']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Capital</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Belgium</td>
      <td>Brussels</td>
    </tr>
    <tr>
      <th>1</th>
      <td>India</td>
      <td>New Delhi</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Brazil</td>
      <td>Brasília</td>
    </tr>
  </tbody>
</table>
</div>



And to select a range of rows, we use the : operator. It simply means select from this index value up to this index value. E.g. 0:2 means select row index 0 and 1


```python
df[0:2]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Capital</th>
      <th>Country</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Brussels</td>
      <td>Belgium</td>
      <td>11190846</td>
    </tr>
    <tr>
      <th>1</th>
      <td>New Delhi</td>
      <td>India</td>
      <td>1303171035</td>
    </tr>
  </tbody>
</table>
</div>



## Deleting Data

Sometimes we dont need all the data in our DataFrame or Series. Pandas provide a very convenient way of removing the  data using the drop function


```python
s.drop('a')
```




    b   -5
    c    7
    d    4
    dtype: int64




```python
s.drop(['a','d'])
```




    b   -5
    c    7
    dtype: int64



DataFrame has an extra parameter that you need to specif, axis. Axis = 0 means you are looking to remove a row. Axis = 1 means you want to remove a column.


```python
df.drop('Capital', axis=1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Belgium</td>
      <td>11190846</td>
    </tr>
    <tr>
      <th>1</th>
      <td>India</td>
      <td>1303171035</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Brazil</td>
      <td>207847528</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.drop([1,2], axis=0)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Capital</th>
      <th>Country</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Brussels</td>
      <td>Belgium</td>
      <td>11190846</td>
    </tr>
  </tbody>
</table>
</div>



A very simple function in Python is the range function, which allows you to define a range of number range([start],[stop])


```python
range(0,2)
```




    [0, 1]




```python
df.drop(range(0,2))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Capital</th>
      <th>Country</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Brasília</td>
      <td>Brazil</td>
      <td>207847528</td>
    </tr>
  </tbody>
</table>
</div>


