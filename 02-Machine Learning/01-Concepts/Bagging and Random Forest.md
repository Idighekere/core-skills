---
title: Bagging and Random Forest
date: 2026-08-31
type: concept
tags:
  - ml
  - concept/tree-based-models
---

# Bagging and Random Forest

## Bagging or Bootstrap Aggregation
Bagging is an [*ensemble*](Ensemble%20Learning.md) method that involves training **same algorithms** multiple times on **different subsets** sampled from the training data.

> As opposed to Voting Classifier which uses **different algorithms** on the **same(entire)** dataset

### Outputs
In [Classification](Classification.md), the final prediction is made by the meta model `BaggingClassifier` using a majority model approach
In [Regression](Regression.md), the final prediciton is made using `BaggingRegressor` in sci-kit learn by calculating an averagae of the outputs.


