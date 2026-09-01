---
title: Boosting
date: 2026-08-31
type: concept
tags:
  - ml
  - concept/ensemble
---

# Boosting
Boosting is an [*ensemble*](Ensemble%20Learning.md) method where **several** models are trained **sequentially** with the eahc model learning from the error of it's previous.
The predictors tries to correct the errors made by its predecessor

This method combines weak learners with strong learners to minimize errors

## AdaBoost (Adaptive Boosting)
As the name implies, the predictors tries to adapt to the results from its predecessor, by focusing more on the wrong instances.. They constantly change the weight of the instances. 

This process is repeated sequentially 
**Learning rate:** $0 \lt \eta \le 1$


![](Pasted%20image%2020260901230820.png)

Based on the above diagrams, we can see how $\alpha_1$ is used to update the weight for the next predictor, so Predictor 2, learnt from Predictor 1's errors and updated it's weight based on the error it learnt.
