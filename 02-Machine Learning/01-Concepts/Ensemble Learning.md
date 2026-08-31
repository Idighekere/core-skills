---
title: Ensemble Methods
date: 2026-08-31
type: concept
tags:
  - ml
  - concept/ensemble
---

# Ensemble Learning
- Train **different** model on **same** dataset
- A meta-model is used to aggregate the prediction of the models (e.g Voting Classifier)
- Then the bettter final ensemble prediction is made.

![](Visual%20Explanation%20of%20Ensemble%20Learning.png)

### Essemble Learning in Action
An example of ensemble learning is when  a dataset is trained on different classififcation models like Decision Tree, KNN and Logistic Regression
For each data point in the dataset, a meta model is used to pick the best prediction output based on all the classification resuts of that data point. 

#### Hard Voting
In hard voting, the majority carries the vote, If the Decision Tree and KNN outputs a `1` for a data point while Logistic Regression outputs a `0`, the Voting Classifier by using hard voting picks `1` as the ensemble prediction