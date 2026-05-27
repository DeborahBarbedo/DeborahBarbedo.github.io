---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: 3_WoE_IV_Python_calculation
title: "How to Calculate WoE and IV: A Complete Guide for Logistic Regression"

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
#author: "Deborah Cholodoysky Barbedo Pereira"
# multiple category is not supported
category: Data Science
# multiple tag entries are possible
tags: [Python, Logistic Regression, Feature Engineering, Variable Selection, Weight of Evidence, Information Value, WoE, IV, Titanic Dataset, Credit Scoring]
# thumbnail image for post
img: ":IV_and_WoE.jpg"
img_alt: "Practical example of calculating WoE and IV in Python using the Titanic Dataset for credit scoring and logistic regression models"
# disable comments on this page
#comments_disable: true

# publish date
date: 2023-06-05 11:48:53 +0900

# seo
# if not specified, date will be used.
#meta_modify_date: 2021-08-10 11:32:53 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: "Learn how to calculate Weight of Evidence (WoE) and Information Value (IV) in Python using the Titanic dataset. Understand how these metrics assist in variable selection for logistic regression and credit scoring models."
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

# post excerpt (used for previews on index/home pages)
excerpt: "Master the concepts of Weight of Evidence (WoE) and Information Value (IV). Learn how to calculate both step by step in Python using the Titanic dataset to enhance your feature engineering and credit scoring models."

---

<!-- outline-start -->

**Weight of Evidence (WoE)** and **Information Value (IV)** are metrics widely used in credit scoring, variable selection, and logistic regression modeling.

These techniques are especially popular in the financial industry because they allow us to measure the discriminatory power of a variable in relation to a target event.

<!-- outline-end -->

Traditionally, records are split into:

- **good** - paying customers (non-defaulters)
- **bad** - defaulting customers

In this article, we will use the dataset from [Kaggle's Titanic competition](https://www.kaggle.com/competitions/titanic/data) to demonstrate, step by step, how to calculate WoE and IV in a visual and intuitive way.

Our goal is to turn concepts that are often seen as abstract into something intuitive, interpretable, and easily applicable in practice.

## Dataset Used

To demonstrate the calculation of **Weight of Evidence (WoE)** and **Information Value (IV)**, we will use the **gender** variable (`female` and `male`) from the Titanic dataset in relation to passenger survival.

The table below shows the distribution of the target variable:

| Segment   | Did Not Survive (`target₀`) | Survived (`target₁`) |
|:----------|----------------------------:|---------------------:|
| female    | 81                          | 233                  |
| male      | 468                         | 109                  |
| **Total** | **549** | **342** |

Where:

- `target₀` represents passengers who **did not survive**
- `target₁` represents passengers who **survived**

## Percentage of Each Class per Segment

The percentage of a class within a segment corresponds to the proportion of that group relative to the total of that respective class. These proportions represent the **distribution of non-events** (`target₀`) and **events** (`target₁`) across each segment.

For the `target₀` class:

$$
\%target_{0,segment_i}
=
\frac{target_{0,segment_i}}{target₀}
$$

Looking at the **female** segment:

$$
\%survived_{0,female}
=
\frac{81}{81 + 468}
=
\frac{81}{549}
\approx 0.1475
$$

In other words, approximately **14.75%** of the passengers who did not survive were female.

---

For the `target₁` class:

$$
\%target_{1,segment_i}
=
\frac{target_{1,segment_i}}{target₁}
$$

In the **female** segment:

$$
\%survived_{1,female}
=
\frac{233}{233 + 109}
=
\frac{233}{342}
\approx 0.6813
$$

Thus, approximately **68.13%** of the surviving passengers were female.

---

### Percentage Distribution per Segment

| Segment   | `target₀` | `target₁` | % `target₀`  | % `target₁`  |
|:----------|-----------:|-----------:|-------------:|-------------:|
| female    | 81         | 233        | 0.1475       | 0.6813       |
| male      | 468        | 109        | 0.8525       | 0.3187       |
| **Total** | **549** | **342** | **1.0** | **1.0** |

Note that the sum of the proportions for each class equals 1:

$$
\sum \%target₀ = 1
\quad\text{and}\quad
\sum \%target₁ = 1
$$

These distributions will be used later in the calculation of both **Weight of Evidence (WoE)** and **Information Value (IV)**.

## Population Percentage per Segment

The population percentage represents the share of each segment relative to the total sample. This metric will be used later when calculating the **ratio between distributions**, which is an essential component of WoE.

This metric is calculated as follows:

$$
\%population_{segment_i}
=
\frac{population_{segment_i}}{population}
$$

For the **female** segment:

$$
\%population_{female}
=
\frac{81 + 233}{81 + 233 + 468 + 109}
=
\frac{314}{891}
\approx 0.3524
$$

Therefore, females represent approximately **35.24%** of the analyzed population, meaning roughly 35 out of every 100 passengers.

---

### Population Distribution per Segment

| Segment   | Population | % Population |
|:----------|-----------:|-------------:|
| female    | 314        | 0.3524       |
| male      | 577        | 0.6476       |
| **Total** | **891** | **1.0** |

This distribution helps clarify the representativeness of each segment within the database, which is useful when interpreting both **Weight of Evidence (WoE)** and **Information Value (IV)**.

It will be compared against the distributions of `target₀` and `target₁` to identify discrepancies between the overall population and the behavior of specific classes.

## Distribution Ratio

The distribution ratio compares the share of a segment between the `target₀` and `target₁` classes.

It is defined as:

$$
Distr_{segment_i}
=
\frac{
\%target_{0,segment_i}
}{
\%target_{1,segment_i}
}
$$

Values less than 1 indicate that the segment is proportionally more associated with the event (`target₁`), while values greater than 1 indicate a stronger association with the non-event (`target₀`).

| Distribution Ratio | Interpretation |
|:---|:---|
| Distr < 1 | The segment is **underrepresented** among non-events (`target₀`) |
| Distr > 1 | The segment is **overrepresented** among non-events (`target₀`) |

For the **female** segment:

$$
Distr_{female}
=
\frac{0.1475}{0.6813}
\approx 0.2166
$$

This indicates that the proportion of women among passengers who did not survive is much lower than among passengers who did survive. In other words, the `female` segment is more strongly associated with the event (`target₁`).

---

### Distribution Ratio per Segment

| Segment   | % `target₀` | % `target₁` | `Distr`  |
|:----------|------------:|------------:|---------:|
| female    | 0.1475      | 0.6813      | 0.2166   |
| male      | 0.8525      | 0.3187      | 2.6745   |
| **Total** | **1.0** | **1.0** | —        |

Values:

- Less than `1` indicate a higher relative concentration in `target₁`;
- Greater than `1` indicate a higher relative concentration in `target₀`.

## Weight of Evidence (WoE)

**Weight of Evidence (WoE)** is obtained by applying the natural logarithm to the distribution ratio. It quantifies the evidence of whether a segment is more strongly associated with the event (`target₁`) or the non-event (`target₀`).

The use of the logarithm makes the scale symmetric around zero, which simplifies interpretation and improves stability in linear models such as logistic regression.

Its definition is given by:

$$
WoE_{segment_i}
=
\ln\left(
Distr_{segment_i}
\right)
$$

Substituting the definition of `Distr`:

$$
WoE_{segment_i}
=
\ln\left(
\frac{
\%target_{0,segment_i}
}{
\%target_{1,segment_i}
}
\right)
$$

For the **female** segment:

$$
WoE_{female}
=
\ln(0.2166)
\approx -1.5299
$$

The negative value indicates that the **female** segment has a higher relative concentration in `target₁` than in `target₀`, meaning there is **evidence in favor of the event**.

---

### Weight of Evidence per Segment

| Segment   | `Distr`  | `WoE`    |
|:----------|---------:|---------:|
| female    | 0.2166   | -1.5299  |
| male      | 2.6745   | 0.9834   |
| **Total** | —        | —        |

Interpretation:

- `WoE < 0`: Higher relative concentration in `target₁`;
- `WoE > 0`: Higher relative concentration in `target₀`;
- `WoE ≈ 0`: Similar distribution between both classes.

## Information Value (IV)

**Information Value (IV)** measures the predictive power of a variable relative to the target variable.
It combines the intensity of the evidence (WoE) with the magnitude of the difference between the distributions, capturing both the strength and the statistical relevance of the segment.

The contribution of each segment to the IV is calculated as follows:

$$
IV_{segment_i}
=
WoE_{segment_i}
\times
\left(
\%target_{0,segment_i}
-
\%target_{1,segment_i}
\right)
$$

Substituting the definition of `WoE`:

$$
IV_{segment_i}
=
\ln\left(
\frac{
\%target_{0,segment_i}
}{
\%target_{1,segment_i}
}
\right)
\times
\left(
\%target_{0,segment_i}
-
\%target_{1,segment_i}
\right)
$$

For the **female** segment:

$$
IV_{female}
=
-1.5299
\times
(0.1475 - 0.6813)
\approx 0.8166
$$

---

### IV Contribution per Segment

| Segment     | `WoE`   | IV      |
|:------------|--------:|--------:|
| female      | -1.5299 | 0.8166  |
| male        | 0.9834  | 0.5244  |
| **Total IV**| —       | **1.3410** |

The **total Information Value** of the variable is obtained by summing the contributions of all segments:

$$
IV
=
\sum_i IV_{segment_i}
$$

Because IV is additive, variables with many segments can artificially inflate the total value.

In general:

- `IV < 0.02`: No predictive power;
- `0.02 ≤ IV < 0.1`: Weak predictive power;
- `0.1 ≤ IV < 0.3`: Medium predictive power;
- `0.3 ≤ IV < 0.5`: Strong predictive power;
- `IV ≥ 0.5`: Very strong predictive power.

In the example presented, the **gender** variable has an IV of **1.3410**, indicating an extremely high discriminatory power. Values this high can even suggest potential *data leakage* in real-world scenarios.

## Important Considerations

When utilizing **Weight of Evidence (WoE)** and **Information Value (IV)**, several precautions should be kept in mind:

- Low-frequency categories can generate unstable values;
- Division-by-zero issues must be properly handled;
- Variables with excessively high IV may indicate *data leakage*;
- WoE is widely used in logistic regression because it promotes more linear relationships between variables and the logit;
- Binning processes can significantly impact both WoE and IV values.

Although WoE was originally designed for logistic regression, it can also be useful in tree-based models by reducing cardinality and noise.

---

## Additional Resources

If you would like to deepen your knowledge of WoE and IV:

- [Understanding WoE and IV](https://deborahbarbedo.github.io/posts/2023-04-24-Unpacking_WOE_and_IV)
- [Python Functions for Calculating WoE and IV](https://deborahbarbedo.github.io/posts/2023-04-09-WoE_IV_Python_Function)
- [Supplementary Materials on GitHub](https://github.com/DeborahBarbedo/Supporting_materials/tree/main/IV_WoE)


## References:

* Anderson, Raymond. The Credit Scoring Toolkit: Theory and Practice for Retail Credit Risk Management and Decision Automation. Oxford University Press, 2007.

* Siddiqi, Naeem. Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring. Wiley, 2006.

* Sudarson Mothilal Thoppay (2015). woe: Computes Weight of Evidence and Information Values. R package version 0.2.
  [https://CRAN.R-project.org/package=woe](https://CRAN.R-project.org/package=woe)

* Thilo Eichenberg (2018). woeBinning: Supervised Weight of Evidence Binning of Numeric Variables and Factors. R package
  version 0.1.6. [https://CRAN.R-project.org/package=woeBinning](https://CRAN.R-project.org/package=woeBinning)