---
title: Interpreting Logistic Regression
date:   2018-11-12
tags:
  - python
  - machine learning
author_profile: true
excerpt:
toc: true
mathjax: true
published: false
---

With deep learning taking the data science community by storm, people tend to look down at relatively simple machine learning algorithms. Although deep learning can tackle problems that require complex input-output mappings, these deep learning models lack adequate interpretability. Enter logistic regression, which provides interpretable reasoning for the predictive outputs of this type of statistical model. The purpose of this post is to provide an example application of logistic regression and how to interpret the model outcome.

## Background

### Assumptions

Before logistic regression can be considered a valid algorithm for the data, check these seven assumptions to confirm logistic regression is the best algorithm for the job: 

1. Logistic regression requires the dependent variable to be binary.

2. For binary regression, 1 should represent the desired outcome, and 0 should represent the undesirable outcome.

3. Assume linear relationship between continuous independent variables and logit function.

4. No mulitcollinearity.

* The independent variables can not be correlated with one another, which leads to difficulty interpreting the variance explained in the dependent variable. 

5. No significant outliers.

6. Only meaningful variables should be included

7. Logistic regression requires large sample sizes 

As mentioned above, logistic regression is used to predict dichotomous (binary) variables. Here are a few classification examples: 

* Presence of disease? (yes/no)
* Death? (yes/no)
* Loan default? (yes/no)

Predictions can be derived from simple (one independent variable) or multivariate (two or more independent variables). The following is the data modeling process for the Titanic dataset. 

## Titanic: Machine Learning from Disaster




