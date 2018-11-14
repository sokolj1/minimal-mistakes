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

With deep learning taking the data science community by storm, people tend to look down at relatively simple machine learning algorithms. Although deep learning can tackle problems that require complex input-output mappings, these models lack adequate interpretability. Enter logistic regression, which provides interpretable reasoning for the predictive outputs of this type of statistical model. The purpose of this post is to shed light on exactly how to interpret logistic regression outputs. 


## Background

### Assumptions

Before logistic regression can be considered a valid statistics test, several test assumptions must be confirmed: 

* Assume linear relationship between independent variables and logit function

* No mulitcollinearity 

* No significant outliers

Logistic regression is used to predict dichotomous (binary) variables. Here are a few classification examples: 

* Presence of disease? (yes/no)
* Death? (yes/no)
* Loan default? (yes/no)

Predictions can be derived from simple (one independent variable) and multivariate (two or more independent variables).


