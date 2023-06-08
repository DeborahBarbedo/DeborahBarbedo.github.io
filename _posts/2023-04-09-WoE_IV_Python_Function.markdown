---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 1_WoE_IV_Python_function
title: "Function in Python to calculate WoE and IV"

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Data Analysis
# multiple tag entries are possible
tags: [Python functions, Data analysis, Logistic regression, Feature selection, Information Value (IV), Weight of Evidence (WoE), Predictive modeling, Titanic dataset ]
# thumbnail image for post
img: ":IV_and_WoE _functions_in_Python.jpg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-04-09 20:48:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Learn how to enhance your predictive models with the power of Information Value (IV) and Weight of Evidence (WoE). This page provides step-by-step instructions for creating custom Python functions that calculate WoE and IV, enabling you to accurately evaluate feature predictiveness and improve model performance."
# optional
# please use the "image_viewer_on" below to enable image viewer for individual pages or posts (_posts/ or [language]/_posts folders).
# image viewer can be enabled or disabled for all posts using the "image_viewer_posts: true" setting in _data/conf/main.yml.
image_viewer_on: true
# please use the "image_lazy_loader_on" below to enable image lazy loader for individual pages or posts (_posts/ or [language]/_posts folders).
# image lazy loader can be enabled or disabled for all posts using the "image_lazy_loader_posts: true" setting in _data/conf/main.yml.
image_lazy_loader_on: true
# exclude from on site search
#on_site_search_exclude: true
# exclude from search engines
#search_engine_exclude: true
# to disable this page, simply set published: false or delete this file
#published: false
---

<!-- outline-start -->

Improve Your Logistic Regression Model with Python Functions.

<!-- outline-end -->

Hey, have you ever heard of Information Value and Weight of Evidence? These two stats are a total power duo when it comes to picking predictor variables for logistic regression models. Together, they help improve predictability big time!
Using Information Value and Weight of Evidence, you can see just how effective a variable is in predicting the response you want, and even find out which direction that variable is leaning the response.

But here's the thing: when I started using Python to create and select variables, I realized that was missing a function that could include these metrics in my analysis. So, I decided to make my own functions in Python that would generate tables with WoE and IV.

# WoE and IV for discrete variables

I started off by building a function for discrete variables and kept improving from there.

<script src="https://gist.github.com/DeborahBarbedo/08ed242316fe3b9ed3350460e2a140f3.js"></script>

So, I plugged some data from the [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) competition into my Python WoE and IV function, and let me tell you, the results were seriously eye-opening!

| Survived |        0 |        1 |    Distr |       WoE |       IV | IV_total |
|---------:|---------:|---------:|---------:|----------:|---------:|---------:|
|   female | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
|     male | 0.852459 | 0.318713 | 2.674688 |  0.983833 | 0.525116 | 1.341681 |
|        C | 0.136612 | 0.273529 | 0.499442 | -0.694264 | 0.095057 | 0.122728 |
|        Q | 0.085610 | 0.088235 | 0.970249 | -0.030203 | 0.000079 | 0.122728 |
|        S | 0.777778 | 0.638235 | 1.218638 |  0.197734 | 0.027592 | 0.122728 |


The function generated a table that clearly showed the WoE and IV values for each variable in the dataset, giving me a better understanding of which factors were most important in predicting survival rates.

If you're working with this same dataset (or any other for that matter), I highly recommend giving this function a go. It's a powerful tool for gaining insights into your data and improving your predictive models.

# WoE and IV for continuous variables

When it came to the continuous variables, things got a bit trickier for me. I wasn't entirely sure how to break down these variables for analysis because they can vary so much depending on the problem at hand.

I spent a lot of time brainstorming and drafting several different ideas until I finally settled on breaking everything down into deciles. That way, I could analyze the IV return in larger portions (like quartiles) if needed.

Anyway, after a lot of hard work, I finally came up with a function for the continuous variables.

<script src="https://gist.github.com/DeborahBarbedo/d9ddd529f9b4359e4a867a649ab9544b.js"></script>


It was definitely a challenge, but the end result was super satisfying!

| variable |                 limit |        0 |        1 |    Distr |       WoE |   IV |
|---------:|----------------------:|---------:|---------:|---------:|----------:|-----:|
|      Age |               <=[14.] | 0.058288 | 0.131579 | 0.442987 | -0.814214 | 0.06 |
|      Age |         [14.] a [19.] | 0.096539 | 0.099415 | 0.971070 | -0.029356 | 0.00 |
|      Age |         [19.] a [22.] | 0.087432 | 0.055556 | 1.573770 |  0.453474 | 0.01 |
|      Age |         [22.] a [25.] | 0.080146 | 0.076023 | 1.054224 |  0.052805 | 0.00 |
|      Age |         [25.] a [28.] | 0.067395 | 0.070175 | 0.960383 | -0.040424 | 0.00 |
|      Age |        [28.] a [31.8] | 0.072860 | 0.076023 | 0.958386 | -0.042505 | 0.00 |
|      Age |        [31.8] a [36.] | 0.085610 | 0.128655 | 0.665425 | -0.407330 | 0.02 |
|      Age |         [36.] a [41.] | 0.061931 | 0.055556 | 1.114754 |  0.108634 | 0.00 |
|      Age |         [41.] a [50.] | 0.085610 | 0.090643 | 0.944474 | -0.057127 | 0.00 |
|      Age |                       | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.09 |
|     Fare |              <=[7.55] | 0.143898 | 0.038012 | 3.785624 |  1.331211 | 0.14 |
|     Fare |     [7.55] a [7.8542] | 0.111111 | 0.076023 | 1.461538 |  0.379490 | 0.01 |
|     Fare |     [7.8542] a [8.05] | 0.158470 | 0.055556 | 2.852459 |  1.048181 | 0.11 |
|     Fare |       [8.05] a [10.5] | 0.109290 | 0.052632 | 2.076503 |  0.730685 | 0.04 |
|     Fare |    [10.5] a [14.4542] | 0.087432 | 0.105263 | 0.830601 | -0.185606 | 0.00 |
|     Fare | [14.4542] a [21.6792] | 0.092896 | 0.108187 | 0.858662 | -0.152380 | 0.00 |
|     Fare |     [21.6792] a [27.] | 0.078324 | 0.134503 | 0.582324 | -0.540729 | 0.03 |
|     Fare |     [27.] a [39.6875] | 0.103825 | 0.099415 | 1.044359 |  0.043403 | 0.00 |
|     Fare | [39.6875] a [77.9583] | 0.076503 | 0.137427 | 0.556679 | -0.585766 | 0.04 |
|     Fare |                       | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.37 |


# All together now

Once I got over the initial obstacles, I decided to tackle creating another function. This one would combine the metrics for both discrete and continuous variables into a single, comprehensive table.

I must admit, this function was way easier and quicker to make! So, without further ado, check out the function I whipped up to bring it all together in one table:

<script src="https://gist.github.com/DeborahBarbedo/bc3597b64ad2fcd54266664c62adbe3f.js"></script>

The final table is:

| variable |                 limit |        0 |        1 |    Distr |       WoE |       IV | IV_total |
|---------:|----------------------:|---------:|---------:|---------:|----------:|---------:|---------:|
|   female |                       | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
|     male |                       | 0.852459 | 0.318713 | 2.674688 |  0.983833 | 0.525116 | 1.341681 |
|        C |                       | 0.136612 | 0.273529 | 0.499442 | -0.694264 | 0.095057 | 0.122728 |
|        Q |                       | 0.085610 | 0.088235 | 0.970249 | -0.030203 | 0.000079 | 0.122728 |
|        S |                       | 0.777778 | 0.638235 | 1.218638 |  0.197734 | 0.027592 | 0.122728 |
|      Age |               <=[14.] | 0.058288 | 0.131579 | 0.442987 | -0.814214 | 0.060000 |          |
|      Age |         [14.] a [19.] | 0.096539 | 0.099415 | 0.971070 | -0.029356 | 0.000000 |          |
|      Age |         [19.] a [22.] | 0.087432 | 0.055556 | 1.573770 |  0.453474 | 0.010000 |          |
|      Age |         [22.] a [25.] | 0.080146 | 0.076023 | 1.054224 |  0.052805 | 0.000000 |          |
|      Age |         [25.] a [28.] | 0.067395 | 0.070175 | 0.960383 | -0.040424 | 0.000000 |          |
|      Age |        [28.] a [31.8] | 0.072860 | 0.076023 | 0.958386 | -0.042505 | 0.000000 |          |
|      Age |        [31.8] a [36.] | 0.085610 | 0.128655 | 0.665425 | -0.407330 | 0.020000 |          |
|      Age |         [36.] a [41.] | 0.061931 | 0.055556 | 1.114754 |  0.108634 | 0.000000 |          |
|      Age |         [41.] a [50.] | 0.085610 | 0.090643 | 0.944474 | -0.057127 | 0.000000 |          |
|      Age |                       | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.090000 |          |
|     Fare |              <=[7.55] | 0.143898 | 0.038012 | 3.785624 |  1.331211 | 0.140000 |          |
|     Fare |     [7.55] a [7.8542] | 0.111111 | 0.076023 | 1.461538 |  0.379490 | 0.010000 |          |
|     Fare |     [7.8542] a [8.05] | 0.158470 | 0.055556 | 2.852459 |  1.048181 | 0.110000 |          |
|     Fare |       [8.05] a [10.5] | 0.109290 | 0.052632 | 2.076503 |  0.730685 | 0.040000 |          |
|     Fare |    [10.5] a [14.4542] | 0.087432 | 0.105263 | 0.830601 | -0.185606 | 0.000000 |          |
|     Fare | [14.4542] a [21.6792] | 0.092896 | 0.108187 | 0.858662 | -0.152380 | 0.000000 |          |
|     Fare |     [21.6792] a [27.] | 0.078324 | 0.134503 | 0.582324 | -0.540729 | 0.030000 |          |
|     Fare |     [27.] a [39.6875] | 0.103825 | 0.099415 | 1.044359 |  0.043403 | 0.000000 |          |
|     Fare | [39.6875] a [77.9583] | 0.076503 | 0.137427 | 0.556679 | -0.585766 | 0.040000 |          |
|     Fare |                       | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.370000 |          |

Hey, did you liked my Python functions for calculating IV and WoE? I had a blast creating them, but I've realized that there are still a lot of folks out there who might be feeling a bit lost when it comes to these metrics.

Since they can be so helpful in improving your logistic regression models, I've decided it's high time to write up a post and explain everything in detail. I'm going to walk you through [how to calculate IV and WoE](https://deborahbarbedo.github.io/posts/2023-06-05-WoE_IV_Calculation) and give you some seriously valuable tips on [how to use these metrics](https://deborahbarbedo.github.io/posts/2023-04-24-Unpacking_WOE_and_IV) to make savvy decisions and elevate your models to the next level.

I'm really excited to share all of this with you, so don't miss out! Keep your eyes peeled for my next post!

# References:

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* [Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  https://CRAN.R-project.org/package=woe](https://CRAN.R-project.org/package=woe)

* [Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. https://CRAN.R-project.org/package=woeBinning](https://CRAN.R-project.org/package=woeBinning)
