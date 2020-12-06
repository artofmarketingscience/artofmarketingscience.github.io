---
layout:			post
title:			"What are Geo Experiments and how it is used in marketing"
tagline:		"What are Geo Experiments and how it is used in marketing"
author:			art_of_marketing_science
categories:		["Art of Marketing Science"]
description:	"Geo Experments are used to measure causality and incrementality when traditional A/B testing cannot be used"
excerpt:		"Geo Experments are used to measure causality and incrementality when traditional A/B testing cannot be used"
image:			/assets/images/2019-01-23-geo_experiments_part1_what_is_it_and_how_will_it_help_you_in_marketing/map_with_action_figure.jpeg
redirect_from:	/AOM-geo-experiments-part1/
featured: 		true
---

This will be a three-part series discussing the topic of Geo Experiments and it's use in marketing.

**Part 1**: What Is It and How Will It Help You In Marketing?

**Part 2**: Understanding the mathematics behind Geo Experiments

**Part 3**: Application of Geo Experiments with examples and R code

### The Bread & Butter of Test & Learn

A/B testing (aka split testing) has been essential in helping marketers remove the guesswork and make data-informed decisions. For most marketers, this comes in the form of landing page optimization. You split traffic into different groups, expose some changes to the page to one of the groups while keeping everything else the same for the other, measure the difference for a specific metric of interest (signup rate, CTR etc) and determine if the difference between the two groups is statistically significant. Some paid marketing specialists may have worked with A/B testing in Google Ads or Facebook Ads, where the audience is subjected to different ad formats or ad copy. Similar methodology for both but applied differently.

So why do marketers run A/B tests? This is where marketers put on their scientist hat. Marketers run A/B testing to understand the causal and incremental impact of something, whether it be a change on a landing page or ad copy for their Facebook campaign. They want to know, for this specific thing that they did, what is the incremental impact that it had, so that they can make informed decisions about what they should do next.

A/B testing has been the bread and butter for marketers to do tests and learn. But if I were to ask you to design an A/B test to answer each of the following questions, how would you go about doing so?

1. Understand the incremental impact of a Facebook campaign on customer conversion? (Meaning, I want to know the actual impact on overall customer conversion and not what’s reported in your Facebook Business Manager under the conversion column)
2. Understand the effect of billboard advertisement on visits to your website

**Give it some thought.**

![geo_experiment_hour_glass](/assets/images/2019-01-23-geo_experiments_part1_what_is_it_and_how_will_it_help_you_in_marketing/hour_glass.gif "geo experiment hour glass")

Let’s go through each question and see why our favourite A/B test doesn’t work:

1. You can A/B test your Facebook campaign to understand the incremental impact for metrics like CTR or engagement rate but customer conversion is a little different. Why? Because under your whole measurement framework lies an attribution model that assigns credit in a non-perfect way. As a result, even though you are running an A/B test to understand customer conversion, it only provides you with a relative comparison between the two campaigns. It does not provide an overall view of whether the campaign brought in incremental customers. You do not know if whether you’re simply taking customer conversions away from other channels.

2. This one is straightforward. Firstly, you just don’t have any direct response metrics to measure. Secondly, how do you plan to create your control and test group? Do you plan to blindfold half the population in a city that passes by the billboard to ensure they don’t see the billboard while the letting other half see it?

A/B testing does not work when you do not have control over your population or have a direct measure of the metric of interest. Although A/B testing is the bread and butter for marketers we also see it’s limitations and the need for additional tooling to help marketers answer important questions.

### The Bread & Olive Oil: Geo Experiments

Sometimes bread and butter just isn't enough and you got to switch things up. Bread and olive oil usually goes well together too. So what exactly are Geo Experiments? Well, in the simplest terms, Geo Experiments are like A/B tests but uses geographies to define your control and treatment group rather than individuals or web cookies. In Geo Experiments, geographic regions are divided into treatment and control group. The regions in the treatment group are exposed to a marketing intervention while regions in the control group remain status quo. The marketing intervention happens for a duration of time and the response metric is observed. The idea is to detect if there is an incremental lift in the response metric in the treatment regions.

Let’s use the Facebook question above as an example and assume we are running the ad campaign in the United States. First, we randomize a set of cities in the United States putting them into treatment and control group. Just like A/B tests, randomization is a critical part of the experimental design. Then in Facebook Business Manager, we set the campaign to only run in the cities in the treatment group. We run the campaign for a duration of time and we measure the incremental customers acquired (agnostic of an attribution model) by comparing the difference between the control vs. treatment group. For example, we may see something like:

![geo_experiment_results](/assets/images/2019-01-23-geo_experiments_part1_what_is_it_and_how_will_it_help_you_in_marketing/geo_experiment_package.png "geo experiment results")

This is a graph produced by the [GeoexperimentsResearch](https://github.com/google/GeoexperimentsResearch) R package developed at Google which implements the Geo Experiment methodology and provides an easy way to perform the analysis. I’ll be demonstrating how to use this R package in Part 3! But at a high level, the y-axis can be customer conversion for our example purposes, the x-axis is divided into three distinct periods:

- Pre-test period (Feb 05 — Mar 31): before the campaign started
- Test period (Apr 01 — Apr 28): when the campaign is running
- Cooldown period (Apr 29 — May 05): campaign stops running but there might be a lingering effect of the ad that trickle in

The top graph illustrates the observed response metric overtime vs the predicted counterfactual. We’ll talk more about this in detail in Part 2 but essentially the counterfactual is what we would have observed if the marketing campaign did not run. The middle graph illustrates the difference between the observed and counterfactual by day which estimates the lift by day. Finally, the bottom graph is the total lift we’ve observed in the experiment.

My explanation above is an oversimplification of the methodology. To truly understand what’s going on, you need to go inside the hood and look at the engine. The methodology and math behind Geo Experiment are different from a traditional A/B test but the overall idea is similar. As mentioned, I will be leaving the detailed explanation of the mathematics to Part 2 of this series. The methodology that I’ll be going through in Part 2 is research from Google, so if you are interested, take a look first. A one-liner explanation: a regression model is used to learn the exchangeability factor between the control regions and the treatment regions and then used to predict the counterfactual during the intervention period. Maybe this sentence doesn’t make any sense to you at all. Don’t worry, I will be breaking this down in simple terms! Stay tuned!

I want to take a short moment to highlight that Geo Experiments are another use case of regression in marketing science. If you haven’t read my article ["Regression, a must have for Marketing Analysts"](./regression-must-have-for-marketing-analysts), take a look to see why regression is a tool every Marketing Analyst should have in their toolkit.

By no means are Geo Experiments designed to replace traditional A/B test. They serve very different purposes and answers different questions but both are equally important. Just as they say, “there is no one size fits all”, you shouldn’t just have butter with your bread. Go ahead and add some variety, olive oil is great too!

### Challenges and Limitations

By now, I might have sold you on how great this new tool is BUT, like any other tools, Geo Experiment comes with its own set of limitations and challenges:

- **Overhead Cost**: There may be a lot more work involved to set up a Geo Experiments depending on how you’ve set up your campaigns. You may need to restructure your existing digital marketing accounts in a way that allows you to target campaigns at a city/region level to create your control and treatment group.
- **Platform**: Not every marketing platform you advertise on allows you to target campaigns at a city/region level. For example, you may be able to target Facebook paid ads at a city/region level but you don’t have that capability when you are running ads in podcasts.
- **Budget**: Depending on your business, campaign, and many other factors, the budget you need for a campaign will greatly vary. For companies that are just starting out with digital marketing, I wouldn’t be looking at Geo Experiments just yet. If your business is mature and you’re looking to optimize, then I think its a good fit.
- **Controlling for variables**: Can you control for marketing activities happening across different cities? E.g are there other marketing initiatives from your organization happening in specific cities that might overlap with your Geo Experiment?

And finally, alway’s remember to choose the right tool for the right job. There is no one size fits all!
