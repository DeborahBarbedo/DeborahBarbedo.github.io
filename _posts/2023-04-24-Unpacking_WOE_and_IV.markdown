---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 2_WoE_IV_Python_Unpacking
title: "Mastering Logistic Regression: Unpacking WOE and IV Metrics for Variable Selection and Interpretation."

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Python
# multiple tag entries are possible
tags: [Logistic Regression, variable's analysis, metrics ]
# thumbnail image for post
img: ":IV_and_WoE _Unpacking.jpg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-04-24 20:48:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Learn how to master logistic regression with the essential WOE and IV metrics for variable selection and interpretation. Discover how to predict responses and interpret variable skewing with this informative guide, including practical tips and insights from our expert."

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

# Unpacking WOE and IV Metrics for Variable Selection and Interpretation.

<!-- outline-end -->

Ever heard of WOE and IV? They stand for "Weight of Evidence" and "Value of Information" respectively, and are essential for building, selecting, and interpreting variables for logistic regression models. These metrics help you determine how well a variable predicts the response you're looking for, as well as which way the variable is skewing the response.

## Metrics Presentation

Remember the previous post on how to create [WoE and IV functions in Python](https://deborahbarbedo.github.io/posts/2023-04-09-WoE_IV_Python_Function)? Now I'll show you how to interpret the table and extract even more insights!

The table presents several metrics that are useful in evaluating the relationship between the study variable and the occurrence of negative or positive outcomes. Some of these metrics include:
-	**Proportion of targets 0 or 1** for each sector of the study variable, which helps to understand the variable's distribution in relation to outcomes.
-	**Relative risk**, an important indicator that shows the association between the variable and the target. The more variable the RR between the categories of the explanatory variable, the more useful it is. A RR close to 1 indicates little association with the target, a RR > 1 indicates a positive association, and a RR < 1 indicates a negative association.
-	**WoE**, a useful metric for assessing the variable's discrimination. The further away from 0 the WoE is, the more discriminating the variable is. A negative WoE indicates that the variable does not favor the occurrence of the target, while a positive WoE indicates that the variable favors the occurrence.
-	**IV**, an Information Value that helps assess the predictivity of the variables. It is important to note that if a sector of the variable indicates a strong association with the target but appears infrequently in the population, its IV will not be high. The Total Information Value of a variable is the sum of the IVs for each sector studied.


Additionally, there is a table indicating the classification of the IV values:

| IV        | Classification            |
|-----------|---------------------------|
| ≤ 0.02   | Not useful for prediction |
| 0.02 -0.1 | Weak predictive power     |
| 0.1 - 0.3 | Moderate predictive power |
| 0.3 - 0.5 | Strong predictive power   |
| \> 0.5     | Suspect predictive power  |


If you're interested in checking out the original table, you can find it at this [link](https://teses.usp.br/teses/disponiveis/45/45134/tde-05022015-232801/pt-br.php).

These metrics enable you to gain an even clearer view of your data!


## Grouping Categories

Grouping categories is an essential step in variable preparation for predictive models. It involves analyzing the similarity in the discrimination of targets and evaluating representative cases in each attribute, resulting in categories grouped in a way that makes sense. Clustering categories based on IV and WoE analysis has several benefits, such as simplifying the equation, reducing the risk of overfitting, and making variables more suitable for the model. However, it's important to keep in mind that the information value always decreases when categories are grouped, and just categories with similar WoE should be combined to avoid losing important information.


## Let's dive right in!

When we apply the [Woe_IV_Dis function](https://gist.github.com/DeborahBarbedo/08ed242316fe3b9ed3350460e2a140f3) to the "Sex" variable in the [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) competition data, we get some interesting insights.

| Survived |        0 |        1 |       RR |       WoE |       IV | IV_total |
|---------:|---------:|---------:|---------:|----------:|---------:|---------:|
|      Sex |          |          |          |           |          |          |
|   female | 0.147541 | 0.681287 | 4.617609 |  1.529877 | 0.816565 | 1.341681 |
|     male | 0.852459 | 0.318713 | 0.373875 | -0.983833 | 0.525116 | 1.341681 |


 The Relative Risk suggests that being female is a positive factor for survival, with female passengers having a four times higher chance of survival compared to male passengers. The WoE metric confirms this relationship, showing that being female favors survival. The IV metric indicates that the sex variable is strongly related to the response variable, which suggests a high predictive power.

Now, let's take a look at the table generated by the [Woe_IV_cont function](https://gist.github.com/DeborahBarbedo/d9ddd529f9b4359e4a867a649ab9544b) for the "Fare" variable.


|   | variable |              limit |        0 |        1 |       RR |       WoE |   IV |
|--:|---------:|-------------------:|---------:|---------:|---------:|----------:|-----:|
| 0 |     Fare |             ≤ 7.55 | 0.143898 | 0.038012 | 0.264157 | -1.331211 | 0.14 |
| 1 |     Fare |     7.55 to 7.8542 | 0.111111 | 0.076023 | 0.684211 | -0.379490 | 0.01 |
| 2 |     Fare |     7.8542 to 8.05 | 0.158470 | 0.055556 | 0.350575 | -1.048181 | 0.11 |
| 3 |     Fare |       8.05 to 10.5 | 0.109290 | 0.052632 | 0.481579 | -0.730685 | 0.04 |
| 4 |     Fare |    10.5 to 14.4542 | 0.087432 | 0.105263 | 1.203947 |  0.185606 | 0.00 |
| 5 |     Fare | 14.4542 to 21.6792 | 0.092896 | 0.108187 | 1.164603 |  0.152380 | 0.00 |
| 6 |     Fare |     21.6792 to 27. | 0.078324 | 0.134503 | 1.717258 |  0.540729 | 0.03 |
| 7 |     Fare |     27. to 39.6875 | 0.103825 | 0.099415 | 0.957525 | -0.043403 | 0.00 |
| 8 |     Fare | 39.6875 to 77.9583 | 0.076503 | 0.137427 | 1.796366 |  0.585766 | 0.04 |
| 9 |     Fare |                    | 1.000000 | 1.000000 | 1.000000 |  0.000000 | 0.37 |

This variable also has a high IV, indicating strong predictive power. To improve the model, it is recommended to create a binary variable indicating whether the value is less than or equal to 10.5. When grouping categories, it's important to keep in mind that the value of information tends to decrease. Therefore, it's best to group only categories with similar WoE. In the case of the "Fare" variable, row 7 has a negative WoE close to zero, which suggests a neutral range with respect to survival. Thus, including it in the group favoring survival is safe.

In summary, analyzing the WoE, IV, and other metrics is crucial in identifying predictive variables and grouping categories effectively to improve model performance. I'm glad that the explanation on how to interpret the metrics in the table has provided you with the necessary knowledge to create, interpret, and select variables for the logistic regression model.


Just a quick heads up that you can find all the [support materials](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE) on my Github page. And in case you missed it, my previous post introduced the [function to calculate WoE and IV in Python](https://deborahbarbedo.github.io/posts/2023-04-09-WoE_IV_Python_Function). But guess what? I'm planning to go even deeper in a future post and really unpack how these metrics are calculated. So stay tuned! And if you have any questions, don't hesitate to hit me up. I'm always here and happy to help in any way I can.

