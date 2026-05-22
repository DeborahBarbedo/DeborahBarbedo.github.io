---
# Multilingual page pair ID (must match translations of this page and be unique)
lng_pair: 1_WoE_IV_Python_function
title: "Building a Python Function to Calculate WoE and IV"

# Post specific settings
category: Data Analysis
tags: [Python, Data Analysis, Logistic Regression, Feature Selection, Information Value, Weight of Evidence, Predictive Modeling, Titanic Dataset, WoE, IV]
img: ":IV_and_WoE.jpg"
img_alt: "Diagram or code snippet representing WoE and IV calculations in Python"

# Publish date
date: 2023-04-09 20:48:53 +0900

# SEO
meta_description: "Enhance your predictive models using Information Value (IV) and Weight of Evidence (WoE). Learn how to build custom Python functions to automate WoE and IV calculations, evaluate feature importance, and streamline variable selection."

# Features
image_viewer_on: true
image_lazy_loader_on: true

excerpt: "A step-by-step guide to building a consolidated Python function that automates Weight of Evidence (WoE) and Information Value (IV) calculations for both categorical and continuous data."

---

<!-- outline-start -->

Learn how to calculate Weight of Evidence (WoE) and Information Value (IV) in Python for feature selection in logistic regression, using practical examples with the Titanic dataset.

<!-- outline-end -->

**Weight of Evidence (WoE)** and **Information Value (IV)** are widely used metrics in binary classification problems, especially in logistic regression models, credit risk analysis, and scorecards.

These metrics help evaluate:

- the predictive power of a variable;
- the relationship between categories and the target event;
- the quality of transformations applied to variables.

While working on some Python projects, I noticed the lack of simple and reusable functions to calculate WoE and IV in a practical way. For this reason, I decided to develop my own functions for:

- categorical variables;
- continuous variables;
- generating consolidated WoE and IV tables.

In this post, I present these functions using the classic [Titanic dataset from Kaggle](https://www.kaggle.com/competitions/titanic/data).

## What is WoE?

**Weight of Evidence (WoE)** measures the relationship between the distribution of events and non-events within a specific category.

$$
WoE = \ln\left(\frac{\%\,\text{non-event}}{\%\,\text{event}}\right)
$$

Positive values indicate a higher concentration of non-events, while negative values indicate a higher concentration of events.

## What is IV?

**Information Value (IV)** measures the predictive power of a variable.

$$
IV = \sum (\%\,\text{non-event} - \%\,\text{event}) \times WoE
$$

A common interpretation of IV is:

| IV | Interpretation |
|---|---|
| < 0.02 | No predictive power |
| 0.02 – 0.1 | Weak |
| 0.1 – 0.3 | Medium |
| 0.3 – 0.5 | Strong |
| > 0.5 | Very strong |

## WoE and IV for Discrete Variables

I started by developing a function for discrete variables, and from there, I kept refining the approach. 


<script src="https://gist.github.com/DeborahBarbedo/08ed242316fe3b9ed3350460e2a140f3.js"></script>

Next, I used data from the [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) competition to apply the WoE and IV function in Python. The results were quite interesting:

| Survived |        0 |        1 |    Distr |       WoE |       IV | IV_total |
|---------:|---------:|---------:|---------:|----------:|---------:|---------:|
|   female | 0.147541 | 0.681287 | 0.216562 | -1.529877 | 0.816565 | 1.341681 |
|     male | 0.852459 | 0.318713 | 2.674688 |  0.983833 | 0.525116 | 1.341681 |
|        C | 0.136612 | 0.273529 | 0.499442 | -0.694264 | 0.095057 | 0.122728 |
|        Q | 0.085610 | 0.088235 | 0.970249 | -0.030203 | 0.000079 | 0.122728 |
|        S | 0.777778 | 0.638235 | 1.218638 |  0.197734 | 0.027592 | 0.122728 |

The function generated a table that clearly highlights the WoE and IV values for each category, making it easier to understand which variables have a greater capacity to differentiate the event of interest.

If you are working with this same dataset (or any other), I highly recommend testing this approach. It is a very useful tool for data exploration and improving predictive models.


## WoE and IV for Continuous Variables

When it comes to continuous variables, the process becomes a bit more challenging, mainly when defining the best discretization strategy. This is because different problems can require different approaches to binning.

After a few attempts and drafts, I chose to divide the data into deciles, which allows for a more stable analysis of the distribution and, if necessary, subsequent aggregation into quartiles or other groupings.

This choice resulted in a more interpretable and consistent approach for calculating WoE and IV in continuous variables.

<script src="https://gist.github.com/DeborahBarbedo/d9ddd529f9b4359e4a867a649ab9544b.js"></script>

The final function now generates more structured tables, making it easier to analyze the predictive power of each variable range:

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


## Consolidated Function for Continuous and Categorical Variables

After overcoming the initial challenges, I decided to take the implementation a step further and develop a consolidated function capable of calculating WoE and IV for both categorical and continuous variables within a single framework.

This approach simplifies the analysis process, providing a unified view of the predictive power of the variables.

<script src="https://gist.github.com/DeborahBarbedo/bc3597b64ad2fcd54266664c62adbe3f.js"></script>

The final table obtained is shown below:

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

## Interpreting the Results

Looking at the results obtained from the Titanic dataset, we can draw some important conclusions about the predictive power of the variables:

- The **Sex** variable showed an IV greater than 1.3, indicating very high predictive power.
- The **Fare** variable showed an IV close to 0.37, suggesting a strong capacity for discrimination.
- On the other hand, the **Age** variable showed a lower IV (0.09), indicating a weaker predictive contribution when analyzed in isolation.

These metrics are widely used in predictive modeling tasks and can support decisions such as:

- feature selection;
- scorecard building;
- variable transformation for logistic regression;
- identifying non-linear relationships between variables and the event of interest.

## Conclusion

Using WoE and IV improve the interpretability and predictive capability of logistic regression models.

In addition to helping with feature selection, these metrics make it possible to understand how different ranges or categories influence the probability of the event of interest.

The functions developed in this post allow you to:

- calculate WoE and IV for categorical variables;
- automatically discretize continuous variables;
- consolidate the results into a single table for exploratory analysis.

---

## Code and Supplementary Materials

- [Variable Interpretation and Selection with WoE and IV](https://deborahbarbedo.github.io/posts/2023-04-24-Unpacking_WOE_and_IV)
- [How WoE and IV are Calculated](https://deborahbarbedo.github.io/posts/2023-06-05-WoE_IV_Calculation)

The code used in this post is available on GitHub:

- [Supporting Materials on GitHub](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE)

The repository contains the functions developed to calculate WoE and IV, including:

- categorical variables;
- continuous variables;
- consolidated function;
- application examples using the [Titanic dataset](https://www.kaggle.com/competitions/titanic/data).

---

## References

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  [https://CRAN.R-project.org/package=woe](https://CRAN.R-project.org/package=woe)

* Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. [https://CRAN.R-project.org/package=woeBinning](https://CRAN.R-project.org/package=woeBinning)
