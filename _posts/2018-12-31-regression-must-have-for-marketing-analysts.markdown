---
layout:			post
title:			"Regression, a must have for Marketing Analysts"
tagline:		"Regression, a must have for Marketing Analysts"
author:			art_of_marketing_science
categories:		["Art of Marketing Science"]
description: 	"Regression is a must have swiss army knife for marketing analysts, with application like geo experiments and customer lifetime value models."
excerpt:		"Regression is a must have swiss army knife for marketing analysts, with application like geo experiments and customer lifetime value models."
image:			/assets/images/2018-12-31-one_thing_marketing_analysts_should_have_in_their_analytics_toolkit//toolbox.jpeg
redirect_from:	/AOM-one_thing_marketing_analysts_should_have_in_their_analytics_toolkit/
---

Let’s be real here, being a data scientist ain’t easy. Have you seen what a modern data scientist should know?! 😱

![the modern data scientist](/assets/images/2018-12-31-one_thing_marketing_analysts_should_have_in_their_analytics_toolkit//modern_data_scientist.png "the modern data scientist")

That’s a pretty extensive list of requirements but how feasible and practical is it to know everything on this list? In my opinion, not that feasible nor practical. In reality, a data scientist shouldn’t need to know everything mentioned (at least with the same level of competency). Instead, the problem space that a data scientist is working in should define the set of tools he/she needs. Similar to doctors, you either have family doctors who are generalists or you have doctors specializing in different areas. You don’t expect one doctor to know everything and be a specialist in everything.


So for a marketing analyst or data scientist in marketing, what are some tools they should have in their toolkit? Most of the time we hear about things like machine learning, R, Python, domain knowledge, communication skills etc. but one tool that has not gotten enough spotlight in marketing is **regression**. To answer why regression should be in every marketing analysts analytics toolkit, we should first think about what makes a good tool for marketing.

### What Makes A Good Tool For Marketing?

And to answer this question, we should think about what problems or questions a marketing organization or a CMO needs to answer. Some bigger questions include:

- What is the growth and revenue for the company?
- What is a customer’s lifetime value?
- Who is at risk of churning?
- Who should we send this promotional offer to?
- What is the ROI of my advertising investment?

In theory, a good tool should help with answering some of these questions (but it would be naive to think that there is one tool that can solve all the problems aka No Free Lunch Theorem). In addition to just answering these questions, what’s equally as important for a CMO is understanding what levers are moving the needle.

![pulling the right lever](/assets/images/2018-12-31-one_thing_marketing_analysts_should_have_in_their_analytics_toolkit//levers.gif "pulling the right lever")

For example, it’s not enough to know whether a company is growing or if revenue is increasing, a CMO needs to know what is moving the needle. Who is at risk of churning and what signals indicate a high risk of churning? Therefore a good tool in marketing should not only help with answering some of these questions but also provide insight into what variables are moving the needle.

### What is Regression?
I won’t be going into any details on regression in this article. I have some resources at the end if you are interested in learning more about regression. At a high-level, regression is a family of statistical analysis techniques that examine the relationships between variables. There are different types of regressions (logistic, linear, survival etc) in the family but the overall objective is to model the relationship between variables.

For example, we may be interested in understanding how investment in each marketing channel influences customer acquisition. We can build a model where the independent variables (factors you hypothesize to have a relationship) is the investment in each marketing channel and the dependent variable (factors you are trying to understand) being customers acquired. It could hypothetically look something like

Customers Acquired = B0 + B1 * AdWords_Spend + B2 * Facebook_Spend + B3 * Snapchat_Spend

### Oh No, Equations and Formulas 😰

![oh no not equations](/assets/images/2018-12-31-one_thing_marketing_analysts_should_have_in_their_analytics_toolkit//oh_no.gif "oh no not equations")

The beauty of regression lies in the equation. Why? Because if we’ve modeled the data appropriately, we can make interpretations of the coefficients (the Betas) in the equation to **make decisions**. This is different than some of the more popular machine learning techniques like deep learning, random forest etc. which are powerful tools for making predictions but the blackbox nature of the method makes it not interpretable.

Depending on how the data is modeled (e.g. logistic vs linear vs log-log) we can interpret the coefficient in different ways to understand the relationship between a dependent variable to the independent variable. Let’s take the equation above as an example: we’ve used linear regression to model marketing spend versus customers acquired. The coefficients can be interpreted as a dollar spent in Google Adwords gets 2 number of customers.

_Customers Acquire = 100+ 2.0 * AdWords_Spend + 1.3 * Facebook_Spend + 0.5 * Snapchat_Spend_

In other words, the coefficients can be interpreted as the levers that a CMO would be interested in knowing.

Regression also provides nice statistical properties including, but not limited to, statistical significance, confidence interval, & standard error of each estimator (our coefficients of interest) which provides a CMO with upper/lower bound of estimates and level of confidence for our estimates. ([From Marketing Analytics To Marketing Science](/from-marketing-analytics-to-marketing-science/) I talk more about why we should look at marketing problems from more of a scientific lens)

### Swiss Army Knife of Data Science

We should also appreciate regression for its versatility and the wide variety of applications within marketing. Regression is primarily used for:

- Predictive Analytics — regression is also a foundational piece for predictive analytics. This is what we’ve been talking about so far. We model the relationships between variables to make predictions about the value of the independent variable based on what we know for the dependent variables.
- Causal Inference — extending beyond just modeling the relationship between variables, we can leverage regression to make causal inference. Meaning making causal claims between variables.

Some applications in marketing include:

- Geo experimentation
- Causal impact of marketing events including IRL / Offline Marketing
- Leading scoring
- Product marketing and upsell / cross-sell
- Media mix modeling and budget optimization
- Forecasting

### Learning Regression

I wish learning regression was as simple as doodling some dots and drawing a line through them but the reality is that it is a vastly deep and complicated subject. But there are good news!

1. There is a saying “Nothing in life worth having come easy.” Therefore regression must be worth having.
2. You don’t need to master regression before applying it. It is one of those subjects that you build up knowledge over time. There are plenty of Ph.D. students in statistics still learning regression.

I have a couple links below as a starting point. If you have any good resources, please share in the comments below as well for others!

[Introduction To Regression Model (Coursera)](https://www.coursera.org/lecture/wharton-quantitative-modeling/4-1-introduction-to-regression-model-DEymf)

[Penn State — Stats 501 Regression Methods](https://newonlinecourses.science.psu.edu/stat501/node/250/)

[Mastering ‘Metrics: The Path from Cause to Effect](https://www.amazon.com/Mastering-Metrics-Path-Cause-Effect/dp/0691152845)

[Regression Modeling Strategies — Frank Harrell (Textbook)](https://www.amazon.ca/Regression-Modeling-Strategies-Applications-Logistic/dp/0387952322)

[Regression Modeling Strategies — Frank Harrell (Course)](https://artofmarketingscience.github.io/DS-regression_modelling_strategies/)
