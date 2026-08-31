---
title: Bagging and Random Forest
date: 2026-08-31
type: concept
tags:
  - ml
  - concept/tree-based-models
---

# Bagging and Random Forest

## Bootstrap Aggregation (BAgging )
Bagging is an [*ensemble*](Ensemble%20Learning.md) method that involves training **same algorithms** multiple times on **different subsets** sampled from the training data.

> As opposed to Voting Classifier which uses **different algorithms** on the **same(entire)** dataset

### Outputs
In [Classification](Classification.md), the final prediction is made by the meta model `BaggingClassifier` using a majority model approach
In [Regression](Regression.md), the final prediciton is made using `BaggingRegressor` in sci-kit learn by calculating an averagae of the outputs.

### Out of Bag
When using the bagging, some instances of the data will get to be sampled multiple times while others will not be sampled at all.
At average, for each model about 63% of the data is sampled while 37% ain't. The 37% instances are called the **Out of Bag (OOB) instances**.

Often the OOB instances are also trained and evaluate alongside the Bootstrap sample. 
Each model will have it's own OOB Evaluation score $OOB_i$ and the aggregated OOB evalution score is the average of all the individaul scores

$$\text{OOB Score} = \frac{OOB_1+OOB_2+...+OOB_n}{N}$$
