---
title: "Regression Modelling Strategies"
layout: post
date: 2018-06-02
image: /assets/images/markdown.jpg
headerImage: false
tag:
- level up
star: true
category: blog
author: jason fong
description: Regression Modelling Strategies
---

I feel very fortunate to work for an employer like Shopify, that places a lot of value in continuous learning and self development. One of the many benefits that we get as employees is an annual self-development budget that we can use on anything to level-up ourselves professionally. This year I decided to experiment with taking a 5 day short course on Regression Modelling Strategies offered at Vanderbilt University. This blog entry is a summary of sed course material, my experience and recommendations for other Data Scientists in the industry that might be interested in diving deeper into the topic of regression. 

# Inspiration

The most common way people tend to use their development budget is by attending conferences. I also know of some coworkers who have used their development budget over several years to pursue a formal University certification in Big Data / Data Science. Personally, I have mix feelings about both and I don’t find either one to fit my learning style very well. I’ve been to couple conferences related to data science and marketing but I felt I wasn’t able to take a whole lot away. Some felt way too product and sales focused while others lacked the content that I was looking for. The challenge that I have with some of these University Big Data certificate programs is the format of the education. Typically the format is a set of courses over several semesters and classes are mandatory on a weekly basis, with midterms, assignments, and exams. What I struggle with is the pace of the learning, I feel that it is too slow for me. I also dislike the breadth and lack of depth of the content. I feel like a lot of topics are usually covered in such programs but none are explored deeply. At this point of my career, I feel I have a better sense of what I know, what I don’t know, and what I want/need to know, which has consequently made me explore alternative options for my development budget.

Browsing on Twitter ultimately led me to Frank Harrell’s RMS course. I was very intrigued by the concept of a condensed course that focused on a topic that I wanted to learn more about and thought was very relevant for my day to day work.

# Course Summary

The course I took is called Regression Modelling Strategies, taught by Professor [Frank Harrell](https://twitter.com/f2harrell?lang=en) at Vanderbilt University. This is a condensed version of the course that he regularly teaches during the semester. The course is broken up into 1 day (optional) R tutorial and 4 full days of course content. The course is intended for Masters and PhD level students, so there is an expectation that students already have a competent understanding of multiple regression modelling. The course becomes very challenging to follow along if students do not have that understanding or are not prepared for the lectures. For more information about the course, see [here](http://biostat.mc.vanderbilt.edu/wiki/Main/RmS).

The philosophy of the course and Professor Frank Harrell can be summarized as:
* It is more productive to make model fit step by step than to postulate a simple model and find out what went wrong
* Carefully fitting improper model is better than badly fitting a well chosen one
* A good overall strategy is to decide how many degrees of freedom (regression parameters) can be “spent”, where they should be spent, and to spend them with no regrets

Topics covered in the course include but not limited to the following:

* Hypothesis Testing vs Estimation vs. Prediction vs. Classification
* General Aspects of Fitting Regression Models
  * Interpreting Model Parameters & Understanding Interactions
  * Relaxing Linearity Assumptions for Continuous Predictors
    * Non Linear Terms
    * Splines for Estimating Shape of Regression Function
    * Cubic / Restricted Cubic Splines / Choosing Number and Position of Knots
    * Nonparametric Regression
    * Assessment of Model Fit
* Handling Missing Data
  * Strategies for Developing Imputation Model
  * Predictive Mean Matching
* Multivariable Modelling Strategies
  * Variable Selection, Sample Size, Overfitting, Limit on Number of Predictors
  * Shrinkage
  * Data Reduction Techniques
* Describing, Resampling, Validating and Simplifying the Model
  * Model Validation
  * Bootstrap vs. Cross Validation
* Binary Logistic Regression, Ordinal Logistic Regression, Survival Analysis
* Case Studies 

For a more detailed list of topics, [click here](http://biostat.mc.vanderbilt.edu/wiki/pub/Main/RmS/rmsDescription.pdf).

# Who Should Attend This Course?

The course description illustrates the target audience as “statisticians and related quantitative researchers who want to learn some general model development strategies, including approaches to missing data imputation, data reduction, model validation, and relaxing linearity assumptions”. In my opinion, I feel this topic / course is suitable for anyone who is working with data, primarily focusing on problems that require a good understanding of how variables interact with each other and affect outcome. My domain of interest is marketing and I am interested in answering questions such as, what levers affect churn or LTV, and at what magnitude? How does each marketing channel influence customer conversion?

This is a traditional statistics course, with a focus on biostatistics, and not a machine learning / data science course. I don’t recommend this for anyone that is only interested in the prediction aspect of modelling and not the interpretation aspect. Although this is a biostatistics course, I think the content is relevant for people outside of the medical field as well. My class comprised of primarily doctors, postgraduate students, researchers or people from the medical industry, but there were also people from the finance sector, geology, etc. In my opinion, I actually think the biostatistics aspect of the course is beneficial and makes the course much more interesting. Surrounding yourself with people that are not in your field and looking at problems that you don’t look at on a day to day basis helps stimulate thoughts and creativity. 


# Recommendations 

Overall I highly recommend other people in the industry to take this course if they are interested in learning more about regression modelling and also have similar reservations towards conferences and long form education. I think there are couple things that can really help enhance this experience (from the perspective of someone coming from industry and not academia):

* Going as a team (4-5 people): Not only is this option more cost efficient, I think you can learn a lot more when you are studying as a group, by asking each other questions, sharing ideas and thoughts, challenging ideas etc.
* Being very prepared for the course so that you can have meaningful conversations with Professor Frank Harrell. There were several students that were taking the course for the 2nd or 3rd time and I noticed that they were able to have a deep conversations with the Professor about specific problems they are working on. There is a textbook that comes with this course, and I would highly encourage going over that material before attending the course as well during the course.
* Course improvements mentioned below.

# Improvements

The course is not perfect and there are definitely improvements that can be made. I’ve provided the same feedback on the course feedback survey. 

* The R tutorial is not necessary. I would prefer the R tutorial to be removed and allot an extra day for the course. The R content can be sent out prior to the course for students to review individually
* There is a lack of interaction / collaboration amongst the students. I think having students from very diverse fields in one place is in itself a very valuable. The concept of having a mini group project intrigues me. Replacing the R tutorial day with extra half day of course content and half day of project presentation is also an intriguing idea
* Go deeper into survival analysis and ordinal regression
* Price for non institute members is fairly expensive and does not include airfare and accomodations, so that is something to consider

# Takeaway

I am very happy that I experimented with using my development budget on a short course. One thing to keep in mind is that you should not make the assumption that after a 5 day course you will know everything about regression modelling. That is simply impossible.  You will need to continue to self study but the course provides you with the foundation to continue exploring this topic further. I am currently still reading/re-reading the course textbook and course notes. At the same time I am reading academic papers that apply regression in the realm of marketing and I  find that I am able to understand the methodologies behind the modelling process. 

If you have any thoughts/comments/questions about my experience, feel free to reach out over [Twitter](https://twitter.com/fongmanfong) or email. I would love to help address any questions or further share with you my experience. If you know of any other good short courses, please please please reach out. I would love to learn more about it!
