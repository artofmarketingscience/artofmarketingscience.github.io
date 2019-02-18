---
layout: post
title:  "From Marketing Analytics To Marketing Science"
author: art_of_marketing_science
categories: ["Art of Marketing Science"]
image: /assets/images/2018-12-10-from_marketing_analytics_to_marketing_science/marketing_science_experiment.png
---

_“Half the money I spend on advertising is wasted; the trouble is I don’t know which half.”_

The changes in the technology landscape in the past two decades have evolved the marketing industry as a whole, creating an entirely new field called digital marketing. First the dot com and internet boom, followed by an explosion of web data, and finally creation of cheap and readily available web analytics tools, digital marketers have much more insight into how their marketing campaigns are performing. Still which this much data and tools, marketers still have trouble answer this question:

![jpeg](/assets/images/2018-12-10-from_marketing_analytics_to_marketing_science/christian_bale_idk.webp)

Web analytic tools like Google Analytics & Adobe Omniture has made marketing analytics a commodity and a marketer’s best friend. With a click of few button, marketers have the ability to see how many clicks, impression and even conversion their marketing campaigns have generated. They can see how their campaigns stack up against each other and overtime, turning marketing into a analytics and metrics driven profession. 

![gif](/assets/images/2018-12-10-from_marketing_analytics_to_marketing_science/simon_gibson_math_gif.gif)

But since then, the move from purely analytics approach to more science-based has been slow. You probably now questioning what I mean by science vs analytics here so let’s take a quick moment to explain how I define the two:

**Analytics approach** → Answering business questions by leveraging descriptive and predictive analytics
- Looking at the data overtime and identifying trends. Are things doing better or worse? 
- Segmenting / slicing & dicing the data to look at it from different perspectives. Are certain segments doing better or worse?
- Making decisions based on correlation between variables. When X moves, Y seems to move as well, so let’s do more of X.
- Using the data to make predictions in the future. Will this person churn?

**Science approach** → interpreting data and drawing inference with scientific rigor
- Attempting to find causality between variables, what’s causing what and at what magnitude
- Looking at data in terms of estimate, probability, confidence interview, posterior distribution. I’m not sure what X is but I’m fairly it’s between Y and Z
- Experimental design to prove/disprove hypothesis. Running randomized trial.

One science approach that marketing has adopted is experimental design in the form of A/B testing. Although widely used within marketing (and other areas), I’d argue a lot of the designed experiments implemented in many corporations are questionable. This will be a separate post. 

I believe that marketing analytics technology has matured enough and as marketers and data analyst, we should look at marketing from a scientific lens in additional to traditional analytics.

### I’m Not Sure If I Understand The Difference

My explanation above might be too theoretical and high level. Let’s go through an example to better understand. 

Assume Sally is a marketer and she does SEM for Company X. She sets up a new campaign targeting `Company X` as the keyword. After a week she sees that her campaign received 100K impressions, 5000 clicks, 50 customers and spent a total of $500. The first question that needs to be answered is: “are the results good or bad”? 

How would Sally go about answering this question? A general approach would be benchmark her new campaign with existing campaigns, comparing CPC and CAC. Let say she compares across several campaigns and on average CPC is around $0.20 and CAC is around $20, meaning her new campaign is 2x more efficient. This is great, seems like this campaign is killing it, let’s double down and spend more on this campaign. 

This is typically how marketing data is interpreted and actioned on. Marketers and data analyst looks at overall trends and make a decision based on insight drawn.

So what’s the problem here? Well several key problems:
- We never actually answered whether the campaign is performing well. We only looked at whether this campaign performed relatively well compared to historical aggregate of data
- What if all this campaign did was take from other campaigns?
- What is the probability that the results we saw happened by chance?

What we should be interested in here instead is the incremental benefit of having this campaign to get a true sense of CAC. Sure the results might look great at face value but what if all it’s doing is taking away traffic from your Organic Search. Meaning these clicks would of been captured through Organic Search anyways. Actually in this case, the campaign is adding negative value since now Company X is paying for “free” traffic it would of gotten anyways.

### Ok, So How Can I Be More Scientific With Marketing?

This is just one example (though very common) of using marketing analytics to drive “insights”. As seen, the problem with pure analytics approach is that we might not be answering the true question of interest. At best we may be making decisions based on signals that are highly correlated with the truth, at worst we are making decisions that are purely noise. Imagine if your company has a nine figure marketing budget, every decision becomes very expensive and critical to get right.

![gif](/assets/images/2018-12-10-from_marketing_analytics_to_marketing_science/jon_stewart_cry.gif)

So how can we answer this question? Methods and techniques to answer this question goes back to what this article is about, scientific approaches including experimental design and regression analysis can be used to help answer this question. I won’t go into too much detail in this introductory article (will in future articles) but designing a geo experiment or building a regression based model to understand the counterfactual are techniques that can be used.

### A Mindset Shift

![gif](/assets/images/2018-12-10-from_marketing_analytics_to_marketing_science/think.webp)

Evolving from marketing analytics to marketing science first requires a shift in mindset. We need to understand that more data does not mean more data informed decisions. We need to look at each problem and insight and critic it with rigor to determine if we are simply making inference on correlation or causation. I’m not saying that analytics is not useful to make decisions, far from that. Analytics provide a way to draw quick “insights” (better than just throwing darts in the dark). What I’m highlighting is the need to complement analytics with more science to make sure we’re hitting bullseye.
