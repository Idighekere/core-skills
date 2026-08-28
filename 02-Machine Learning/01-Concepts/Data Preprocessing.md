---
title: "Data Preprocessing"
date: 2026-08-28
type: concept
tags: [ml, concept/preprocessing]
---

# Data Preprocessing

## Why it exists
Models eat numbers. Real datasets are born messy: **missing values, text categories, wildly different scales**. Garbage in, garbage out — preprocessing is the step that converts raw data into the clean, numeric matrix a model can learn from. It's routinely the *largest* chunk of real ML work.

## The pipeline (in order)

### 1. Load and inspect
```python
import pandas as pd
df = pd.read_csv("Data.csv")
df.info()     # dtypes + non-null counts → spot missing values
```

### 2. Handle missing values
| Strategy | When | Effect |
|----------|------|--------|
| Drop rows/columns | value mostly missing | loses data, simplest |
| Mean/median imputation | *numerical* column, few missing | preserves rows |
| Mode imputation | *categorical* column | preserves rows |

```python
from sklearn.impute import SimpleImputer
imputer = SimpleImputer(missing_values = np.nan, strategy = "mean")
# 'X[:, 1:3]' → apply to the numeric columns only
X[:, 1:3] = imputer.fit_transform(X[:, 1:3])
```

### 3. Encode categorical variables
Most models need numbers, so text categories become **dummy codes**:

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
ct = ColumnTransformer([("encoder", OneHotEncoder(), [0])], remainder = "passthrough")
X = np.array(ct.fit_transform(X))
```
- **Nominal** (no order): one-hot encoding → `France` → `[1, 0, 0]`.
- **Ordinal** (has order, e.g. low/med/high): label/ordinal encoding keeps the ranking.

### 4. Split train vs test
The golden rule: **fit on train, apply to test.** The test set must stay unseen — it simulates the future.

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
```

### 5. Feature scaling
Columns measured in different units dominate each other (salary ~5 digits vs years ~1 digit). Scale so the model treats them fairly — and **fit the scaler on train only**, then `transform` the test set.

| Scaler | What it does | When |
|--------|--------------|------|
| **StandardScaler** | mean 0, variance 1 | default — robust to outliers-ish, works with most models |
| **MinMaxScaler** | range [0,1] | bounded features / neural nets |

```python
from sklearn.preprocessing import StandardScaler
sc = StandardScaler()
X_train[:, 3:] = sc.fit_transform(X_train[:, 3:])
X_test[:, 3:]  = sc.transform(X_test[:, 3:])
```

## The one rule to never break
**Imputer + encoder + scaler are fit on the *training* split; the test split is only *transformed*.** Leaking stats from test into train (by fitting scalers on the full dataset) inflates performance estimates and lies about generalization.

## Code
Worked end-to-end: [`02-Code/data-preprocessing.ipynb`](../02-Code/data-preprocessing.ipynb) — data: `Data.csv` (kept local, git-ignored).

## Pitfalls
- Fitting preprocessing on all data, then splitting — sneaky data leakage.
- Mean-imputing categorical columns (garbage subtraction) or one-hotting ordinal columns (destroying ordering info).
- Scaling before splitting (same leakage, different disguise).