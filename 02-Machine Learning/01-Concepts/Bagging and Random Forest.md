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

It makes use of a base estimators which can be a Decision tree, logistic regression, NN etc an the estimators uses all the features fortraining and  prediction

> As opposed to Voting Classifier which uses **different algorithms** on the **same(entire)** dataset


- For [Classification](Classification.md), the final prediction is made by the meta model `BaggingClassifier` using a majority model approach
- For [Regression](Regression.md), the final prediciton is made using `BaggingRegressor` in sci-kit learn by calculating an averagae of the outputs.

### Out of Bag
When using the bagging, some instances of the data will get to be sampled multiple times while others will not be sampled at all.
At average, for each model about 63% of the data is sampled while 37% ain't. The 37% instances are called the **Out of Bag (OOB) instances**.

Often the OOB instances are also trained and evaluate alongside the Bootstrap sample. 
Each model will have it's own OOB Evaluation score $OOB_i$ and the aggregated OOB evalution score is the average of all the individaul scores

$$\text{OOB Score} = \frac{OOB_1+OOB_2+...+OOB_n}{N}$$
```python
bc = BaggingClassifier(base_estimator=DecisionTree(), oob_score=True)
```

**N/B:** In sci-kit learn, the OOB score for classifiers corresponds to accuracy score and $R^2$ for regressors

## Random Forest

It makes prediction by combining multiple trees called a 'forest'. This uses Decision Tree as it's base estimator. It introduces some randomization on the features. When making a split, each tree only considers a random subset of features. 

- For [Classification](Classification.md), the final prediction is made by the meta model `RandomForestClassifier` using a majority model approach
- For [Regression](Regression.md), the final prediciton is made using `RandomForestRegressor` in sci-kit learn by calculating an averagae of the outputs.
- 