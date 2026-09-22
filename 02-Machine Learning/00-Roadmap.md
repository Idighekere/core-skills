---
title: "ML & AI Engineering Roadmap"
type: roadmap
---
# Roadmap

Status ladder: `learning` -> `built` -> `explained` -> `mastered`

Rule: a topic is done only when I pass its **Proof** out loud, notes closed.

Focus: Machine Learning -> Deep Learning -> LLMs/AI Engineering, alongside DSA.

---

## Module 0: Prerequisites (run alongside Module 1)

### 0.1 Python for ML
- [ ] NumPy: arrays, shapes, broadcasting, vectorization, indexing, `axis`, random seeds
- [ ] Pandas: DataFrame/Series, `loc`/`iloc`, groupby, merge/join, pivot, missing values, `apply` vs vectorized
- [ ] Visualization: Matplotlib basics, Seaborn, histograms, scatter, boxplots, correlation heatmaps
- [ ] Tooling: virtual environments (`uv`/`venv`), Jupyter, reading tracebacks
- **Proof:** clean a messy CSV and answer 5 questions with Pandas only, no lookups

### 0.2 Linear algebra
- [ ] Vectors, matrices, tensors and their shapes
- [ ] Dot product, matrix multiplication (and why order matters), transpose
- [ ] Inverse, determinant, rank (intuition only)
- [ ] Norms (L1, L2), distance, cosine similarity
- [ ] Eigenvalues/eigenvectors, SVD (intuition, needed for PCA)
- **Proof:** explain what a matrix multiplication does geometrically

### 0.3 Calculus
- [ ] Derivative, partial derivative, gradient
- [ ] Chain rule (the entire basis of backprop)
- [ ] Gradient descent as "walk downhill"; learning rate
- [ ] Jacobian (intuition only)
- **Proof:** derive the MSE gradient by hand

### 0.4 Probability and statistics
- [ ] Random variables, distributions (Bernoulli, binomial, normal, uniform)
- [ ] Expectation, variance, covariance, correlation
- [ ] Conditional probability, Bayes' theorem, independence
- [ ] Maximum likelihood estimation (why cross-entropy exists)
- [ ] Sampling, central limit theorem, confidence intervals, p-values, hypothesis tests
- **Proof:** Bayes with a medical test example; explain MLE in plain words

---

## Module 1: Core ML

### 1.1 What ML is
- [x] Supervised, unsupervised, self-supervised, reinforcement learning
- [ ] Regression vs classification; features, labels, parameters vs hyperparameters
- [ ] Full workflow: problem, data, features, model, evaluation, deploy, monitor
- [ ] When NOT to use ML
- **Proof:** classify 5 business problems by type and name the data needed

### 1.2 Linear regression
- [ ] Hypothesis, MSE cost, gradient descent (batch, SGD, mini-batch)
- [ ] Learning rate, convergence, feature scaling
- [ ] Normal equation, multiple and polynomial features
- Notes: [Simple Linear Regression](01-Concepts/Simple%20Linear%20Regression.md), [Regression](01-Concepts/Regression.md)
- [ ] **Build:** from scratch in NumPy
- **Proof:** explain why scaling speeds up gradient descent

### 1.3 Logistic regression
- [ ] Sigmoid, decision boundary, log loss/cross-entropy
- [ ] Why MSE fails for classification
- [ ] Multiclass: one-vs-rest, softmax
- Notes: [Logistic Regression](01-Concepts/Logistic%20Regression.md), [Classification](01-Concepts/Classification.md)
- [ ] **Build:** from scratch
- **Proof:** derive why the log-loss gradient looks like linear regression's

### 1.4 Generalization
- [ ] Overfitting, underfitting, bias-variance
- [ ] Train/validation/test split; k-fold and stratified CV
- [ ] Regularization: L1 (Lasso), L2 (Ridge), elastic net
- [ ] Learning curves, **data leakage** (a top interview topic)
- Notes: [The Bias-Variance Tradeoff](01-Concepts/The%20Bias-Variance%20Tradeoff.md)
- **Proof:** why 99% train accuracy can still be useless, and three fixes

### 1.5 Evaluation metrics
- [ ] Regression: MAE, MSE, RMSE, R-squared
- [ ] Classification: confusion matrix, precision, recall, F1, ROC-AUC, PR curve
- [ ] Class imbalance: resampling, class weights, why accuracy lies
- [ ] Threshold selection, calibration
- Notes: [Loss Functions](01-Concepts/Loss%20Functions.md)
- **Proof:** pick and defend a metric for fraud detection

### 1.6 Feature engineering and data preparation
- [ ] Missing values, outliers, duplicates
- [ ] Encoding: one-hot, ordinal, target; scaling: standardization vs normalization
- [ ] Feature selection and interactions
- [ ] sklearn `Pipeline` and `ColumnTransformer`
- Notes: [Data Preprocessing](01-Concepts/Data%20Preprocessing.md), [Pipelines](01-Concepts/Pipelines.md)
- **Proof:** a preprocessing pipeline that cannot leak test data

### 1.7 Trees and ensembles
- [ ] Decision trees: entropy, Gini, information gain, pruning
- [ ] Bagging and random forests: bootstrap, feature randomness, OOB error
- [ ] Boosting: AdaBoost idea, gradient boosting, XGBoost/LightGBM, key hyperparameters
- [ ] Feature importance, SHAP basics
- Notes: [Tree Based Models](01-Concepts/Tree%20Based%20Models.md), [Bagging and Random Forest](01-Concepts/Bagging%20and%20Random%20Forest.md), [Boosting](01-Concepts/Boosting.md), [Ensemble Learning](01-Concepts/Ensemble%20Learning.md)
- [ ] **Build:** decision tree from scratch
- **Proof:** bagging vs boosting in two minutes

### 1.8 Other classic models
- [ ] k-Nearest Neighbors, Naive Bayes
- [ ] SVMs: margin, support vectors, kernels
- [ ] When each is the right choice
- Notes: [Support Vector Machines](01-Concepts/Support%20Vector%20Machines.md), [Linear Classifiers](01-Concepts/Linear%20Classifiers.md)

### 1.9 Unsupervised learning
- [ ] k-means (and where it fails), hierarchical clustering, DBSCAN
- [ ] PCA: math and intuition; t-SNE/UMAP for visualization
- [ ] Anomaly detection basics
- [ ] **Build:** k-means and PCA from scratch

### 1.10 Tuning and experiment discipline
- [ ] Grid vs random vs Bayesian search (Optuna)
- [ ] Baselines: always start dumb
- [ ] Experiment tracking (MLflow or Weights & Biases)
- [ ] Error analysis: read your model's actual mistakes
- Notes: [Fine-Tuning Models](01-Concepts/Fine-Tuning%20Models.md)

- [ ] **Capstone 1:** end-to-end tabular project with error analysis and write-up

---

## Module 2: Deep learning

### 2.1 Neural network foundations
- [ ] Perceptron, layers, activations (ReLU, sigmoid, tanh, softmax, GELU)
- [ ] Forward pass, loss functions
- [ ] **Backpropagation by hand** until obvious
- [ ] Initialization, vanishing/exploding gradients
- [ ] **Build:** NumPy net, then a micrograd-style autograd engine

### 2.2 Training deep networks
- [ ] Optimizers: SGD, momentum, RMSprop, Adam, AdamW
- [ ] LR schedules, batch size effects
- [ ] Regularization: dropout, weight decay, early stopping, augmentation
- [ ] Batch norm vs layer norm
- **Proof:** recite a debugging checklist for a loss that won't go down (start by overfitting one batch)

### 2.3 PyTorch
- [ ] Tensors, autograd, `nn.Module`, Dataset/DataLoader
- [ ] Training loop, GPU usage, saving/loading, reproducibility
- [ ] **Build:** rebuild 2.1's model in PyTorch with a clean loop

### 2.4 CNNs and transfer learning
- [ ] Convolution, stride, padding, pooling, receptive field
- [ ] ResNet and skip connections
- [ ] Transfer learning and fine-tuning
- [ ] **Build:** image classifier with transfer learning

### 2.5 Sequence models (context only)
- [ ] RNNs, LSTMs, GRUs; why attention replaced them

### 2.6 Transformers
- [ ] Tokenization (BPE), embeddings, positional encoding
- [ ] Self-attention (Q, K, V), multi-head attention
- [ ] Encoder vs decoder, residuals, layer norm, feed-forward blocks
- [ ] **Build:** a mini-GPT
- **Proof:** whiteboard self-attention with the formula and explain every term

- [ ] **Capstone 2:** from-scratch project with write-up

---

## Module 3: LLMs and AI engineering

### 3.1 How LLMs work
- [ ] Pretraining, next-token prediction, scaling (conceptual)
- [ ] Instruction tuning, RLHF/preference optimization (conceptual)
- [ ] Context window, sampling (temperature, top-k, top-p), hallucination

### 3.2 Embeddings and vector search
- [ ] Text embeddings, cosine similarity
- [ ] Vector databases (FAISS, Chroma, pgvector), HNSW/approximate NN
- [ ] Hybrid search (BM25 plus dense)

### 3.3 Prompting and structured outputs
- [ ] Prompt design, few-shot, chain-of-thought
- [ ] JSON/schema-constrained outputs, function/tool calling

### 3.4 RAG
- [ ] Chunking strategies, retrieval, reranking, query rewriting
- [ ] Context construction, citations, failure modes
- [ ] **Build:** RAG app on a real corpus

### 3.5 Fine-tuning
- [ ] Full fine-tune vs LoRA/QLoRA
- [ ] When fine-tuning beats RAG (and when it doesn't)
- [ ] **Build:** LoRA fine-tune of a small open model

### 3.6 Agents
- [ ] Tool use, ReAct loop, planning, memory
- [ ] Failure modes: loops, tool errors, cost blowups
- [ ] MCP basics

### 3.7 Evaluation (what separates seniors from juniors)
- [ ] Building eval sets, LLM-as-judge and its biases
- [ ] Retrieval metrics: recall@k, MRR
- [ ] Prompt regression tests, human evaluation
- [ ] Tracing and observability

---

## Module 4: Production

### 4.1 Engineering hygiene
- [ ] Project structure, testing, typing, linting, Git workflow
- [ ] Reproducibility: seeds, pinned dependencies, data versioning

### 4.2 Serving
- [ ] FastAPI, batching, latency vs throughput
- [ ] Docker/Podman, deploy on a free tier

### 4.3 MLOps essentials
- [ ] Experiment tracking, model registry, CI/CD for ML
- [ ] Monitoring: data drift, concept drift, performance decay
- [ ] Cost and latency optimization (caching, smaller models, quantization)

### 4.4 ML system design
- [ ] Recommendation, search, fraud detection, RAG system
- [ ] Batch vs real-time, online vs offline evaluation

- [ ] **Capstone 3 (flagship):** deployed AI product with evals, monitoring, public write-up

---

## Writing tiers

Reading is the input. Writing is what proves I understood. Move up a tier only when the one below is easy.

| Tier | Form | Length | Frequency | Purpose |
|---|---|---|---|---|
| 1 | Daily log | 5 lines, private | Daily | Record what I learned and couldn't explain |
| 2 | Concept note | 1 page | Per concept | Own-words explanation + Self-test |
| 3 | Short public note | 2-5 sentences | 2-3 per week | A bug, a "finally clicked" moment, a chart |
| 4 | Blog post | 600-1000 words | Every 1-2 weeks | Teach one concept end to end |
| 5 | Deep write-up | 2000+ words | Per capstone | Full project story: problem, method, evals, failures |
| 6 | Paper notes | 1 page | Optional, after Module 2 | Problem, idea, method, result, my critique |

Tiers 1-2 are mandatory. Tiers 3-5 are the public byproduct.

**Blog post template (Tier 4)**

~~~markdown
# Title: the question I was stuck on

1. Hook (2-3 sentences): what confused me or what problem I tried to solve
2. Short answer: the concept in plain words, no jargon
3. Intuition: an analogy or a diagram
4. The build: my code in chunks, 1-2 sentences after each chunk
5. Where it broke: my mistakes and how I found them
6. What I'd tell my past self: 3 bullets
7. Resources I used: links, in the order they helped
~~~

**Short public note formats (Tier 3)**
- "Today I tried X and it broke because Y. Fix: Z."
- "The one-sentence version of [concept] I wish I had started with: ..."
- A chart or screenshot plus 2 lines on what it taught me

**My earlier posts (progression, not a pivot)**
- Laptop price predictor -> Module 1.2 and 1.6. The deeper pass: rebuild from scratch, error analysis, leakage checks.
- Classification metrics / confusion matrix -> Module 1.5. The deeper pass: precision-recall tradeoff, ROC-AUC, imbalance, threshold choice.
- When posting new work, say so plainly: "I shared X a while back. I'm now going deeper on the fundamentals underneath it." Never delete old posts. Link back to them.

---

## Resources

**Video channels**
- StatQuest: https://www.youtube.com/@statquest
- 3Blue1Brown: https://www.youtube.com/@3blue1brown
- Andrej Karpathy: https://www.youtube.com/@AndrejKarpathy
- Stanford Online (CS229): https://www.youtube.com/@stanfordonline
- freeCodeCamp: https://www.youtube.com/@freecodecamp

**Courses**
- Karpathy, Neural Networks: Zero to Hero: https://karpathy.ai/zero-to-hero.html
- Hugging Face LLM Course: https://huggingface.co/learn/llm-course
- fast.ai: https://course.fast.ai
- Andrew Ng, Machine Learning Specialization: https://www.coursera.org/specializations/machine-learning-introduction
- DeepLearning.AI short courses: https://www.deeplearning.ai/short-courses/

**Books**
- Sebastian Raschka, Build a Large Language Model (From Scratch): https://sebastianraschka.com/llms-from-scratch/
- Code for the book: https://github.com/rasbt/LLMs-from-scratch
- Dive into Deep Learning: https://d2l.ai
- Mathematics for Machine Learning: https://mml-book.github.io
- Hands-On ML notebooks (Geron): https://github.com/ageron/handson-ml3

**Blogs and engineering writing**
- Sebastian Raschka: https://sebastianraschka.com/blog/
- Lilian Weng: https://lilianweng.github.io
- Chip Huyen: https://huyenchip.com
- Eugene Yan: https://eugeneyan.com
- Jay Alammar: https://jalammar.github.io
- Hugging Face blog: https://huggingface.co/blog
- Company engineering blogs (Google, Meta, Netflix, Uber): search "[company] engineering blog ML"

**Practice and papers**
- Kaggle: https://www.kaggle.com
- Google Colab: https://colab.research.google.com
- arXiv: https://arxiv.org

**DSA**
- NeetCode: https://neetcode.io
- LeetCode: https://leetcode.com
- Algorythm.org bootcamp (current)

---

## How to use this roadmap

**The loop for every concept (about 3 days)**
1. **Understand:** one intuition source (StatQuest / 3Blue1Brown), one depth source (CS229 / Ng / d2l / Geron), one real-world source (a blog post). Then write "In my own words" without looking.
2. **Build:** implement from scratch (NumPy first, library second). Break it on purpose. Save the notebook in `02-Code/` and link it from the concept note.
3. **Prove:** answer the Self-test cold, explain it aloud, then update `status` and `last-tested` in the note's frontmatter. Anything I can't explain goes back to `learning`.

**Rules against hopping**
- One concept at a time. Pick sources per concept, not per platform.
- Order: intuition -> depth -> real-world -> close everything and build.
- Something interesting shows up mid-session? Put it in the Parked folder. Don't open it.
- Notes are named after concepts, never after resources.
- Finishing a course is not the goal. Passing the Proof is.
- First two weeks: only StatQuest, Karpathy Zero to Hero, and my own notes.

**Weekly rhythm**
- Mon: pick 2-3 concepts, tell my accountability partner what to test me on
- Tue-Thu: run the 3-day loop
- Fri: one-line check-in with partner (done / not done / stuck)
- Sat: accountability session, notes closed, explain aloud, then one public note
- Sun: 30-minute review, re-test 3 old concepts, plan Monday

**Accountability session (60-75 min, notes closed)**
1. Each person explains 2 concepts aloud, 5 minutes each, no interruptions
2. Listener asks 3 hard questions (why does it fail, what changes at 100x data, where would you use it)
3. Whiteboard one derivation or diagram from memory
4. Swap one DSA problem and explain approach and complexity aloud
5. Anything that wobbled drops back to `status: learning`

**DSA alongside ML**
- 2 problems a day (45-60 min), explain approach and complexity aloud after each
- Failed problems go on the `Solve again` list, retried after 7 days
- Bootcamp backlog (weeks 5-9): treat it as a problem list, not a video list
