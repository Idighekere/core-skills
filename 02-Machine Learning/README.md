# 02 - Machine Learning

Hands-on ML from courses and notebooks. The pattern here: each **concept note** explains the *why*, and the **notebooks** show the *how* with real code you can rerun in Colab.

## 01 - Concepts

| Note | Covers |
|------|--------|
| [Data Preprocessing](01-Concepts/Data%20Preprocessing.md) | cleaning, encoding, feature scaling — the first 80% of most projects |
| [Simple Linear Regression](01-Concepts/Simple%20Linear%20Regression.md) | ordinary least squares, cost, gradient descent, evaluation |

## 02 - Code

Notebooks (load into Google Colab/Jupyter; the CSVs are local helper data and are git-ignored — re-download or re-upload as needed):

| Notebook | Open in Colab |
|----------|---------------|
| [data-preprocessing.ipynb](02-Code/data-preprocessing.ipynb) | <a href="https://colab.research.google.com/github/Idighekere/core-skills/blob/main/02-Machine%20Learning/02-Code/data-preprocessing.ipynb" target="_blank"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> |
| [simple-linear-regression.ipynb](02-Code/simple-linear-regression.ipynb) | <a href="https://colab.research.google.com/github/Idighekere/core-skills/blob/main/02-Machine%20Learning/02-Code/simple-linear-regression.ipynb" target="_blank"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> |

**Data note:** `Salary_Data.csv` (regression) and `Data.csv` (preprocessing) are kept locally but **not committed** to git. To run the notebooks, upload the CSVs in Colab or download them from the course sources.

## Priors / next steps

- Everything here was written on a first pass through ML coursework (IBM machine learning + a linear-regression intro). Revisit the concept notes and rewire examples into a small portfolio project.
- Logical next topics: multiple regression, train/test hygiene, gradient descent variants, classification metrics.