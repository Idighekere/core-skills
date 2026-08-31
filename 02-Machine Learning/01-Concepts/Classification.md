---
title: "Classification"
date: 2026-08-31
type: concept
tags: [ml, concept/classification]
---

# Classification

Predicting a **category**, not a number. Binary classification: two categories. Multi-class: three or more. The metrics below assume binary unless noted; multi-class is a per-class average of the same ideas.

## The confusion matrix

The source of every classification metric. For binary classification with positive class $1$ and negative class $0$:

| | predicted 0 | predicted 1 |
|---|---|---|
| **actual 0** | TN (true negative) | FP (false positive) |
| **actual 1** | FN (false negative) | TP (true positive) |

- **TP** = correctly predicted positives.
- **FP** = wrongly predicted positives (type I error — false alarm).
- **FN** = wrongly predicted negatives (type II error — missed positive).
- **TN** = correctly predicted negatives.

## The metrics

**Accuracy** — the fraction of all predictions that are correct:

$$\text{accuracy} = \frac{TP + TN}{TP + TN + FP + FN}$$

Trap: on imbalanced data (99% class 0), predicting 0 every time gives 99% accuracy and 0 information. Accuracy is the default only when the classes are roughly balanced.

**Precision** — of all the positives you predicted, how many were right:

$$\text{precision} = \frac{TP}{TP + FP}$$

Use when **false positives are expensive** (spam filter — don't want to bury real email).

**Recall** (sensitivity, true positive rate) — of all the actual positives, how many did you catch:

$$\text{recall} = \frac{TP}{TP + FN}$$

Use when **false negatives are expensive** (cancer screening — don't want to miss a tumour).

**F1** — the harmonic mean of precision and recall:

$$F_1 = 2 \cdot \frac{\text{precision} \cdot \text{recall}}{\text{precision} + \text{recall}}$$

Single-number summary of precision and recall. Use it when you care about both and want one score to optimise.

**ROC-AUC** — area under the Receiver Operating Characteristic curve. Plots recall vs. false-positive-rate as you sweep the decision threshold. AUC = 1.0 is perfect, 0.5 is random.

$$\text{AUC} = P(\hat{s}(\text{positive}) > \hat{s}(\text{negative}))$$

The probability that a random positive is ranked above a random negative. Threshold-independent — useful when you'll move the threshold later.

## Which to report

| Situation | Report |
|---|---|
| Balanced classes, single decision | accuracy |
| Imbalanced, false positives matter | precision |
| Imbalanced, false negatives matter | recall |
| Imbalanced, both matter | F1 |
| Probabilistic output, threshold will move | ROC-AUC |
| Probabilistic output, calibration matters | log loss |

## Threshold tuning

[Logistic Regression](Logistic%20Regression.md) and similar models output a probability $\hat{p} \in (0, 1)$. The default 0.5 threshold is rarely the right one for the metric you care about.

Move it:
- **Up** (e.g. 0.7): fewer positives predicted, precision up, recall down.
- **Down** (e.g. 0.3): more positives predicted, recall up, precision down.

```python
from sklearn.metrics import precision_recall_curve
prec, rec, thr = precision_recall_curve(y_test, probs)
# plot prec vs rec, pick threshold where the curve gives the trade-off you want
```

For imbalanced classes, plot precision and recall against the threshold and pick the operating point that fits the cost of each error.

## Class imbalance

Three things to do, in order of effort:

1. **Don't touch the data, change the metric.** F1 or ROC-AUC instead of accuracy.
2. **Resample.** Undersample the majority class, oversample the minority, or use SMOTE to synthesise minority samples.
3. **Reweight the loss.** In `LogisticRegression`, pass `class_weight="balanced"`. The model now pays a higher price for minority-class mistakes.

```python
LogisticRegression(class_weight="balanced")
```

The right answer depends on the data and the cost. There's no universal rule.

## Code

```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score,
    f1_score, roc_auc_score, confusion_matrix, classification_report,
)

y_pred = clf.predict(X_test)
y_prob = clf.predict_proba(X_test)[:, 1]

print(accuracy_score(y_test, y_pred))
print(precision_score(y_test, y_pred))
print(recall_score(y_test, y_pred))
print(f1_score(y_test, y_pred))
print(roc_auc_score(y_test, y_prob))
print(classification_report(y_test, y_pred))  # all of the above at once
```

## Pitfalls

- **Reporting accuracy on imbalanced data.** Predict the majority class, hit 99%, learn nothing.
- **Picking the threshold from the test set.** Optimises a single point on the curve to your specific test split. Use a validation set or cross-validation to pick the threshold.
- **Comparing models with F1 from different thresholds.** Two F1 scores are only comparable at the same operating point. Report precision + recall + threshold, or use ROC-AUC.
- **Conflating probability with class.** A model that says "60% positive" is not predicting class 1 — it's predicting $P(y=1) = 0.6$. Whether to *act* on that depends on the cost.

## Related

- [Logistic Regression](Logistic%20Regression.md) — produces the probabilities
- [Support Vector Machines](Support%20Vector%20Machines.md) — needs calibration for probabilities
- [Linear Classifiers](Linear%20Classifiers.md) — index
- [The Bias-Variance Tradeoff](The%20Bias-Variance%20Tradeoff.md) — precision/recall trade-off is one face of it
- [Fine-Tuning Models](Fine-Tuning%20Models.md) — picking the threshold with cross-validation