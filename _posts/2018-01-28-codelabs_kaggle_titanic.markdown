---
title: "Remember Jack and Rose?"
layout: post
date: 2018-01-27
image: /assets/images/markdown.jpg
headerImage: false
projects: true
blog: false
hidden: true
tag:
- kaggle
star: true
category: blog
author: jason fong
description: Kaggle Competition - Titanic Dataset
---

![jpeg](/assets/images/2018-01-27-project-kaggle_titanic/jack_rose.jpg)

Remember Jack & Rose? Leo & Kate Winslet? Titanic movie? Nostalgia? I wish this project was half as romantic as the movie but I'd be lying to you if I said it was.



## TL;DR

This exercise is the infamous Titanic dataset that all Kaggle newbs start out with. It is a classification problem where we need to predict if passengers will survive. Accuracy is used to evaluate predictions. At the time of submission, I was ranked 391/9642, top 4% on the leaderboard with an accuracy of 81.8%. By no means is this a perfect submission, nor did I write beautiful Python code. At the end, I have a section on considerations and improvements.

![png](/assets/images/2018-01-27-project-kaggle_titanic/output_2_0.png)

I won't include all the analysis and code in this post, just the key areas of my analysis and prediction. If you want the detailed step by step, checkout my [GitHub here](https://github.com/fongmanfong/kaggle_competitions/blob/master/titanic/Titanic.ipynb).

## Table of Contents
- [Introduction](#introduction)
- [Feature Exploration / Cleaning / Engineering](#feature)
	- [Sex](#sex)
	- [Embarked](#embarked)
	- [Name](#name)
	- [SibSp & Parch](#sibspparch)
	- [Cabin](#cabin)
	- [Ticket](#ticket)
	- [Age](#age)
	- [Fare](#fare)
- [Modelling & Making Predictions](#model)
- [Results](#results)
- [Considerations / Improvements](#considerations)

## <a name="introduction">Introduction</a>
As mentioned, the goal here is the predict whether passengers will survive this accident. For more information about this kaggle competition, you can checkout <https://www.kaggle.com/c/titanic>. A list of passenger information is broke into two datasets, training and test. The training set has information on whether a passenger survived while in the test set this information is hidden. We will use the training set to train our prediction model and apply onto the test set and kaggle will provide an accuracy score. 


Information provided in the data sets:
- PassengerID: ID of the passenger
- Survived: If a passenger survived
- Pclass: Ticket class
- Name: Passenger's name
- Sex: Gender
- Age: Age of passenger
- SibSp: Number of siblings and/or spouses
- Parch: Number of parents and/or children
- Ticket: Ticket number
- Fare: Dollar amount of ticket
- Cabin: Cabin unit and number 
- Embarked: Boarding port 


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>


    Total records in train & test: 1309
    Train: Total Survived 342
    Train: Total People 891 


## <a name="feature"> Feature Exploration / Cleaning / Engineering </a>

Here we'll go through one feature at a time, exploring it, cleaning it if necessary and do feature engineering if it make sense.

### <a name="sex">Sex</a>

First off, we see from above Sex is a categorical variable so I'll define a simple function map it to numerical values. It would be interesting to just see if there is a correlation between gender and survival. We see females has a significantly higher chance of survival.


```python
def map_sex(df, col):
    return df[col].map(lambda x: 1 if x == 'female' else 0)
```


```python
train.groupby('Sex')['Survived'].aggregate((np.sum, len, np.mean))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sum</th>
      <th>len</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>Sex</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>109</td>
      <td>577</td>
      <td>0.188908</td>
    </tr>
    <tr>
      <th>1</th>
      <td>233</td>
      <td>314</td>
      <td>0.742038</td>
    </tr>
  </tbody>
</table>
</div>



### <a name="embarked">Embarked</a>

We noticed that Embarked has one missing value in the training set. Since its a categorical, it would intuitively make sense that we can use the probability of the missing value being S, C, Q. We noticed that there is the highest chance of the missing value being S. Segmenting this by Survived, we see people Embarked = C is slightly correlated with survival rate vs S & Q


```python
sns.countplot(x="Embarked", data=train, palette="Greens_d");
```


![png](/assets/images/2018-01-27-project-kaggle_titanic/output_21_0.png)


```python
sns.barplot(x="Embarked", y="Survived", hue="Embarked", data=train, ci=None);
```


![png](/assets/images/2018-01-27-project-kaggle_titanic/output_23_0.png)


### <a name="name">Name</a>

Name is a little more interesting. At face value, there are many different names but we see they all have a title. We know Mr. is different than Mrs. and we also see some interesting titles like Master. and Lady. Doing some research on Google we can see how these titles are used. We also notice that there are a lot of titles with similar meaning as well as titles that identify personnel with a different social class. We can group these titles together into intuitive categories to see if they have correlation with survival rate.


```python
def extract_title(df, col):
    return df[col].str.extractall('([A-Z]+[a-z]+\.)').unstack()[0]
```


```python
sns.countplot(y="Title", data=train, palette="Greens_d");
```


![png](/assets/images/2018-01-27-project-kaggle_titanic/output_27_0.png)


    Mr.        240
    Miss.       78
    Mrs.        72
    Master.     21
    Col.         2
    Rev.         2
    Dona.        1
    Dr.          1
    Ms.          1
    Name: Title, dtype: int64



One thing to notice here is that Dona. is in test data but not the training set. We should include that in our categorization. Based on intuitive grouping, I've essentially classified all titles that are used for people with certain social class as Noble and anything else as Other.

Similar to gender, we see female titles Mrs. and Miss have strong correlation with survival. People with noble title tend to have a higher chance of surviving, which intuitively make sense. The so called "important" people tend to get favoured treatments.


```python
title_map = {
    'Mme.': 'Mr.',
    'Mlle.': 'Miss.',
    'Ms.': 'Miss.',
    'Sir.' : 'Noble',
    'Rev.' : 'Other',
    'Major.': 'Other',
    'Lady.': 'Noble',
    'Jonkheer.': 'Noble',
    'Dr.': 'Other',
    'Countess.' : 'Noble',
    'Col.': 'Other',
    'Capt.': 'Other',
    'Don.': 'Noble', 
    'Dona.': 'Noble',
    'Master.': 'Noble',
    'Mr.': 'Mr.',
    'Mrs.': 'Mrs.',
    'Miss.': 'Miss.'
}
```


```python
def map_title(df, col):
    return df[col].map(lambda x: title_map[x] if x in title_map.keys() else 'Other')
```

```python
sns.barplot(x="Title", y="Survived", hue="Title", data=train, ci=None);
```


![png](/assets/images/2018-01-27-project-kaggle_titanic/output_33_0.png)


### <a name="sibspparch">SibSp & Parch</a>

These two variables give us information about the passengers family, (spouse, siblings, parents, children). The tricky part is we don't really know if someone has a sibling or a spouse, similarly a parent or a child. We can probably take some time to infer, which may give us better insight into whether that person is a mother with certain number of child. This could be a good signal since we know mothers and children tend to be saved first in situations like Titanic. Given that blurb, in this exercise, I've kept it basic. 


```python
sns.countplot(y="SibSp", data=train, palette="Greens_d");
```


![png](/assets/images/2018-01-27-project-kaggle_titanic/output_35_0.png)



```python
train.groupby('SibSp')['Survived'].aggregate((np.sum, len, np.mean))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sum</th>
      <th>len</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>SibSp</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>210</td>
      <td>608</td>
      <td>0.345395</td>
    </tr>
    <tr>
      <th>1</th>
      <td>112</td>
      <td>209</td>
      <td>0.535885</td>
    </tr>
    <tr>
      <th>2</th>
      <td>13</td>
      <td>28</td>
      <td>0.464286</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>16</td>
      <td>0.250000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>18</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>5</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0</td>
      <td>7</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.countplot(y="Parch", data=train, palette="Greens_d");
```


![png](/assets/images/2018-01-27-project-kaggle_titanic/output_37_0.png)



```python
train.groupby('Parch')['Survived'].aggregate((np.sum, len, np.mean))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sum</th>
      <th>len</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>Parch</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>233</td>
      <td>678</td>
      <td>0.343658</td>
    </tr>
    <tr>
      <th>1</th>
      <td>65</td>
      <td>118</td>
      <td>0.550847</td>
    </tr>
    <tr>
      <th>2</th>
      <td>40</td>
      <td>80</td>
      <td>0.500000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>5</td>
      <td>0.600000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>4</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1</td>
      <td>5</td>
      <td>0.200000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0</td>
      <td>1</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>



We see pasengers with one sibling or spouse have a slight correlation with survival. Similarly for passengers with 1 parent or child. One thing we can also look at is what both of these variables together tell us. Together will identify the family size of the passenger. We see more interesting results if we look at family. People with family size of 3 is highly correlated with surviving, while people with family size of 1 and 2 have a slight correlation with survival.


```python
train['Family Size'] = train['SibSp'] + train['Parch']
test['Family Size'] = train['SibSp'] + train['Parch']
```


```python
train.groupby('Family Size')['Survived'].aggregate((np.sum, len, np.mean))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sum</th>
      <th>len</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>Family Size</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>163</td>
      <td>537</td>
      <td>0.303538</td>
    </tr>
    <tr>
      <th>1</th>
      <td>89</td>
      <td>161</td>
      <td>0.552795</td>
    </tr>
    <tr>
      <th>2</th>
      <td>59</td>
      <td>102</td>
      <td>0.578431</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>29</td>
      <td>0.724138</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>15</td>
      <td>0.200000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3</td>
      <td>22</td>
      <td>0.136364</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4</td>
      <td>12</td>
      <td>0.333333</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0</td>
      <td>6</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0</td>
      <td>7</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>



### <a name="cabin">Cabin</a>

As seen above, we noticed that there are a lot of missing data points for cabin. Although I just replace this information with unknown, I can probably do a better job of inferring what cabin person could be in. For example inferring which passengers belong to the same family and use knowledge that family prefer to stay close to each other. Maybe we can use name and see which members have the same last name, look at SibSP, Parch and Family size, are they in the same Pclass.

For cabin, I'm only interested in the first letter of the cabin. Reason being, the letter usually denotes the section of the cabin which implies different areas of the boat. Intuitively, even in the Titanic movie, people in different areas of the boat had different survival rate.

```python
def parse_cabin (df, col):
    return df[col].map(lambda x: re.sub(r'[0-9]+', '', x)[0])

train['Cabin - Parsed'] = parse_cabin(train, 'Cabin')
test['Cabin - Parsed'] = parse_cabin(test, 'Cabin')
```


```python
train.groupby('Cabin - Parsed')['Survived'].aggregate((np.sum, len, np.mean))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sum</th>
      <th>len</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>Cabin - Parsed</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>7</td>
      <td>15</td>
      <td>0.466667</td>
    </tr>
    <tr>
      <th>B</th>
      <td>35</td>
      <td>47</td>
      <td>0.744681</td>
    </tr>
    <tr>
      <th>C</th>
      <td>35</td>
      <td>59</td>
      <td>0.593220</td>
    </tr>
    <tr>
      <th>D</th>
      <td>25</td>
      <td>33</td>
      <td>0.757576</td>
    </tr>
    <tr>
      <th>E</th>
      <td>24</td>
      <td>32</td>
      <td>0.750000</td>
    </tr>
    <tr>
      <th>F</th>
      <td>8</td>
      <td>13</td>
      <td>0.615385</td>
    </tr>
    <tr>
      <th>G</th>
      <td>2</td>
      <td>4</td>
      <td>0.500000</td>
    </tr>
    <tr>
      <th>T</th>
      <td>0</td>
      <td>1</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>U</th>
      <td>206</td>
      <td>687</td>
      <td>0.299854</td>
    </tr>
  </tbody>
</table>
</div>




```python
test['Cabin - Parsed'].value_counts()
```




    U    327
    C     35
    B     18
    D     13
    E      9
    F      8
    A      7
    G      1
    Name: Cabin - Parsed, dtype: int64



One thing to notice here is that the test set does not have cabin T. As a result, any cabin room letters that are not in the test set I will set as U (for Unknown). The only case here is T


```python
train['Cabin - Parsed'] = train['Cabin - Parsed'].map(lambda x: x if x in test['Cabin - Parsed'].value_counts().index.tolist() else 'U')
```

### <a name="ticket">Ticket</a>

The approach that I've taken here is to look at the ticket label. Similar intuition as cabin, ticket label usually identify a certain type of ticket for certain types of passenger. Looking at the ticket values below, we see that there are a lot of clean up work to be done, like cleaning up empty spaces. I've made a simple assumption here that the periods are just noise. One hypothesis is that tickets were hand written back then so there could of been more variation to how tickets were labelled, like some people wrote A and some wrote A (period). Tickets with no identifier I've given it a no identifier label. Ticket labels with a / I've considered it to have two labels and introduced a label count variable.


```python
train['Ticket'].head(30).unique()
```




    array(['A/5 21171', 'PC 17599', 'STON/O2. 3101282', '113803', '373450',
           '330877', '17463', '349909', '347742', '237736', 'PP 9549',
           '113783', 'A/5. 2151', '347082', '350406', '248706', '382652',
           '244373', '345763', '2649', '239865', '248698', '330923', '113788',
           '347077', '2631', '19950', '330959', '349216'], dtype=object)




```python
def ticket_split(x):
    x_split = x.split(' ')
    if len(x_split) > 1:
        return re.sub('\.', '', x_split[0]).lower()
    else:
        return 'No Identifier'
```


```python
def ticket_first_label(x):
    return x.split('/')[0]
```


```python
def ticket_label_count(x):
    if x == 'No Identifier':
        return 0
    else:
        return len(x.split('/'))
```


```python
train['Ticket - Parsed'] = train['Ticket'].map(lambda x: ticket_split(x))
train['Ticket - First Label'] = train['Ticket - Parsed'].map(lambda x: ticket_first_label(x))
train['Ticket Label Count'] = train['Ticket - Parsed'].map(lambda x: ticket_label_count(x))
```


```python
train['Ticket - First Label'].value_counts()
```




    No Identifier    665
    pc                60
    ca                42
    a                 26
    ston              18
    soton             17
    sc                16
    w                 10
    c                  5
    fcc                5
    soc                5
    so                 4
    pp                 3
    a5                 2
    we                 2
    sw                 2
    p                  2
    a4                 1
    fa                 1
    sp                 1
    fc                 1
    sco                1
    sop                1
    wep                1
    Name: Ticket - First Label, dtype: int64



We see a lot of ticket labels that appears very infrequently in our dataset and can be treated like outliers/noise. I've grouped all the ticket labels that appear less than ~3% of training set into the Other category to produce a ticket label that has a more representative sample size. A lot of levels in a categorical variable can throw off prediction accuracy. We see some interesting correlation between ticket label PC and surviving.


```python
train['Ticket - First Label'] = train['Ticket - First Label'].map(lambda x: x if x in ['No Identifier', 'pc', 'ca', 'a'] else 'Other' )
```

```python
train.groupby(('Ticket - First Label'))['Survived'].aggregate((np.sum, len, np.mean))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sum</th>
      <th>len</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>Ticket - First Label</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>No Identifier</th>
      <td>255</td>
      <td>665</td>
      <td>0.383459</td>
    </tr>
    <tr>
      <th>Other</th>
      <td>32</td>
      <td>98</td>
      <td>0.326531</td>
    </tr>
    <tr>
      <th>a</th>
      <td>2</td>
      <td>26</td>
      <td>0.076923</td>
    </tr>
    <tr>
      <th>ca</th>
      <td>14</td>
      <td>42</td>
      <td>0.333333</td>
    </tr>
    <tr>
      <th>pc</th>
      <td>39</td>
      <td>60</td>
      <td>0.650000</td>
    </tr>
  </tbody>
</table>
</div>



### Age

Age is probably the most interesting attribute here. We have around 150 missing records and we need to do something with them. One simple way of handling this is to simple populate it with a sample statistic like mean or median. This would lose a lot of information (~1/8 of the training set would be labeled with median / average age). Instead we can consider segmenting our data set to see if we can find more meaning ways of inferring age. 

First, lets take a look if age has anything to do with survival rate. We saw that gender had a huge impact on survival, lets segment by Sex to see if female vs. male age distribution.


```python
h = sns.FacetGrid(train, row='Sex', col="Survived", margin_titles=True)
h.map(plt.hist, "Age", color="steelblue", bins=85, lw=0)
```




    <seaborn.axisgrid.FacetGrid at 0x1156b4410>




![png](/assets/images/2018-01-27-project-kaggle_titanic/output_61_1.png)


Results are fairly intuitive, young to middle age male has the lowest chance of survival, while young to middle age women has the highest chance. This is one variable for our segmentation. Other variables I am considering are Title and Family size. Title make sense because a title like Miss vs Mrs sometimes can give you some indication of the age range of an individual. Family size also make sense because certain age groups tend to travel certain way. E.g people travelling individually tend to fall into a younger a group while people travelling with a family tend to fall into higher age group. 

Because family size is a continuous variable, It would be easier to segment our data by labelling and converting it into a categorical variable. I've defined three labels:

- Individuals (Family size = 0)
- Small Family (Family size between 1 and 3)
- Big Family (Family size above 3)

Like we expected, we see very different results based on our segmentation, which we would of missed if we simply replaced the missing values with a blanket sample statistic. A teenage girl travelling with family we would call her Miss and simultaneously a young professional female that's not married travelling individually we would also call her Miss. 


```python
def define_indiv (df, col):
    return df[col].map(lambda x: 1 if x == 0 else 0)
```


```python
def small_family (df, col):
    return df[col].map(lambda x: 1 if x > 0 and x <=3 else 0)
```


```python
def big_family (df, col):
    return df[col].map(lambda x: 1 if x >=4 else 0)
```


```python
segment_age = train.groupby(['Sex', 'Title', 'Individual', 'Big Family', 'Small Family'])['Age'].aggregate((np.mean, np.median, len))
segment_age
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th>mean</th>
      <th>median</th>
      <th>len</th>
    </tr>
    <tr>
      <th>Sex</th>
      <th>Title</th>
      <th>Individual</th>
      <th>Big Family</th>
      <th>Small Family</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="8" valign="top">0</th>
      <th rowspan="3" valign="top">Mr.</th>
      <th rowspan="2" valign="top">0</th>
      <th>0</th>
      <th>1</th>
      <td>32.681818</td>
      <td>31.0</td>
      <td>109.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <td>27.750000</td>
      <td>17.5</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <th>0</th>
      <td>32.388316</td>
      <td>29.0</td>
      <td>397.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Noble</th>
      <th rowspan="2" valign="top">0</th>
      <th>0</th>
      <th>1</th>
      <td>6.174762</td>
      <td>3.0</td>
      <td>23.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <td>5.250000</td>
      <td>4.0</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <th>0</th>
      <td>39.000000</td>
      <td>39.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Other</th>
      <th>0</th>
      <th>0</th>
      <th>1</th>
      <td>49.200000</td>
      <td>50.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <th>0</th>
      <td>45.363636</td>
      <td>51.0</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th rowspan="10" valign="top">1</th>
      <th rowspan="3" valign="top">Miss.</th>
      <th rowspan="2" valign="top">0</th>
      <th>0</th>
      <th>1</th>
      <td>15.901961</td>
      <td>15.0</td>
      <td>59.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <td>12.000000</td>
      <td>9.0</td>
      <td>23.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <th>0</th>
      <td>27.654321</td>
      <td>26.0</td>
      <td>103.0</td>
    </tr>
    <tr>
      <th>Mr.</th>
      <th>1</th>
      <th>0</th>
      <th>0</th>
      <td>24.000000</td>
      <td>24.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Mrs.</th>
      <th rowspan="2" valign="top">0</th>
      <th>0</th>
      <th>1</th>
      <td>34.243902</td>
      <td>33.0</td>
      <td>95.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <td>40.000000</td>
      <td>40.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <th>0</th>
      <td>41.812500</td>
      <td>41.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Noble</th>
      <th>0</th>
      <th>0</th>
      <th>1</th>
      <td>48.000000</td>
      <td>48.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <th>0</th>
      <td>33.000000</td>
      <td>33.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>Other</th>
      <th>1</th>
      <th>0</th>
      <th>0</th>
      <td>49.000000</td>
      <td>49.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



Based on the segmentation, I take the median age in each group and build a lookup dictionary. Then I simply go through missing data in the training data and test data and fill in with the inferred age.


```python
age_median_map = segment_age['median'].to_dict()
age_median_map
```




    {(0, 'Mr.', 0, 0, 1): 31.0,
     (0, 'Mr.', 0, 1, 0): 17.5,
     (0, 'Mr.', 1, 0, 0): 29.0,
     (0, 'Noble', 0, 0, 1): 3.0,
     (0, 'Noble', 0, 1, 0): 4.0,
     (0, 'Noble', 1, 0, 0): 39.0,
     (0, 'Other', 0, 0, 1): 50.0,
     (0, 'Other', 1, 0, 0): 51.0,
     (1, 'Miss.', 0, 0, 1): 15.0,
     (1, 'Miss.', 0, 1, 0): 9.0,
     (1, 'Miss.', 1, 0, 0): 26.0,
     (1, 'Mr.', 1, 0, 0): 24.0,
     (1, 'Mrs.', 0, 0, 1): 33.0,
     (1, 'Mrs.', 0, 1, 0): 40.0,
     (1, 'Mrs.', 1, 0, 0): 41.0,
     (1, 'Noble', 0, 0, 1): 48.0,
     (1, 'Noble', 1, 0, 0): 33.0,
     (1, 'Other', 1, 0, 0): 49.0}




```python
def compile_age_segment_key (df, col1, col2, col3, col4, col5):
    return list(zip(df[col1], df[col2], df[col3]))
```

```python
def age_map(df, age_col, lookup_col):
    if df[age_col] == -1:
        if df[lookup_col] in age_median_map.keys():
            return age_median_map[df[lookup_col]]
        else:
            return 28
    else:
        return df[age_col]
```


### <a name="fare">Fare</a>

Fare only has one missing value in the training set so we can simply fill it with a sample statistic. I chose median here because average fare price can be thrown off by couple VIP super expensive tickets.


```python
print train['Fare'].mean()
print train['Fare'].median()
```

    32.2042079686
    14.4542


## <a name="model">Modelling and Making Prediction</a>

Alright now that we have cleaned up our features. We have a lot of extra columns when we were doing feature engineering and we won't be using everything. Let's only look at what we want to use.


```python
feature = ['PassengerId', 'Pclass', 'Sex', 'Age', 'SibSp', 'Parch', \
                      'Fare', 'Embarked', 'Title', 'Family Size', 'Individual', \
                      'Cabin - Parsed', 'Ticket - First Label', 'Ticket Label Count',\
                      'Small Family', 'Big Family']
target = ['Survived']
```


```python
train_cleaned = train[feature + target]
```

Because sklearn models doesn't like categorical variables, we need to turn the categorical variables into individual binary vectors.


```python
encode = ['Embarked', 'Title', 'Cabin - Parsed', 'Ticket - First Label', 'Pclass']
train_cleaned = pd.get_dummies(train_cleaned, columns=encode)
test_cleaned = pd.get_dummies(test_cleaned, columns=encode)
```


```python
all_name = train_cleaned.columns
feature_name = all_name.drop(['Survived'])
target_name = 'Survived'
```


```python
X_train = train_cleaned[feature_name]
y_train = train_cleaned[target_name]
```

I've decided to go with tree based based classified for several, but not perfect, reasons. Firstly because tree classifiers provide out-of-the-box tool to identify feature importance based on information gain in the algorithm. This is handy as we can fine tune the model by eliminating not important features that could overfit our model. Secondly I specifically chose RandomForest for its robustness. This is not to say my reasonings are perfect, I should definitely take the time to explore additional models and benchmark it against each other to compare overall predictability.


```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.feature_selection import SelectFromModel
```


```python
clf = RandomForestClassifier(n_estimators=200)
clf = clf.fit(X_train, y_train)
```


```python
feature_importance = pd.DataFrame()
feature_importance['Feature'] = X_train.columns
feature_importance['Importance'] = clf.feature_importances_
```


```python
feature_importance.sort('Importance', ascending=False)
```

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Feature</th>
      <th>Importance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>PassengerId</td>
      <td>0.144271</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Fare</td>
      <td>0.138941</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Age</td>
      <td>0.127578</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Title_Mr.</td>
      <td>0.109264</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sex</td>
      <td>0.100539</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Title_Miss.</td>
      <td>0.039642</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Pclass_3</td>
      <td>0.035005</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Title_Mrs.</td>
      <td>0.033395</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Family Size</td>
      <td>0.032260</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Cabin - Parsed_U</td>
      <td>0.026881</td>
    </tr>
    <tr>
      <th>3</th>
      <td>SibSp</td>
      <td>0.019907</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Pclass_1</td>
      <td>0.017717</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Small Family</td>
      <td>0.015980</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Parch</td>
      <td>0.014885</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Big Family</td>
      <td>0.013341</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Embarked_S</td>
      <td>0.012709</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Ticket Label Count</td>
      <td>0.012216</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Pclass_2</td>
      <td>0.011202</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Embarked_C</td>
      <td>0.010485</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Title_Noble</td>
      <td>0.009934</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Individual</td>
      <td>0.008314</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Ticket - First Label_No Identifier</td>
      <td>0.007968</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Embarked_Q</td>
      <td>0.007566</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Cabin - Parsed_E</td>
      <td>0.006788</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Ticket - First Label_Other</td>
      <td>0.006511</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Cabin - Parsed_B</td>
      <td>0.005857</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Cabin - Parsed_D</td>
      <td>0.005812</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Title_Other</td>
      <td>0.005701</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Cabin - Parsed_C</td>
      <td>0.005364</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Ticket - First Label_pc</td>
      <td>0.004154</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Ticket - First Label_a</td>
      <td>0.003145</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Ticket - First Label_ca</td>
      <td>0.002252</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Cabin - Parsed_A</td>
      <td>0.002128</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Cabin - Parsed_F</td>
      <td>0.001254</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Cabin - Parsed_G</td>
      <td>0.001032</td>
    </tr>
  </tbody>
</table>
</div>



The first classifier is to understand the feature importance based on information gain. Using this information, I remove variables that has less than median importance between all features. Using the remaining features, I retrain the Random Forest Classifier. Using the GridSearch approach provided by sklearn, I do a 10 fold stratified cross validation with different model parameters to see which one gives back the highest accuracy score. This requires a lot of computational resource so I've kept the number of parameters to tune to a limited number. After pruning, we're left with 18 features (~50%) for our Random Forest model


```python
model = SelectFromModel(clf, prefit=True, threshold='median')
X_train_pruned = model.transform(X_train)
```


```python
X_train_pruned.shape
```

    (891, 18)


## <a name="results">Results</a>

```python
from sklearn.model_selection import cross_val_score
from math import sqrt
from sklearn.model_selection import StratifiedKFold
from sklearn.model_selection import GridSearchCV
```


```python
forest = RandomForestClassifier(max_features='auto')

parameters = {
    'n_estimators': [200, 400],
    'max_depth': [7,8,9],
    'max_features': ['auto', 'sqrt', 0.2, 0.1, 0.3]
}

grid_search = GridSearchCV(forest, param_grid = parameters, cv=StratifiedKFold(n_splits=10), scoring='accuracy')

grid_search.fit(X_train_pruned, y_train)
```

From the grid search and 10 fold cross validation, we see that setting a max depth of 9, considering 20% of all features when looking for best split, and 200 estimators in our Random Forest produces the best average accuracy score of 83.6%. Note that this score is not reflective of the actual score that you will get back from kaggle as cross validation is done on the training set.

```python
grid_search.best_score_
```

    0.83613916947250277


```python
grid_search.best_params_
```

    {'max_depth': 9, 'max_features': 0.2, 'n_estimators': 200}


```python
test['Survived'] = pd.Series(grid_search.predict(X_test))
```

```python
test[['PassengerId', 'Survived']].to_csv('titanic_prediction.csv')

```

## <a name="considerations">Considerations / Improvements</a>

As mentioned in this post, the solution here is not perfect and there are many improvements and iterations that can be done on top of this to get a better score. There are a lot of people in this Kaggle competition that has a perfect score, which shows that there are ways to go with this solution. To summarize, if I had more time in the future, I would love to explore different improvements including:

- Use different models including logistic regression, SVM, gradient boosting etc to see if I can get better results
- Finding new and more useful features. E.g. is the passenger a parent?  does last name (representing family and social class) have an effect on survival?
- Use machine learning for filling in missing data like age. E.g. using KNN to find top X similar ppl to infer age


If you have any suggestions, ideas, questions, comments, feel free to shoot me a note and we can chat more!
	