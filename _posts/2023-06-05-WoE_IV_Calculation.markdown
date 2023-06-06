---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 3_WoE_IV_Python_calculation
title: "Mastering Logistic Regression: A Comprehensive Guide to WoE and IV Calculation."

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Data Analysis
# multiple tag entries are possible
tags: [Data analysis, Logistic regression, Feature selection, Information Value (IV), Weight of Evidence (WoE), Predictive modeling, Titanic dataset ]
# thumbnail image for post
img: ":WoE_IV_Python_calculation.jpg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-06-05 11:48:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Learn how to calculate and understand the Weight of Evidence (WoE) and Information Value (IV) metrics, which are widely used to differentiate between creditworthy and non-creditworthy individuals. This blog post explores these calculations using the Titanic - Machine Learning from Disaster dataset, focusing on survival information segregated by gender. Demystify the concepts of IV, WoE, and RR (Relative Risk) to make them more accessible and tangible. Dive into the provided data and tables to gain insights and apply these metrics effectively."
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

 A Comprehensive Guide to WoE and IV Calculation.

<!-- outline-end -->

These metrics are widely recognized for their ability to discern between creditworthy and non-creditworthy individuals. Throughout our journey into understanding these calculations, we frequently encounter the familiar labels of 'good' and 'bad' customers. In this context, 'bad customers' are those who have defaulted on their loans, while 'good customers' are those who have dutifully fulfilled their financial obligations.

To shed light on these concepts, we will draw insights from the [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) dataset, specifically examining the survival information segregated by gender. Our aim is to demystify the calculations of IV, WoE, and RR, making them more approachable and tangible. We will utilize the provided data in the table below as a foundation for our exploration.


| Sector | # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" /> | # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{1}" title="https://latex.codecogs.com/svg.image?\small target_{1}" /> |
|--------:|--------:|--------:|
| female | 81      | 233     |
| male    | 468     | 109     |
| Total   | 549     | 342     |

What is commonly referred to as 'good' is the <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />.

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;target_{0,&space;sector_i}&space;=&space;\frac{&space;target_{0,&space;sector_i}}{&space;target_{0}}&space;" title="https://latex.codecogs.com/svg.image?\small % target_{0, sector_i} = \frac{ target_{0, sector_i}}{ target_{0}} " />

Let's consider the chosen sector as female.

For this problem:

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;survived_{0,&space;female&space;}&space;=&space;\frac{&space;survived_{0,&space;female}}{&space;survived_{0}}&space;=\frac{81}{81&plus;468}&space;\approx&space;0.147541&space;" title="https://latex.codecogs.com/svg.image?\small % survived_{0, female } = \frac{ survived_{0, female}}{ survived_{0}} =\frac{81}{81+468} \approx 0.147541 " />

What is typically described as 'bad' is the <img src="https://latex.codecogs.com/svg.image?\small&space;target_{1}" title="https://latex.codecogs.com/svg.image?\small target_{1}" />.

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;target_{1,&space;sector_i}&space;=&space;\frac{&space;target_{1,&space;sector_i}}{&space;target_{1}}&space;" title="https://latex.codecogs.com/svg.image?\small % target_{1, sector_i} = \frac{ target_{1, sector_i}}{ target_{1}} " />

For this problem, in the female sector:

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;survived_{1,&space;female}&space;=&space;\frac{&space;survived_{1,&space;female}}{&space;survived_{1}&space;}&space;=&space;\frac{233}{233&plus;109}&space;\approx&space;0.681287" title="https://latex.codecogs.com/svg.image?\small % survived_{1, female} = \frac{ survived_{1, female}}{ survived_{1} } = \frac{233}{233+109} \approx 0.681287" />

| Sector | # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" /> | # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" /> | % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" /> | % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" /> |
|--------:|--------:|--------:|--------:|--------:|
| female  | 81      | 233     | <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{81}{594}" title="https://latex.codecogs.com/svg.image?\tiny \frac{81}{594}" /> | <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{233}{342}" title="https://latex.codecogs.com/svg.image?\tiny \frac{233}{342}" /> |
| male    | 468     | 109     | <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{468}{594}" title="https://latex.codecogs.com/svg.image?\tiny \frac{468}{594}" /> | <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{109}{342}" title="https://latex.codecogs.com/svg.image?\tiny \frac{109}{342}" /> |
| Total   | 549     | 342     | 1 | 1 |


# Percentage of population in the study sector:

The Percentage of Population in the study sector is a measure that indicates the proportion of the total population represented by a particular sector:

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;population_{sector_i}&space;=&space;\frac{&space;population_{sector_i}}{&space;population}" title="https://latex.codecogs.com/svg.image?\small % population_{sector_i} = \frac{ population_{sector_i}}{ population}" />

Let's calculate the percentage of population for the chosen sector, which is the female sector in this case:

<img src="https://latex.codecogs.com/svg.image?\small&space;%&space;population_{female}&space;=&space;\frac{&space;population_{female}}{&space;population&space;}&space;=&space;\frac{81&space;&plus;&space;233}{81&space;&plus;&space;233&space;&plus;&space;468&space;&plus;109}&space;\approx&space;0.352413" title="https://latex.codecogs.com/svg.image?\small % population_{female} = \frac{ population_{female}}{ population } = \frac{81 + 233}{81 + 233 + 468 +109} \approx 0.352413" />

Now, let's examine the table that presents the statistics:

| Sector |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % Population   |
|---------:|---------:|---------:|---------:|---------:|---------:|
|   female | 81 | 233| 0.15 | 0.68| <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{314}{891}" title="https://latex.codecogs.com/svg.image?\tiny \frac{314}{891}" /> |
|     male  | 468 | 109|0.85| 0.32| <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{577}{891}" title="https://latex.codecogs.com/svg.image?\tiny \frac{577}{891}" /> |
|     Total | 549 | 342 | 1 | 1 |

This measure provides us with valuable information about the representation of the study sector within the overall population. Understanding this distribution is crucial for conducting a comprehensive analysis of the results and drawing meaningful conclusions from the data.

# Relative Risk (RR):

The RR (Relative Risk) for sector 'i' can be calculated as the proportion of the sector under study in the target of occurrences in relation to the proportion of sector 'i' in the target of non-occurrences:

<img src="https://latex.codecogs.com/svg.image?\small&space;RR_{sector_i}&space;=&space;\frac{&space;%&space;target_{1,&space;sector_i}}{&space;%&space;target_{0,&space;sector_i}}" title="https://latex.codecogs.com/svg.image?\small RR_{sector_i} = \frac{ % target_{1, sector_i}}{ % target_{0, sector_i}}" />

Likewise, the RR for the female category can be calculated as the percentage of females among survivors compared to the percentage of females among those who died: 

<img src="https://latex.codecogs.com/svg.image?\small&space;RR_{female}&space;=&space;\frac{&space;%&space;survived_{1,&space;female}}{&space;%&space;survived_{0,&space;female&space;}}&space;=&space;\frac{&space;\frac{233}{233&plus;109}}{&space;\frac{81}{81&plus;468}&space;}&space;\approx&space;4.617609" title="https://latex.codecogs.com/svg.image?\small RR_{female} = \frac{ % survived_{1, female}}{ % survived_{0, female }} = \frac{ \frac{233}{233+109}}{ \frac{81}{81+468} } \approx 4.617609" />

| Sector |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" /> |    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % Population   | RR |
|---------:|---------:|---------:|---------:|---------:|---------:| ---------:|
|   female | 81 | 233| 0.15 | 0.68| 0.35| <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{0.68}{0.15}\" title="https://latex.codecogs.com/svg.image?\tiny \frac{0.68}{0.15}" /> |
|     male  | 468 | 109|0.85| 0.32| 0.65| <img src="https://latex.codecogs.com/svg.image?\tiny&space;\frac{0.32}{0.85}" title="https://latex.codecogs.com/svg.image?\tiny \frac{0.32}{0.85}" /> |
|     Total | 549 | 342 | 1 | 1 |1| |

The relative risk is a measure that allows us to assess the variability of categories within an explanatory variable, indicating how discriminative that variable can be.

# Weight of Evidence (WoE):

 It can be calculated using the natural logarithm of the Relative Risk (RR) for each sector:

<img src="https://latex.codecogs.com/svg.image?\small&space;WoE_{sector_i}&space;=&space;ln\left&space;(&space;RR_{sector_i}&space;\right&space;)" title="https://latex.codecogs.com/svg.image?\small WoE_{sector_i} = ln\left ( RR_{sector_i} \right )" />

Let's consider the female sector as an example:

<img src="https://latex.codecogs.com/svg.image?\small&space;WoE_{female}&space;=&space;ln\left&space;(&space;RR_{female}&space;\right&space;)&space;\approx&space;1.529877" title="https://latex.codecogs.com/svg.image?\small WoE_{female} = ln\left ( RR_{female} \right ) \approx 1.529877" />

Now, let's examine the table that presents the statistics:

| Sector |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" /> |    % Population   | RR |    WoE |
|---------:|---------:|---------:|---------:|---------:|---------:| ---------:|---------:|
|   female | 81 | 233| 0.15 | 0.68| 0.35| 4.62|ln(4.62) |
|     male  | 468 | 109|0.85| 0.32| 0.65| 0.37|ln(0.37) |
|     Total | 549 | 342 | 1 | 1 |1| |  |

By analyzing the WoE values, we can gain insights into the discriminative nature of the variables in predicting the desired outcome.

# Information Value (IV):

It can be calculated using the following formula:

<img src="https://latex.codecogs.com/svg.image?\small&space;IV_{sector_i}&space;=&space;WoE_{sector_i}&space;\times&space;(%&space;target_{1,&space;sector_i}&space;-&space;%&space;target_{0,&space;sector_i}&space;)" title="https://latex.codecogs.com/svg.image?\small IV_{sector_i} = WoE_{sector_i} \times (% target_{1, sector_i} - % target_{0, sector_i} )" />

Let's consider the Female sector as an example:

<img src="https://latex.codecogs.com/svg.image?\small&space;IV_{female}&space;=&space;WoE_{female}&space;\times&space;(\%&space;survived_{1,&space;female}&space;-&space;\%&space;survived_{0,&space;female}&space;)&space;&space;=&space;1.529877&space;\times&space;(0.681287&space;-&space;0.147541&space;)&space;\approx&space;0.816566" title="https://latex.codecogs.com/svg.image?\small IV_{female} = WoE_{female} \times (\% survived_{1, female} - \% survived_{0, female} ) = 1.529877 \times (0.681287 - 0.147541 ) \approx 0.816566" />

| Sector |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % Population   | RR |    WoE | IV |
|---------:|---------:|---------:|---------:|---------:|---------:| ---------:|---------:|---------:|
|   female | 81 | 233| 0.15 | 0.68| 0.35| 4.62|1.53| <img src="https://latex.codecogs.com/svg.image?\tiny&space;1.53&space;\times&space;(0.68-0.15&space;)&space;" title="https://latex.codecogs.com/svg.image?\tiny 1.53 \times (0.68-0.15 ) " />|
|     male  | 468 | 109|0.85| 0.32| 0.65| 0.37|-0.98| <img src="https://latex.codecogs.com/svg.image?\tiny&space;-0.98&space;\times&space;(0.32-0.85&space;)" title="https://latex.codecogs.com/svg.image?\tiny -0.98 \times (0.32-0.85 )" />|
|     Total | 549 | 342 | 1 | 1 |1| |  | | |


If you're interested in checking out the IV values classification , you can find it at this [link](https://deborahbarbedo.github.io/pt/2023-05-08-Unpacking_WOE_and_IV).


The table with all the calculated metrics looks as follows:

| Sector |        # <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        # <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % <img src="https://latex.codecogs.com/svg.image?\small&space;target_{0}" title="https://latex.codecogs.com/svg.image?\small target_{0}" />  |        % <img src="https://latex.codecogs.com/svg.image?target_{1}" title="https://latex.codecogs.com/svg.image?target_{1}" />|    % Population  | RR |    WoE | IV |
|---------:|---------:|---------:|---------:|---------:|---------:| ---------:|---------:|---------:|
|   female | 81 | 233| 0.15 | 0.68| 0.35| 4.62|1.53| 0.82|
|     male  | 468 | 109|0.85| 0.32| 0.65| 0.37|-0.98| 0.53|
|     Total | 549 | 342 | 1 | 1 |1| |  |  1.35|


To ensure a more precise comprehension of WoE, IV, and RR, I have curated an informative post that delves into these concepts. You can access it [here](https://deborahbarbedo.github.io/pt/2023-05-08-Unpacking_WOE_and_IV). This article aims to provide a comprehensive explanation, elucidating the intricacies of these metrics.

Moreover, if you find yourself in need of performing these calculations using Python, I have created another post featuring the corresponding formulas, which can be accessed at this [link](https://deborahbarbedo.github.io/pt/2023-04-17-WoE_IV_Python_Function). This resource will empower you to execute the calculations efficiently.

For additional support, I have compiled a variety of supplementary materials on [my GitHub](https://github.com/DeborahBarbedo), specifically related to the topic of this post. These resources, accessible in the [supporting materials](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE) repository, are designed to enhance your comprehension and aid in the practical implementation of IV, WoE, and RR calculations.

If you have any further questions or need more information, I'm here to help!
