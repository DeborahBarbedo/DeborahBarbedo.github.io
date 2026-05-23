---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 2_WoE_IV_Python_Unpacking
title: "Mastering Logistic Regression: Unpacking WoE and IV Metrics for Variable Selection"

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Data Science
# multiple tag entries are possible
tags: [Python, Logistic Regression, Feature Engineering, Feature Selection, Weight of Evidence, Information Value, WoE, IV, Credit Scoring, Titanic Dataset]
# thumbnail image for post
img: ":IV_and_WoE.jpg"
img_alt: "Interpreting Weight of Evidence (WoE) and Information Value (IV) in logistic regression models"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-04-24 20:48:53 +0900

# SEO
meta_description: "Learn how to interpret Weight of Evidence (WoE) and Information Value (IV) in logistic regression models. Discover how to use these metrics for feature selection, predictive analysis, and category binning with practical examples from the Titanic dataset."

excerpt: "An essential guide to interpreting Weight of Evidence (WoE) and Information Value (IV) in logistic regression. Discover how to leverage these metrics for feature selection and optimal category binning using the Titanic dataset."

# Features
image_viewer_on: true
image_lazy_loader_on: true

---

<!-- outline-start -->

Learn how to interpret **Weight of Evidence (WoE)** and **Information Value (IV)** for variable selection, predictive analysis, and logistic regression modeling.

<!-- outline-end -->

Building a good classification model is not just about choosing algorithms, it’s essential to understand how variables behave and how they relate to the event of interest.

That’s where **Weight of Evidence (WoE)** and **Information Value (IV)** come in. These two metrics are widely used in *credit scoring*, variable selection, and predictive modeling.

They help measure a variable’s discriminatory power and provide insights into the direction and strength of the relationship between predictors and the target variable.

In this article, you will learn:

- how to interpret WoE and IV in practice;
- how these metrics support the development of logistic regression models;
- how to use WoE and IV for category grouping (*binning*);
- how to transform variables into more interpretable and predictive features.

To make the concepts more intuitive, we’ll use practical examples based on the dataset from the [Kaggle Titanic competition](https://www.kaggle.com/competitions/titanic/data).



## Metric Overview

In the previous post on [how to calculate WoE and IV in Python](https://deborahbarbedo.github.io/posts/2023-04-09-WoE_IV_Python_Function), we explored how to build the functions responsible for computing these metrics. Now, we’ll focus on how to interpret the results and extract insights for logistic regression models.

These metrics help evaluate how each explanatory variable relates to the response variable.

The main metrics are:

---

### **Class Proportions (`0` and `1`)**  
  Represents the distribution of the target variable within each segment of the analyzed feature. This metric helps you understand how events are distributed across categories.

---  

### **Weight of Evidence (WoE)**  
  Measures the discriminatory power of each category. The farther the WoE value is from zero, the stronger the segment’s ability to distinguish between classes.

  In general:

| WoE | Interpretation |
|:---|:---|
| `WoE > 0` | higher relative concentration of non-events (`target₀`) |
| `WoE ≈ 0` | similar distribution between classes. |
| `WoE < 0` | higher relative concentration of events (`target₁`) |


---  

### **Information Value (IV)**  
  Measures the overall predictive power of the variable. The total IV is obtained by summing the contributions of each segment.

  The table below presents a commonly used classification for interpreting the predictive power of a variable:

| IV | Interpretation |
|:---|:---|
| `IV ≤ 0.02` | Variable with no predictive power |
| `0.02 < IV ≤ 0.10` | Weak predictive power |
| `0.10 < IV ≤ 0.30` | Medium predictive power |
| `0.30 < IV ≤ 0.50` | Strong predictive power |
| `IV > 0.50` | Very strong predictive power (*possible data leakage*) |

Extremely high IV values may indicate information leakage (*data leakage*), especially when the variable has a direct relationship with the target event.

---

It is important to note that **rare categories** may exhibit extreme WoE values but still contribute little to the total IV due to their **low population representativeness**.

These metrics are extremely useful across several modeling stages, especially in **exploratory analysis**, **feature engineering**, **category grouping (*binning*)**, and **variable selection**. Ultimately, by correctly interpreting WoE and IV, it becomes easier to enhance the **interpretability of logistic regression models** and to build models that are more robust and better aligned with the underlying data behavior.


---




## Creating Variables Through Category Grouping (*Binning*)

Category grouping (*binning*) is a widely used strategy for creating variables in predictive models, especially in logistic regression and credit scoring.

The idea is to combine categories or intervals that exhibit similar behavior with respect to the target variable, using metrics such as **Weight of Evidence (WoE)** and **Information Value (IV)** to support the analysis.

By adopting this approach, the model becomes more robust. This process simplifies variables and reduces data dimensionality, which consequently *reduces the risk of overfitting* and increases statistical stability. In addition, binning improves model interpretability and helps establish more linear relationships between explanatory variables and the logistic regression logit.

However, some precautions are important during the grouping process:

- grouped categories should exhibit similar behavior;
- segments with very different WoE values should not be combined;
- excessive grouping may significantly reduce the variable’s predictive power;
- very rare categories may produce unstable WoE values.

This manipulation introduces a well‑known trade‑off: when categories are grouped, a natural reduction in **Information Value (IV)** occurs, since part of the variable’s original discriminatory power is smoothed out. Therefore, the main challenge in binning is to find the balance between model simplification, statistical stability, and preservation of relevant information. When done correctly, category grouping can significantly improve the robustness and generalization ability of the predictive model.



## Practical Application

Now let’s put into practice the concepts of **Weight of Evidence (WoE)** and **Information Value (IV)** using the dataset from the [Titanic - Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic/data) competition.

Our goal will be to interpret the metrics, identify predictive variables, and understand how to use WoE and IV in the creation of new variables for logistic regression models.

---

### Example with a Discrete Variable

When we apply the [Woe_IV_Discrete](https://gist.github.com/DeborahBarbedo/08ed242316fe3b9ed3350460e2a140f3) function to the `Sex` variable, we obtain the following table:


| Sex    | `target₀` | `target₁` | `Distr`   | `WoE`      | `IV`      | `IV_total` |
|:-------|-----------:|-----------:|-----------:|-----------:|-----------:|------------:|
| female | 0.147541   | 0.681287   | 0.216562   | -1.529877  | 0.816565   | 1.341681    |
| male   | 0.852459   | 0.318713   | 2.674688   | 0.983833   | 0.525116   | 1.341681    |

The interpretation of these results is quite intuitive:

- the negative `WoE` value for `female` indicates a stronger association with survival (`target₁`);
- the positive `WoE` value for `male` indicates a stronger association with non‑survival (`target₀`);
- the `IV_total = 1.34` indicates an extremely high discriminatory power for the `Sex` variable.

In credit scoring problems, variables with very high IV values typically require additional attention, as they may indicate excessive class separation or potential information leakage.

---

### Example with a Continuous Variable

In addition to categorical variables, [WoE and IV can also be applied to continuous variables](https://gist.github.com/DeborahBarbedo/d9ddd529f9b4359e4a867a649ab9544b) after a discretization (*binning*) process.

| Variable | Interval | `target₀` | `target₁` | `Distr` | `WoE` | `IV` |
|:---|:---|---:|---:|---:|---:|---:|
| Fare | `<= 7.55` | 0.143898 | 0.038012 | 3.785624 | 1.331211 | 0.14 |
| Fare | `7.55 – 7.8542` | 0.111111 | 0.076023 | 1.461538 | 0.379490 | 0.01 |
| Fare | `7.8542 – 8.05` | 0.158470 | 0.055556 | 2.852459 | 1.048181 | 0.11 |
| Fare | `8.05 – 10.5` | 0.109290 | 0.052632 | 2.076503 | 0.730685 | 0.04 |
| Fare | `10.5 – 14.4542` | 0.087432 | 0.105263 | 0.830601 | -0.185606 | 0.00 |
| Fare | `14.4542 – 21.6792` | 0.092896 | 0.108187 | 0.858662 | -0.152380 | 0.00 |
| Fare | `21.6792 – 27` | 0.078324 | 0.134503 | 0.582324 | -0.540729 | 0.03 |
| Fare | `27 – 39.6875` | 0.103825 | 0.099415 | 1.044359 | 0.043403 | 0.00 |
| Fare | `39.6875 – 77.9583` | 0.076503 | 0.137427 | 0.556679 | -0.585766 | 0.04 |
| **Total** | — | **1.000000** | **1.000000** | **1.000000** | **0.000000** | **0.37** |

The `Fare` variable also shows strong predictive power (`IV = 0.37`).

In addition, the WoE analysis allows us to identify ranges with similar behavior, making it possible to group categories (*binning*).

Observe that:

- ranges with `WoE > 0` tend to be more associated with non‑survival;
- ranges with `WoE < 0` tend to be more associated with survival;
- WoE values close to zero indicate neutral behavior.

Based on these results, we can create a binary variable indicating, for example, whether the fare is less than or equal to `10.5`.

---

### Creating New Variables

Based on the WoE and IV analysis, we can construct derived variables that make the model simpler and more interpretable.

Example:

| PassengerId | Survived | Sex    | Fare   | `FLG_female` | `FLG_Fare_leq_10.5` |
|------------:|---------:|:-------|-------:|-------------:|---------------------:|
| 1           | 0        | male   | 7.2500 | 0            | 1                    |
| 2           | 1        | female | 71.2833| 1            | 0                    |
| 3           | 1        | female | 7.9250 | 1            | 1                    |
| ...         | ...      | ...    | ...    | ...          | ...                  |
| 891         | 0        | male   | 7.7500 | 0            | 1                    |

In this scenario, we create binary variables (*flags*) to simplify the information:

- `FLG_female` identifies female passengers;
- `FLG_Fare_leq_10.5` identifies fares less than or equal to `10.5`.

This type of transformation enhances **model interpretability** and ensures greater **statistical stability**. In addition, by reducing noise from the original data, the approach improves the model’s **generalization** and overall **predictive performance**.

---

## Conclusion

The **Weight of Evidence (WoE)** and **Information Value (IV)** metrics are extremely useful tools for variable selection, exploratory analysis, category grouping (*binning*), creation of derived variables, and interpretation of logistic regression models.

In addition to contributing to more interpretable and robust models, WoE and IV make it possible to understand how variables behave across different population segments, supporting both attribute selection and feature engineering for binary classification problems.

---

## Additional Resources

- [Supporting materials on GitHub](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE)
- [Function for calculating WoE and IV in Python](https://deborahbarbedo.github.io/posts/2023-04-09-WoE_IV_Python_Function)
- [How WoE and IV are calculated](https://deborahbarbedo.github.io/posts/2023-06-05-WoE_IV_Calculation)


## References:

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  [https://CRAN.R-project.org/package=woe](https://CRAN.R-project.org/package=woe)

* Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. [https://CRAN.R-project.org/package=woeBinning](https://CRAN.R-project.org/package=woeBinning)
