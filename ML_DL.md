# Machine Learning, Deep Learning & Transformers
### Complete Notes for AI/ML Engineers | Interview-Ready | Placement-Focused

> **How to use this:** Every section is written so the concept explanation *is* the answer to the interview question at the top. Covers theory + intuition + math + code patterns. Sections flow from classical ML → deep learning → transformers → interview Q&A.

---

## Table of Contents

**Part 1 — Classical Machine Learning**
1. [ML Fundamentals & Taxonomy](#1-ml-fundamentals--taxonomy)
2. [Bias-Variance Tradeoff & Regularisation](#2-bias-variance-tradeoff--regularisation)
3. [Linear & Logistic Regression — Deep Dive](#3-linear--logistic-regression--deep-dive)
4. [Decision Trees, Random Forests & Boosting](#4-decision-trees-random-forests--boosting)
5. [Support Vector Machines](#5-support-vector-machines)
6. [Clustering — K-Means, DBSCAN, Hierarchical](#6-clustering--k-means-dbscan-hierarchical)
7. [Dimensionality Reduction — PCA, t-SNE, UMAP](#7-dimensionality-reduction--pca-t-sne-umap)
8. [Model Evaluation & Metrics](#8-model-evaluation--metrics)
9. [Feature Engineering & Selection](#9-feature-engineering--selection)
10. [Optimisers — Gradient Descent Family](#10-optimisers--gradient-descent-family)

**Part 2 — Deep Learning**
11. [Neural Networks — Forward Pass & Backpropagation](#11-neural-networks--forward-pass--backpropagation)
12. [Activation Functions](#12-activation-functions)
13. [Loss Functions](#13-loss-functions)
14. [Regularisation in Deep Learning](#14-regularisation-in-deep-learning)
15. [Convolutional Neural Networks (CNN)](#15-convolutional-neural-networks-cnn)
16. [Recurrent Neural Networks — RNN, LSTM, GRU](#16-recurrent-neural-networks--rnn-lstm-gru)
17. [Batch Normalisation & Layer Normalisation](#17-batch-normalisation--layer-normalisation)
18. [Transfer Learning & Fine-Tuning](#18-transfer-learning--fine-tuning)
19. [Autoencoders & VAEs](#19-autoencoders--vaes)
20. [GANs — Generative Adversarial Networks](#20-gans--generative-adversarial-networks)

**Part 3 — Transformers & Modern NLP**
21. [Attention Mechanism — Deep Dive](#21-attention-mechanism--deep-dive)
22. [Transformer Architecture](#22-transformer-architecture)
23. [BERT — Encoder-Only Models](#23-bert--encoder-only-models)
24. [GPT — Decoder-Only Models](#24-gpt--decoder-only-models)
25. [T5, BART — Encoder-Decoder Models](#25-t5-bart--encoder-decoder-models)
26. [Positional Encoding — Sinusoidal, RoPE, ALiBi](#26-positional-encoding--sinusoidal-rope-alibi)
27. [Efficient Transformers — Flash Attention, MoE, GQA](#27-efficient-transformers--flash-attention-moe-gqa)
28. [Vision Transformers (ViT)](#28-vision-transformers-vit)

**Part 4 — Interview Q&A**
29. [Interview Q&A — Classical ML](#29-interview-qa--classical-ml)
30. [Interview Q&A — Deep Learning](#30-interview-qa--deep-learning)
31. [Interview Q&A — Transformers & NLP](#31-interview-qa--transformers--nlp)
32. [Quick Reference Cheat Sheet](#32-quick-reference-cheat-sheet)

---

## 1. ML Fundamentals & Taxonomy

> **Interview Q:** *What is machine learning? How does it differ from traditional programming and from deep learning?*

**Traditional programming:** Human writes explicit rules → Rules + Data → Output.
**Machine learning:** Algorithm learns rules from data → Data + Output (labels) → Rules (model).
**Deep learning:** ML with deep neural networks — automatically learns hierarchical feature representations from raw data.

### ML Taxonomy

```
Machine Learning
├── Supervised Learning      → labelled data, predict output
│   ├── Classification       → discrete output (spam/not spam)
│   └── Regression           → continuous output (house price)
├── Unsupervised Learning    → no labels, find structure
│   ├── Clustering           → group similar data points
│   ├── Dimensionality Red.  → compress features
│   └── Density Estimation   → learn data distribution
├── Semi-Supervised          → small labelled + large unlabelled
├── Self-Supervised          → labels derived from data itself (masking, next-token)
└── Reinforcement Learning   → agent, environment, reward signal
```

### The ML Pipeline (always mentioned in interviews)

```
1. Problem Definition    → what are we predicting? what metric matters?
2. Data Collection       → sources, volume, freshness
3. EDA                   → distributions, correlations, outliers, class balance
4. Data Preprocessing    → missing values, encoding, scaling
5. Feature Engineering   → create informative features
6. Model Selection       → choose algorithm family
7. Training              → fit model on training data
8. Evaluation            → metrics on held-out test set
9. Hyperparameter Tuning → Grid/Random/Bayesian search
10. Deployment           → serving, monitoring, retraining
```

### No Free Lunch Theorem

No single model is best for all problems. A model's performance on unseen data is linked to assumptions (inductive bias) about the data. The theorem implies: always benchmark multiple approaches.

---

## 2. Bias-Variance Tradeoff & Regularisation

> **Interview Q:** *What is the bias-variance tradeoff? How does it relate to overfitting and underfitting?*

### The Decomposition

Expected prediction error can be decomposed as:

```
Total Error = Bias² + Variance + Irreducible Noise

Bias²:    Error from wrong assumptions in the model
          (underfitting — model too simple)
Variance: Error from sensitivity to training data fluctuations
          (overfitting — model too complex)
Noise:    Inherent randomness in data (can't be eliminated)
```

**Intuition:**
- **High Bias (Underfitting):** A linear model trying to fit a sine wave. Training AND test error are both high. Model misses the real pattern.
- **High Variance (Overfitting):** A degree-10 polynomial on 10 data points. Training error near 0, test error very high. Model memorised noise.
- **Sweet Spot:** Model complexity balanced so both train and test error are low.

```
Model Complexity →
         Underfitting    Optimal    Overfitting
Train err:    High          Low         Very Low
Test err:     High          Low         High
Bias:         High          Low         Low
Variance:     Low           Low         High
```

### Regularisation — Controlling Variance

**L2 Regularisation (Ridge):**
```
Loss = MSE + λ · Σwᵢ²
```
Penalises large weights. Shrinks weights toward zero but doesn't zero them out. Closed-form solution exists. Handles correlated features well.

**L1 Regularisation (Lasso):**
```
Loss = MSE + λ · Σ|wᵢ|
```
Produces sparse weights — drives irrelevant feature weights exactly to zero. Built-in feature selection. No closed-form (subgradient needed).

**ElasticNet:** Combines L1 + L2:
```
Loss = MSE + λ₁·Σ|wᵢ| + λ₂·Σwᵢ²
```

| | L1 (Lasso) | L2 (Ridge) | ElasticNet |
|---|---|---|---|
| Sparsity | Yes (zeros weights) | No | Partial |
| Feature selection | Built-in | No | Partial |
| Handles correlated | Poor | Good | Good |
| Solution | No closed-form | Closed-form | No closed-form |
| Geometry | Diamond constraint | Sphere constraint | Combined |

> **Interview Q:** *Why does L1 produce sparse solutions and L2 doesn't?*
> The L1 penalty has corners at axes — gradient descent paths are likely to hit an axis (zero). The L2 penalty is smooth and circular — paths spiral to the origin but rarely land exactly on an axis. Geometrically: the L1 ball is a diamond (corners on axes), the L2 ball is a sphere (no corners). The optimal solution in a constrained optimisation is most likely to hit a corner of the constraint set.

### Cross-Validation

Estimate generalisation error using k-fold CV:
```
Split data into k folds (typically k=5 or k=10)
For each fold:
    Train on remaining k-1 folds
    Evaluate on this fold
Report mean ± std of k evaluation scores
```

**Stratified k-fold:** Preserve class distribution in each fold — essential for imbalanced datasets.

---

## 3. Linear & Logistic Regression — Deep Dive

> **Interview Q:** *Derive the linear regression update rule. What are its assumptions?*

### Linear Regression

**Model:** `ŷ = Xw + b` (predict a continuous value)

**Loss function:** Mean Squared Error:
```
MSE = (1/n) · Σ(yᵢ - ŷᵢ)²
```

**Closed-form solution (Normal Equation):**
```
w* = (XᵀX)⁻¹Xᵀy
```
Works for small datasets. Fails when XᵀX is singular (collinear features) or when n is huge (O(d³) inversion).

**Gradient Descent:**
```
∂MSE/∂w = (2/n) · Xᵀ(Xw - y)
w ← w - η · ∂MSE/∂w
```

**Assumptions of Linear Regression (critical for interviews):**
1. **Linearity:** Relationship between X and y is linear
2. **Independence:** Observations are independent (no autocorrelation)
3. **Homoscedasticity:** Constant variance of residuals
4. **Normality of residuals:** Errors are normally distributed
5. **No multicollinearity:** Features are not highly correlated

### Logistic Regression

**Model:** Outputs probability of class 1:
```
P(y=1|x) = σ(wᵀx + b) = 1 / (1 + e^(-(wᵀx+b)))
```

**Loss function:** Binary Cross-Entropy (Log Loss):
```
L = -(1/n) · Σ [yᵢ·log(ŷᵢ) + (1-yᵢ)·log(1-ŷᵢ)]
```

**Why not MSE for classification?**
MSE with sigmoid produces a non-convex loss surface — many local minima. Cross-entropy gives a convex surface with a unique global minimum. Also, cross-entropy's gradient is cleaner: `∂L/∂w = (1/n)·Xᵀ(ŷ - y)`.

**Decision boundary:** Where `wᵀx + b = 0` — always a hyperplane (linear). For non-linear boundaries, use polynomial features or other models.

**Multiclass extensions:**
- **One-vs-Rest (OvR):** Train K binary classifiers; predict class with highest probability
- **Softmax Regression:** `P(y=k|x) = exp(wₖᵀx) / Σⱼexp(wⱼᵀx)` — extends logistic to K classes directly

---

## 4. Decision Trees, Random Forests & Boosting

> **Interview Q:** *How does a decision tree split? What are Random Forests and how do they reduce variance?*

### Decision Trees

A tree recursively partitions the feature space. At each node, choose the feature and threshold that best separates classes.

**Split criteria for classification:**

**Gini Impurity:**
```
Gini = 1 - Σ pᵢ²
```
0 = perfect purity, 0.5 = maximum impurity (binary). Measures probability of misclassifying a randomly chosen element.

**Information Gain (Entropy-based):**
```
Entropy = -Σ pᵢ · log₂(pᵢ)
IG = Entropy(parent) - Σ (|child|/|parent|) · Entropy(child)
```

**For regression:** Use variance reduction (MSE decrease) as split criterion.

**Stopping criteria:** Max depth, min samples per leaf, min impurity decrease, max features.

**Overfitting in trees:** Deep trees memorise training data. Control with: max_depth, min_samples_split, min_samples_leaf, pruning (cost-complexity pruning).

### Ensemble Methods

**Bagging (Bootstrap Aggregation):**
- Sample n data points with replacement (bootstrap) → train separate models → aggregate
- Reduces variance without increasing bias
- Works best with high-variance base learners (deep trees)

### Random Forests

Random Forest = Bagging + Feature Randomness:
- For each tree: bootstrap sample of data
- At each split: only consider a **random subset of features** (√d for classification, d/3 for regression)
- Final prediction: majority vote (classification) or mean (regression)

**Why feature randomness?** Without it, all trees would use the same dominant features → correlated trees → bagging doesn't help. Random feature subsets decorrelate trees → better variance reduction.

**Out-of-Bag (OOB) Error:** Each bootstrap sample leaves ~37% of data unsampled. Use these as a free validation set → OOB error ≈ cross-validation error.

**Feature Importance:** Average decrease in impurity across all trees for each feature.

### Gradient Boosting

**Key idea:** Train models sequentially, each correcting the errors of the previous:

```
Stage 1: Train F₁(x) → predict ŷ
Stage 2: Compute residuals r = y - ŷ
         Train F₂(x) on residuals r
Stage 3: Update: F₂_total = F₁ + lr · F₂
Stage 4: Compute new residuals, train F₃...
Final:   F(x) = F₁(x) + lr·F₂(x) + lr·F₃(x) + ...
```

**Why residuals?** The gradient of MSE loss w.r.t. predictions is exactly the residuals. So fitting on residuals = gradient descent in function space.

**Hyperparameters:** n_estimators, learning_rate, max_depth (shallow trees: 3–5 preferred), subsample, min_samples_leaf.

### XGBoost / LightGBM / CatBoost — Production-Grade Boosting

| Feature | XGBoost | LightGBM | CatBoost |
|---|---|---|---|
| Tree growth | Level-wise | Leaf-wise (faster) | Symmetric |
| Speed | Good | Fastest | Good |
| Categoricals | Encode manually | Built-in (limited) | Native (best) |
| Memory | Moderate | Low | Moderate |
| Regularisation | L1 + L2 | L1 + L2 | Built-in |
| Best for | General purpose | Large datasets | Datasets with many categoricals |

**LightGBM's leaf-wise growth:** Instead of growing level by level (expanding all leaves), LightGBM grows the leaf with the highest loss reduction. Faster convergence but more prone to overfitting on small datasets — control with `min_data_in_leaf`.

> **Interview Q:** *Bias-Variance in trees vs forests vs boosting?*
> Single deep tree: low bias, high variance. Random Forest: reduces variance via averaging; bias unchanged. Boosting: reduces bias sequentially (each model corrects errors); risk of variance increase if not regularised. Boosting generally outperforms Random Forests on tabular data at the cost of longer training and more tuning.

---

## 5. Support Vector Machines

> **Interview Q:** *Explain SVMs. What is the kernel trick?*

### Hard Margin SVM

Find the hyperplane `wᵀx + b = 0` that separates classes with the **maximum margin** — the distance from the hyperplane to the nearest data points (support vectors).

```
Maximise margin = 2 / ||w||
Subject to: yᵢ(wᵀxᵢ + b) ≥ 1  for all i
```

Equivalent to minimising `(1/2)||w||²`. Dual form: express in terms of dot products between training points.

### Soft Margin SVM (C-SVM)

Allow some misclassification via slack variables ξᵢ:
```
Minimise: (1/2)||w||² + C · Σξᵢ
Subject to: yᵢ(wᵀxᵢ + b) ≥ 1 - ξᵢ,  ξᵢ ≥ 0
```

**C parameter:** Large C → smaller margin, fewer violations (risk overfitting). Small C → larger margin, more violations tolerated (risk underfitting).

### Kernel Trick

For non-linearly separable data: map to higher-dimensional space where data IS separable. The kernel trick computes dot products in the high-dimensional space without explicitly computing the transformation:

```
K(xᵢ, xⱼ) = φ(xᵢ)ᵀ φ(xⱼ)   ← kernel computes this implicitly
```

**Common kernels:**

| Kernel | Formula | Use |
|---|---|---|
| Linear | `xᵢᵀxⱼ` | Default baseline |
| Polynomial | `(γxᵀy + r)^d` | Polynomial decision boundaries |
| RBF (Gaussian) | `exp(-γ||xᵢ-xⱼ||²)` | Most common; radial decision boundaries |
| Sigmoid | `tanh(γxᵀy + r)` | Neural net-like |

**RBF kernel intuition:** Measures similarity based on distance. `γ` controls the radius of influence of each support vector. Large γ → narrow Gaussian → complex boundary (overfitting). Small γ → wide Gaussian → smooth boundary (underfitting).

---

## 6. Clustering — K-Means, DBSCAN, Hierarchical

> **Interview Q:** *How does K-Means work? What are its limitations?*

### K-Means Algorithm

```
1. Initialise K centroids randomly (or K-Means++)
2. Assign each point to nearest centroid (Euclidean distance)
3. Recompute centroids as mean of assigned points
4. Repeat steps 2-3 until centroids don't move (convergence)
```

**Objective:** Minimise within-cluster sum of squares (WCSS / inertia):
```
J = Σₖ Σ_{x∈Cₖ} ||x - μₖ||²
```

**Choosing K:**
- **Elbow method:** Plot inertia vs K; find the "elbow" where diminishing returns begin
- **Silhouette score:** Measures how well each point fits its cluster vs. neighbours. Range [-1, 1]; higher is better.

**Limitations:**
- Requires K specified upfront
- Assumes spherical (circular) clusters — fails on elongated or non-convex shapes
- Sensitive to outliers (centroid pulled toward outliers)
- Sensitive to feature scale (must standardise first)
- Local optima — depends on initialisation (K-Means++ mitigates)

**K-Means++:** Initialise centroids to be far from each other — first centroid random, subsequent centroids chosen with probability proportional to distance from nearest existing centroid. Reduces worst-case performance.

### DBSCAN (Density-Based Spatial Clustering)

Groups together points that are closely packed; marks outliers as noise.

**Parameters:** `eps` (neighbourhood radius), `min_samples` (minimum points to form a core point).

**Labels:**
- **Core point:** At least `min_samples` points within `eps` radius
- **Border point:** Within `eps` of a core point but not a core point itself
- **Noise:** Neither core nor border

**Advantages over K-Means:**
- Discovers arbitrary-shaped clusters
- Automatically identifies outliers (noise)
- No K needed upfront
- Robust to outliers

**Disadvantage:** Struggles with varying density clusters; sensitive to eps and min_samples.

### Hierarchical Clustering

Builds a tree (dendrogram) of clusters:
- **Agglomerative (bottom-up):** Start with each point as its own cluster; merge closest clusters iteratively
- **Divisive (top-down):** Start with one cluster; split iteratively

**Linkage criteria (how to measure cluster distance):**
- Single linkage: minimum pairwise distance (chaining effect)
- Complete linkage: maximum pairwise distance (compact clusters)
- Average linkage: mean pairwise distance (balance)
- Ward linkage: minimise within-cluster variance (most common)

**Advantage:** No need to specify K upfront; cut dendrogram at desired level. **Disadvantage:** O(n³) for agglomerative — doesn't scale.

---

## 7. Dimensionality Reduction — PCA, t-SNE, UMAP

> **Interview Q:** *What is PCA and how does it work? When would you use t-SNE instead?*

### PCA (Principal Component Analysis)

Find the directions (principal components) of maximum variance in the data and project onto them.

**Step-by-step:**
```
1. Standardise data (zero mean, unit variance)
2. Compute covariance matrix: Σ = (1/(n-1)) · XᵀX
3. Eigendecomposition: Σ = VΛVᵀ
   V = eigenvectors (principal components / directions)
   Λ = diagonal matrix of eigenvalues (variance explained)
4. Sort by eigenvalue (descending)
5. Project: Z = XV  (select top-k components)
```

**Explained variance ratio:** `λᵢ / Σλⱼ` — fraction of total variance captured by component i. Choose k such that cumulative explained variance ≥ 85–95%.

**What PCA preserves:** Global variance structure, linear relationships.
**What PCA loses:** Non-linear structure; distances may be distorted.

**Uses in ML:**
- Dimensionality reduction before training (combat curse of dimensionality)
- Visualisation (project to 2D/3D)
- Noise reduction (top components capture signal, later ones capture noise)
- Decorrelate features (PCA components are orthogonal)

### t-SNE (t-Distributed Stochastic Neighbour Embedding)

Non-linear dimensionality reduction, primarily for **visualisation** (2D/3D). Preserves local structure — nearby points in high-d stay nearby in low-d.

**How it works:**
1. Compute pairwise similarities in high-d space as probabilities (Gaussian kernel)
2. Initialise low-d embeddings randomly
3. Minimise KL divergence between high-d and low-d similarities using gradient descent
4. Uses t-distribution in low-d space (heavier tails → spread out clusters)

**Key hyperparameter:** Perplexity (5–50): controls the effective number of neighbours. Higher perplexity = considers more global structure.

**Limitations:** Non-deterministic (random init), slow O(n² ) or O(n log n) with Barnes-Hut, cannot project new points (transductive only), distances between clusters NOT meaningful — only local structure is preserved.

### UMAP (Uniform Manifold Approximation and Projection)

Like t-SNE but faster, more scalable, better preserves global structure, and can project new points.

| | PCA | t-SNE | UMAP |
|---|---|---|---|
| Linearity | Linear | Non-linear | Non-linear |
| Preserves | Global variance | Local structure | Local + some global |
| Speed | Fast | Slow O(n²) | Fast O(n log n) |
| New points | Yes | No | Yes |
| Reproducible | Yes | Partially (fix seed) | Yes (fix seed) |
| Best for | Preprocessing, noise reduction | Visualisation | Visualisation + clustering |

---

## 8. Model Evaluation & Metrics

> **Interview Q:** *When would you use F1 score vs AUC-ROC vs precision vs recall?*

### Classification Metrics

**Confusion Matrix:**
```
                  Predicted Positive  Predicted Negative
Actual Positive:        TP                 FN
Actual Negative:        FP                 TN
```

**Core metrics:**
```
Accuracy  = (TP + TN) / (TP + FP + TN + FN)    ← misleading when imbalanced
Precision = TP / (TP + FP)                       ← of predicted positives, how many correct?
Recall    = TP / (TP + FN)                       ← of actual positives, how many found?
F1 Score  = 2 · (Precision · Recall) / (Precision + Recall)  ← harmonic mean
```

**Precision-Recall Tradeoff:**
Lowering the decision threshold → more positives predicted → recall ↑, precision ↓. A high threshold → fewer positives → precision ↑, recall ↓.

**When to prioritise:**
- **High Recall:** Cancer detection, fraud (don't miss positives; FN costly)
- **High Precision:** Spam filter, legal document review (don't want false alarms; FP costly)
- **F1:** Imbalanced classes where both matter
- **Accuracy:** Only when classes are balanced

### ROC-AUC

ROC curve plots TPR (recall) vs FPR (1-specificity) at all thresholds.

```
AUC = Area under ROC curve
AUC = 0.5 → random classifier
AUC = 1.0 → perfect classifier
AUC = 0.8 → model ranks a random positive above a random negative 80% of the time
```

**AUC interpretation:** Probability that the model ranks a random positive example higher than a random negative one — a rank-based metric, threshold-independent.

**PR-AUC (Precision-Recall AUC):** Better than ROC-AUC for **highly imbalanced datasets** where negatives dominate — ROC can be misleadingly optimistic.

### Regression Metrics

```
MAE  = (1/n) · Σ|yᵢ - ŷᵢ|              ← robust to outliers; same units as target
MSE  = (1/n) · Σ(yᵢ - ŷᵢ)²             ← penalises outliers heavily
RMSE = √MSE                              ← same units as target
R²   = 1 - SS_res/SS_tot                ← proportion of variance explained; [−∞, 1]
MAPE = (1/n) · Σ|yᵢ - ŷᵢ|/|yᵢ| · 100% ← percentage error; undefined if yᵢ=0
```

**R² interpretation:** R² = 0.8 means the model explains 80% of the variance in y. R² can be negative (model worse than predicting the mean).

### Handling Imbalanced Datasets

- **Resampling:** Oversample minority (SMOTE) or undersample majority
- **Class weights:** `class_weight='balanced'` in sklearn — penalise misclassifying minority more
- **Threshold moving:** Adjust decision threshold post-training
- **Metrics:** Use F1, PR-AUC, Cohen's Kappa — not accuracy

---

## 9. Feature Engineering & Selection

> **Interview Q:** *What is feature engineering and how does it affect model performance?*

Feature engineering is the process of using domain knowledge to create or transform raw features into a representation that better captures the underlying patterns — often more impactful than model choice.

### Feature Scaling

**Why scale?**
- Distance-based models (KNN, SVM, K-Means) are sensitive to scale
- Gradient descent converges faster with scaled features
- Tree-based models (Random Forest, XGBoost) are scale-invariant

**Standardisation (Z-score):** `x' = (x - μ) / σ` → mean 0, std 1. Use when distribution is approximately normal.

**Min-Max Normalisation:** `x' = (x - min) / (max - min)` → range [0, 1]. Use when bounded range is needed (e.g., neural network inputs, image pixels).

**Robust Scaler:** `x' = (x - median) / IQR` → robust to outliers.

### Encoding Categorical Variables

**Label Encoding:** Map categories to integers (0, 1, 2, ...). Only appropriate for ordinal features — implies ordering. Never use for nominal categories in linear models or tree splits.

**One-Hot Encoding (OHE):** Binary column per category. Drop one column (dummy variable trap). Leads to high dimensionality with high-cardinality features.

**Target Encoding:** Replace category with mean of target variable for that category. Powerful but leaky — use with cross-validation within folds to prevent data leakage.

**Frequency Encoding:** Replace category with its frequency count. Preserves no ordering, no target signal, but handles high cardinality.

**Embedding (neural networks):** Learn a dense vector representation per category — best for high cardinality + neural networks.

### Handling Missing Values

**Statistical imputation:**
- Mean/median imputation → simple; distorts distribution
- Mode imputation for categoricals
- KNN imputation → uses neighbours; better but slow

**Model-based imputation:** Predict missing values using other features (IterativeImputer in sklearn).

**Indicator variables:** Add a binary "was_missing" column alongside imputed values — preserves missingness pattern as signal.

**When to drop:** If >50% of values are missing and the feature isn't critical.

### Feature Selection

**Filter methods (model-independent):**
- Pearson correlation (continuous vs continuous)
- Chi-squared test (categorical vs categorical)
- ANOVA F-test (continuous vs categorical)
- Mutual information

**Wrapper methods:**
- **Forward selection:** Start empty, add best feature iteratively
- **Backward elimination:** Start full, remove worst feature iteratively
- **Recursive Feature Elimination (RFE):** Fit model, eliminate least important feature, repeat

**Embedded methods (during training):**
- L1 regularisation (Lasso) zeros out irrelevant features
- Tree-based feature importance (Random Forest, XGBoost)

---

## 10. Optimisers — Gradient Descent Family

> **Interview Q:** *Compare SGD, Adam, and RMSProp. When would you use each?*

### Gradient Descent Variants

**Batch Gradient Descent:**
- Compute gradient over entire dataset per update
- Stable convergence, exact gradient
- Too slow for large datasets (one update per full pass)

**Stochastic Gradient Descent (SGD):**
- Compute gradient for one random sample per update
- Fast updates; noisy gradients; can escape local minima
- High variance in parameter updates

**Mini-batch SGD (most common):**
- Compute gradient over a small batch (32–512 samples)
- Balance between speed and stability
- Standard in deep learning

### Momentum

Accumulate a velocity vector in directions of persistent gradient:
```
v ← β·v + (1-β)·∇L
w ← w - η·v
```
Accelerates in consistent directions, dampens oscillations. β typically 0.9.

**Intuition:** Like a ball rolling downhill — builds up speed in the right direction, resists changes in direction.

### RMSProp

Adapts learning rate per parameter based on recent squared gradients:
```
s ← β·s + (1-β)·(∇L)²
w ← w - (η / √(s + ε)) · ∇L
```
Parameters with large recent gradients get smaller effective learning rate. Helps with non-stationary objectives; good for RNNs.

### Adam (Adaptive Moment Estimation)

Combines Momentum (1st moment) + RMSProp (2nd moment):
```
m ← β₁·m + (1-β₁)·∇L        ← 1st moment (mean of gradients)
v ← β₂·v + (1-β₂)·(∇L)²     ← 2nd moment (variance of gradients)
m̂ = m/(1-β₁ᵗ)               ← bias correction
v̂ = v/(1-β₂ᵗ)
w ← w - η·m̂ / (√v̂ + ε)
```
Default hyperparameters: β₁=0.9, β₂=0.999, ε=1e-8, η=1e-3.

**Adam is the default choice for most deep learning tasks** — handles sparse gradients, adapts per-parameter, insensitive to learning rate scale.

### Comparison

| Optimiser | Adaptive LR | Momentum | Best For |
|---|---|---|---|
| SGD | No | Optional | Simple problems, fine-tuning LLMs |
| SGD + Momentum | No | Yes | CV training (often better final acc than Adam) |
| RMSProp | Yes | No | RNNs, non-stationary |
| Adam | Yes | Yes | Default; NLP, most deep learning |
| AdamW | Yes | Yes | LLM pre-training (Adam + weight decay fix) |
| Lion | No | Yes | LLM fine-tuning (memory-efficient) |

**When SGD with momentum beats Adam:** Convolutional networks trained to convergence often achieve slightly better generalisation with tuned SGD+momentum. Adam converges faster but can generalise worse ("sharp minima" hypothesis).

### Learning Rate Scheduling

```
Constant LR           → simple baseline
Step Decay            → lr *= 0.1 every N epochs
Exponential Decay     → lr = lr₀ · e^(-kt)
Cosine Annealing      → lr = lr_min + (lr_max - lr_min) · (1 + cos(π·t/T))/2
Warmup + Cosine       → standard for LLMs; avoid large gradients at start
Cyclical LR           → oscillate between lr_min and lr_max; helps escape saddle points
OneCycleLR            → fast warmup to high LR, then cosine decay; great for fine-tuning
```

---

## 11. Neural Networks — Forward Pass & Backpropagation

> **Interview Q:** *Derive backpropagation. Explain the chain rule in the context of neural networks.*

### Forward Pass

```
Input: x
Layer 1: z₁ = W₁x + b₁;  a₁ = σ(z₁)
Layer 2: z₂ = W₂a₁ + b₂; a₂ = σ(z₂)
Output:  ŷ = a₂
Loss:    L = loss(y, ŷ)
```

### Backpropagation — Chain Rule

Backprop computes the gradient of loss w.r.t. every weight using the **chain rule** of calculus. Work backwards from the loss:

```
∂L/∂W₂ = ∂L/∂ŷ · ∂ŷ/∂z₂ · ∂z₂/∂W₂
        = δ₂ · a₁ᵀ          (where δ₂ = ∂L/∂z₂ is the "error signal" at layer 2)

∂L/∂W₁ = ∂L/∂a₁ · ∂a₁/∂z₁ · ∂z₁/∂W₁
        = (W₂ᵀδ₂) ⊙ σ'(z₁) · xᵀ     (⊙ = element-wise product)
```

**Key insight:** Error signals propagate backwards through the network by multiplying with weight matrices and element-wise activation derivatives. This is why the algorithm is called "backpropagation."

**Weight update (gradient descent):**
```
W ← W - η · ∂L/∂W
b ← b - η · ∂L/∂b
```

### Vanishing & Exploding Gradients

**Vanishing gradient:** As you backpropagate through many layers, gradients can shrink exponentially (especially with sigmoid/tanh where derivative ≤ 0.25). Deep layers receive near-zero gradients → don't learn.

**Exploding gradient:** Gradients grow exponentially → NaN weights. Common in RNNs with long sequences.

**Solutions:**

| Problem | Solution |
|---|---|
| Vanishing | ReLU activations, residual connections, LSTM/GRU, batch norm, careful weight init |
| Exploding | Gradient clipping (`clip_grad_norm_`), weight regularisation |

### Weight Initialisation

Bad initialisation → vanishing/exploding gradients from the start.

| Method | Formula | Use with |
|---|---|---|
| Zero | w = 0 | ❌ Symmetry breaking fails |
| Xavier/Glorot | `N(0, 2/(nᵢₙ+nₒᵤₜ))` | Sigmoid, Tanh |
| He (Kaiming) | `N(0, 2/nᵢₙ)` | ReLU (most common) |
| Orthogonal | Orthogonal matrix | RNNs |

Xavier/He initialisation ensures signal variance remains constant across layers on average.

---

## 12. Activation Functions

> **Interview Q:** *Why do we need activation functions? Compare sigmoid, tanh, ReLU, and GELU.*

Without activation functions, a stack of linear layers = one linear transformation. Non-linear activations enable the network to approximate any function (Universal Approximation Theorem).

### Activation Function Comparison

**Sigmoid:**
```
σ(x) = 1 / (1 + e^(-x))     range: (0, 1)
σ'(x) = σ(x)(1 - σ(x))      max derivative = 0.25
```
Problems: Saturates (derivative ≈ 0 for large |x|) → vanishing gradients. Not zero-centred → slow convergence. Expensive (exponential). **Use only in output layer for binary classification.**

**Tanh:**
```
tanh(x) = (eˣ - e⁻ˣ)/(eˣ + e⁻ˣ)    range: (-1, 1)
tanh'(x) = 1 - tanh²(x)               max derivative = 1
```
Zero-centred (better than sigmoid). Still saturates. Often used in RNNs.

**ReLU (Rectified Linear Unit):**
```
ReLU(x) = max(0, x)
ReLU'(x) = 1 if x > 0, else 0
```
No saturation for positive values. Sparse activation (many zeros). Fast. **Default choice for hidden layers.**

**Problem: Dying ReLU** — neurons with negative inputs always output 0 and receive zero gradient → permanently dead. Mitigations: Leaky ReLU, careful initialisation, lower learning rates.

**Leaky ReLU:** `max(αx, x)` where α=0.01. Small gradient for negative inputs → prevents dying.

**GELU (Gaussian Error Linear Unit):**
```
GELU(x) = x · Φ(x)    where Φ is the CDF of standard normal
≈ 0.5x(1 + tanh(√(2/π)(x + 0.044715x³)))
```
Smooth, non-monotonic. **Used in BERT, GPT, and most modern transformers.** Empirically outperforms ReLU in transformers.

**SiLU / Swish:** `x · sigmoid(x)`. Similar to GELU; used in Llama, Mistral, Gemma.

**Softmax (output only):**
```
softmax(xᵢ) = exp(xᵢ) / Σⱼ exp(xⱼ)    range: (0,1), sums to 1
```
Converts logits to probability distribution. Used in multi-class output layer. Not used in hidden layers (saturates badly).

---

## 13. Loss Functions

> **Interview Q:** *When would you use cross-entropy loss vs MSE?*

### Regression Losses

| Loss | Formula | Properties |
|---|---|---|
| MSE | `(1/n)Σ(y-ŷ)²` | Sensitive to outliers; differentiable everywhere |
| MAE | `(1/n)Σ|y-ŷ|` | Robust to outliers; non-differentiable at 0 |
| Huber | MSE if `|e|<δ`, MAE otherwise | Best of both — use when outliers present |
| Log-Cosh | `log(cosh(y-ŷ))` | Smooth Huber approximation |

### Classification Losses

**Binary Cross-Entropy:**
```
L = -(y·log(ŷ) + (1-y)·log(1-ŷ))
```
For binary classification with sigmoid output.

**Categorical Cross-Entropy:**
```
L = -Σₖ yₖ · log(ŷₖ)
```
For multi-class with one-hot labels and softmax output.

**Sparse Categorical Cross-Entropy:** Same as above but labels are integers (not one-hot). More memory efficient.

**KL Divergence:**
```
KL(P||Q) = Σ P(x) · log(P(x)/Q(x))
```
Measures how much distribution Q diverges from reference P. Used in VAEs, knowledge distillation, RLHF (KL penalty term).

**Focal Loss:**
```
FL(pₜ) = -(1 - pₜ)^γ · log(pₜ)
```
Down-weights easy examples, focuses training on hard ones. Used in object detection (RetinaNet). Great for extreme class imbalance.

### Why Cross-Entropy (not MSE) for Classification?

1. **Convexity:** Cross-entropy with softmax is convex → single global minimum. MSE with softmax is non-convex.
2. **Gradient signal:** Cross-entropy gradient is `ŷ - y` — large gradient when model is wrong. MSE gradient also includes the sigmoid/softmax derivative, which saturates → vanishing gradient problem.
3. **Probabilistic interpretation:** Cross-entropy is the negative log-likelihood of the data under the model — principled probabilistic objective (MLE).

---

## 14. Regularisation in Deep Learning

> **Interview Q:** *What is dropout and how does it prevent overfitting?*

### Dropout

During training, randomly set a fraction p of neuron activations to 0 in each forward pass. At inference, use all neurons but scale weights by (1-p).

```python
# PyTorch
nn.Dropout(p=0.5)   # 50% neurons dropped each forward pass
```

**Why it works:**
1. Forces network to not rely on any single neuron — learns redundant representations
2. Equivalent to training an ensemble of 2^n thinned networks sharing weights
3. Acts as noise injection — the model must learn robust features

**Typical rates:** 0.5 for hidden layers; 0.1–0.3 for input/output; 0.1–0.2 for transformers.

**Dropout at test time:** Disabled (all neurons active). Weights implicitly scaled so expected output matches training.

**Inverted Dropout (modern standard):** Scale UP activations during training by 1/p so no scaling needed at test time.

### Data Augmentation

Create synthetic training examples by applying transformations that preserve labels:

**Computer Vision:** random crop, horizontal flip, colour jitter, rotation, cutout, mixup, cutmix, RandAugment.

**NLP:** back-translation, synonym replacement, random insertion/deletion, EDA (Easy Data Augmentation).

**Why it works:** Increases effective dataset size; teaches invariance to transformations; explicit inductive biases (flipped cat is still a cat).

### Early Stopping

Monitor validation loss during training. Stop when validation loss stops improving (patience=N epochs). Save the model at best validation performance.

**Intuition:** Training loss always decreases. Validation loss decreases then increases (overfitting starts). Early stopping captures the sweet spot.

### Weight Decay (L2 in neural networks)

Add L2 penalty to loss. In Adam, standard L2 doesn't work correctly because the adaptive learning rate decouples weight decay from gradient updates. **AdamW** decouples weight decay — applies it directly to weights, not through the gradient — the correct way to do L2 regularisation with Adam.

### Label Smoothing

Instead of one-hot targets [0, 0, 1, 0], use [ε/K, ε/K, 1-ε+ε/K, ε/K]:

```
Smoothed label = (1 - ε) · hard_label + ε/K
```

Prevents overconfident predictions. Improved calibration. Standard in image classification (ε=0.1) and LLM training.

---

## 15. Convolutional Neural Networks (CNN)

> **Interview Q:** *Explain convolution, pooling, and why CNNs work for images.*

### Why Not Fully Connected for Images?

224×224×3 image = 150,528 input neurons. A single FC layer with 1000 units = 150M+ parameters — massively overfit, no spatial inductive bias.

**CNN inductive biases (why they work for images):**
1. **Translation equivariance:** A cat in the top-left and bottom-right both trigger the same "cat detector" filter
2. **Local connectivity:** Nearby pixels are more related than distant ones
3. **Weight sharing:** Same filter applied everywhere → drastically reduces parameters

### Convolution Operation

A filter (kernel) of size k×k slides over the input, computing a dot product at each position:

```
Output[i,j] = Σₘ Σₙ Input[i+m, j+n] · Filter[m,n] + bias
```

**Key parameters:**
- **Filter size:** 3×3 (most common), 5×5, 1×1
- **Stride:** How many pixels to move per step (stride=2 → halves spatial dimensions)
- **Padding:** "same" padding adds zeros to preserve spatial size; "valid" = no padding
- **Output size:** `(W - F + 2P) / S + 1`  (W=input size, F=filter, P=padding, S=stride)
- **Number of filters:** Each filter learns a different feature detector → output has one channel per filter

**1×1 Convolution:** Projects channel dimension without spatial mixing — used for channel reduction (bottleneck layers in ResNet, Inception).

### Pooling

Reduces spatial dimensions, introduces translation invariance:

- **Max pooling:** Take max in each window → preserves dominant features
- **Average pooling:** Take mean → smooths features
- **Global Average Pooling (GAP):** Average each channel to a single value → replaces FC layers in modern architectures

### Classic CNN Architectures

| Model | Year | Key Innovation |
|---|---|---|
| LeNet-5 | 1998 | First practical CNN |
| AlexNet | 2012 | Deep CNN on GPU; ReLU; dropout |
| VGG-16/19 | 2014 | Very deep with 3×3 filters only |
| GoogLeNet (Inception) | 2014 | Inception modules; 1×1 convolutions |
| ResNet | 2015 | Residual connections → 100+ layer networks |
| DenseNet | 2017 | Dense connections (all layers connected) |
| EfficientNet | 2019 | Compound scaling (width, depth, resolution) |
| ConvNeXt | 2022 | "Modernised" CNN matching ViT performance |

### Residual Connections (Skip Connections)

The key innovation of ResNet — enables training very deep networks (50, 101, 152 layers):

```
Output = F(x, weights) + x    ← skip connection adds input directly to output
```

**Why it works:**
1. Provides gradient highways — gradients flow directly from loss to early layers via the skip path
2. Makes it easy for layers to learn "identity" (just output 0 in F) — safe to add more layers
3. Mitigates vanishing gradient problem in deep networks

---

## 16. Recurrent Neural Networks — RNN, LSTM, GRU

> **Interview Q:** *What is the vanishing gradient problem in RNNs and how do LSTMs solve it?*

### Vanilla RNN

Processes sequences by maintaining a hidden state:
```
hₜ = tanh(Wₕ·hₜ₋₁ + Wₓ·xₜ + b)
yₜ = Wy·hₜ
```

**Backpropagation Through Time (BPTT):** Unroll the RNN for T steps → compute gradients. Gradient of loss w.r.t. early hidden states involves multiplying Wₕ T times:

```
∂L/∂h₁ = ∂L/∂hₜ · (Wₕ)^(T-1)
```

If ||Wₕ|| < 1: gradients → 0 (vanishing).
If ||Wₕ|| > 1: gradients → ∞ (exploding).

**Result:** Vanilla RNNs can't capture long-range dependencies.

### LSTM (Long Short-Term Memory)

Adds a **cell state** Cₜ — a "conveyor belt" for long-term memory — controlled by three gates:

```
Forget gate:  fₜ = σ(Wf · [hₜ₋₁, xₜ] + bf)     ← what to forget from cell state
Input gate:   iₜ = σ(Wᵢ · [hₜ₋₁, xₜ] + bᵢ)     ← what new info to add
Candidate:    C̃ₜ = tanh(Wc · [hₜ₋₁, xₜ] + bc)   ← new candidate values
Cell update:  Cₜ = fₜ⊙Cₜ₋₁ + iₜ⊙C̃ₜ             ← update cell state
Output gate:  oₜ = σ(Wo · [hₜ₋₁, xₜ] + bo)      ← what to output
Hidden state: hₜ = oₜ ⊙ tanh(Cₜ)
```

**Why LSTM solves vanishing gradient:**
The cell state Cₜ = fₜ⊙Cₜ₋₁ + iₜ⊙C̃ₜ allows the gradient to flow through the additive cell update (addition → gradient passes unchanged). The forget gate fₜ ≈ 1 means "remember everything" → gradient ≈ 1 → no decay over many steps.

### GRU (Gated Recurrent Unit)

Simplified LSTM — two gates instead of three:
```
Reset gate:  rₜ = σ(Wr · [hₜ₋₁, xₜ])
Update gate: zₜ = σ(Wz · [hₜ₋₁, xₜ])
Candidate:   h̃ₜ = tanh(W · [rₜ⊙hₜ₋₁, xₜ])
Hidden:      hₜ = (1 - zₜ)⊙hₜ₋₁ + zₜ⊙h̃ₜ
```

No separate cell state — merges hidden and cell states. ~33% fewer parameters than LSTM. Often comparable performance; faster to train.

| | RNN | LSTM | GRU |
|---|---|---|---|
| Gates | None | 3 (forget, input, output) | 2 (reset, update) |
| Memory | Hidden state | Hidden + cell state | Hidden state |
| Long-range | Poor | Good | Good |
| Speed | Fastest | Slowest | Fast |
| Parameters | Fewest | Most | Fewer than LSTM |
| When to use | Simple sequences | Complex dependencies | Balance speed/quality |

**Modern reality:** For most NLP tasks, Transformers have replaced RNNs/LSTMs. RNNs still appear in: low-latency streaming applications, time series (especially short ones), resource-constrained environments.

---

## 17. Batch Normalisation & Layer Normalisation

> **Interview Q:** *What is batch normalisation? Why is it used? How does layer norm differ?*

### Batch Normalisation (BatchNorm)

Normalise activations within a mini-batch to have zero mean and unit variance, then apply learned scale (γ) and shift (β):

```
μ_B = (1/m) · Σxᵢ          ← batch mean
σ²_B = (1/m) · Σ(xᵢ - μ_B)² ← batch variance
x̂ᵢ = (xᵢ - μ_B) / √(σ²_B + ε)  ← normalise
yᵢ = γ · x̂ᵢ + β           ← scale and shift (learned)
```

**Benefits:**
1. Reduces internal covariate shift — stabilises distribution of activations
2. Allows higher learning rates → faster training
3. Slight regularisation effect (batch statistics add noise)
4. Reduces sensitivity to weight initialisation

**Limitations:**
- Depends on batch size — unstable with small batches (batch size < 8)
- Different behaviour at train vs. inference (uses running statistics at inference)
- Doesn't work for variable-length sequences (RNNs, Transformers)

**At inference:** Uses exponential moving average of training batch statistics (accumulated during training) instead of current batch.

### Layer Normalisation (LayerNorm)

Normalise across the **feature dimension** (all neurons in a layer) for a single example:

```
μ = mean over all features of single example
σ² = variance over all features of single example
x̂ = (x - μ) / √(σ² + ε)
y = γ · x̂ + β
```

**Key difference from BatchNorm:** Computed per sample, not per batch — independent of batch size, same at train and inference.

**Used in Transformers (all modern LLMs):** Variable-length sequences, works with any batch size, no train-inference discrepancy.

**RMSNorm:** Like LayerNorm but skips the mean subtraction:
```
x̂ = x / RMS(x) · γ    where RMS(x) = √(mean(x²))
```
Faster and simpler. Used in Llama 2/3, Mistral, Gemma.

| | BatchNorm | LayerNorm | RMSNorm |
|---|---|---|---|
| Normalise over | Batch (per feature) | Features (per sample) | Features (per sample) |
| Batch size dep | Yes | No | No |
| Train vs Infer | Different | Same | Same |
| Used in | CNNs | Transformers/RNNs | Modern LLMs (Llama) |

---

## 18. Transfer Learning & Fine-Tuning

> **Interview Q:** *What is transfer learning? When do you fine-tune all layers vs freeze most?*

Transfer learning uses a model pre-trained on a large dataset (source task) as a starting point for a new task (target task), rather than training from scratch.

**Why it works:** Lower layers learn general features (edges, textures, basic syntax) that transfer across tasks. Only upper layers need to specialise.

### Strategies

**Feature Extraction (Freeze all, train classifier head):**
- Freeze all pre-trained weights
- Add a new output layer (classification head)
- Train only the new head
- Use when: small target dataset + similar domain to source

**Fine-Tuning (Unfreeze some/all layers):**
- Unfreeze top N layers (or all)
- Train with a very small learning rate (1e-5 to 1e-4)
- Use when: target dataset is moderately sized or domain differs slightly

**Full Fine-Tuning:**
- Unfreeze all layers
- Risk of catastrophic forgetting — mitigate with small LR + layer-wise LR decay
- Use when: large target dataset, significant domain shift

**Layer-wise LR decay:** Lower layers get even smaller LR (they've learned general features that shouldn't change much):
```
LR for layer L = base_LR × decay^(total_layers - L)
```

### Pre-trained Models for Common Tasks

| Domain | Pre-trained Model | Fine-tune for |
|---|---|---|
| NLP | BERT, RoBERTa | Classification, NER, QA |
| NLP (generation) | GPT-2, Llama, Mistral | Text generation, chatbots |
| Computer Vision | ResNet, EfficientNet | Classification, detection |
| Vision-Language | CLIP | Zero-shot classification, image search |
| Speech | Whisper | ASR, translation |

---

## 19. Autoencoders & VAEs

> **Interview Q:** *What is a Variational Autoencoder and how does it differ from a standard autoencoder?*

### Autoencoder

Two-part network: Encoder compresses input to a latent code z; Decoder reconstructs input from z.

```
Input x → Encoder → z (bottleneck) → Decoder → x̂
Loss: Reconstruction loss = ||x - x̂||²
```

**Uses:** Dimensionality reduction, denoising (train on noisy input, reconstruct clean), anomaly detection (high reconstruction error = anomaly), representation learning.

**Limitation:** Latent space is not structured — you can't sample from it to generate new examples.

### Variational Autoencoder (VAE)

Instead of encoding to a point z, encode to a distribution N(μ, σ²):

```
Encoder → μ, log σ²
z = μ + σ · ε    where ε ~ N(0, I)   ← reparameterisation trick
Decoder → x̂

Loss = Reconstruction Loss + KL Divergence
     = ||x - x̂||² + KL(N(μ,σ²) || N(0,1))
```

**KL term:** Forces the latent distribution to stay close to N(0,1) → structured, continuous latent space. You can sample z ~ N(0,1) and decode to generate new samples.

**Reparameterisation trick:** Instead of sampling z (non-differentiable), express z = μ + σ·ε where ε is sampled from fixed N(0,1). Now gradients can flow through μ and σ.

**Uses:** Generative modelling, interpolation in latent space, disentangled representation learning, image generation (though diffusion models now dominate).

---

## 20. GANs — Generative Adversarial Networks

> **Interview Q:** *How do GANs work? What are the common training instabilities?*

### Architecture

Two networks trained adversarially:

```
Generator G(z):    Takes random noise z → generates fake data
Discriminator D(x): Takes data → outputs P(real | x)

D tries to distinguish real from fake.
G tries to fool D into classifying its output as real.
```

**Minimax objective:**
```
min_G max_D E[log D(x)] + E[log(1 - D(G(z)))]
```
At equilibrium: G produces indistinguishable fakes; D outputs 0.5 everywhere.

### Training Instabilities

**Mode collapse:** Generator learns to produce only a few examples (ignores diversity in z). The discriminator can't distinguish them. Mitigation: minibatch discrimination, Wasserstein loss.

**Vanishing gradient:** When D is too good early → G receives near-zero gradients → G can't learn. Use non-saturating loss: G maximises log(D(G(z))) instead of minimising log(1-D(G(z))).

**Training instability:** GAN training is a two-player game — oscillations common. Mitigation: balanced training (more D steps per G step), lower LR, gradient penalty.

**Wasserstein GAN (WGAN):** Replaces JS divergence with Wasserstein distance — smoother gradients, more stable training. Requires weight clipping or gradient penalty (WGAN-GP).

### Notable GAN Variants

| Model | Innovation |
|---|---|
| DCGAN | Convolutional architecture for images |
| CycleGAN | Image-to-image translation without paired data |
| StyleGAN | High-quality face synthesis; latent style control |
| Pix2Pix | Paired image-to-image translation |
| SRGAN | Super resolution |

**Modern reality:** Diffusion models have largely surpassed GANs for image generation (more stable training, better diversity, higher quality). GANs still used for video, fast sampling, and specific paired-image tasks.

---

## 21. Attention Mechanism — Deep Dive

> **Interview Q:** *Derive the scaled dot-product attention formula. Why is scaling needed?*

### The Intuition

Attention is a mechanism for a query to selectively retrieve information from a set of key-value pairs. Think of a search engine: query is what you're looking for, keys are what's indexed, values are the content. Similarity between query and key determines how much of each value is retrieved.

### Scaled Dot-Product Attention

Each token is projected into three vectors via learned linear transformations:
```
Q = XWQ    (Queries — what am I looking for?)
K = XWK    (Keys — what do I contain?)
V = XWV    (Values — what do I output if selected?)

Attention(Q, K, V) = softmax(QKᵀ / √dₖ) · V
```

**Step by step:**
1. `QKᵀ`: dot product of each query with all keys → raw attention scores (n×n matrix)
2. `/ √dₖ`: scale by square root of key dimension
3. `softmax(...)`: normalise scores to sum to 1 → attention weights
4. `· V`: weighted sum of value vectors

**Why √dₖ scaling?** With large dₖ, dot products grow large → softmax saturates (near 1 for max, near 0 for others) → gradient of softmax ≈ 0 → vanishing gradient. Dividing by √dₖ keeps dot products in a reasonable range.

**Complexity:** O(n²d) time and O(n²) space for storing the attention matrix — the bottleneck for long contexts.

### Multi-Head Attention

Instead of one attention pass, run h heads with different projections:

```
head_i = Attention(QW_i^Q, KW_i^K, VW_i^V)
MultiHead(Q,K,V) = Concat(head₁,...,headₕ) · W^O
```

**Why multiple heads?** Each head can attend to different relationship types simultaneously:
- Head 1 might track syntactic dependencies
- Head 2 might track co-reference
- Head 3 might track positional proximity

Different heads learn different subspaces of the representation.

### Self-Attention vs Cross-Attention

**Self-attention:** Q, K, V all come from the same sequence. Each token attends to all other tokens in the same sequence. Used in encoder (full self-attention) and decoder (masked self-attention).

**Cross-attention:** Q comes from one sequence (e.g., decoder), K and V come from another (e.g., encoder output). Lets the decoder "look at" the encoder's representation. Used in encoder-decoder models.

**Causal (masked) self-attention:** Future tokens are masked out — token i can only attend to tokens 1..i. Essential for autoregressive language generation.

---

## 22. Transformer Architecture

> **Interview Q:** *Walk me through the full Transformer architecture.*

```
INPUT TOKENS
     ↓
[Token Embedding + Positional Encoding]
     ↓
┌──────────────────────────────────────────┐
│  ENCODER BLOCK × N  (used in BERT)      │
│                                          │
│  ┌─────────────────────────────────┐    │
│  │ Multi-Head Self-Attention       │    │
│  │ (bidirectional — sees all)      │    │
│  └──────────┬──────────────────────┘    │
│             │ + Residual                 │
│  LayerNorm(x + Attention(x))            │
│             ↓                           │
│  ┌─────────────────────────────────┐    │
│  │ Feed-Forward Network            │    │
│  │ FFN(x) = max(0, xW₁+b₁)W₂+b₂  │    │
│  └──────────┬──────────────────────┘    │
│             │ + Residual                 │
│  LayerNorm(x + FFN(x))                  │
└──────────────────────────────────────────┘
     ↓
┌──────────────────────────────────────────┐
│  DECODER BLOCK × N  (used in GPT)       │
│                                          │
│  Masked Multi-Head Self-Attention       │
│  (causal — only attends to past)        │
│       ↓                                 │
│  Cross-Attention (enc-dec models only)  │
│       ↓                                 │
│  Feed-Forward Network                   │
└──────────────────────────────────────────┘
     ↓
Linear + Softmax → Probability over vocabulary
```

### Key Architectural Choices

**Residual connections:** `output = LayerNorm(x + sublayer(x))`. Allow gradients to flow directly from loss to early layers. Enable very deep stacking (96 layers in GPT-3).

**Pre-LN vs Post-LN:**
- Original Transformer: Post-LN (LayerNorm after residual add) — unstable for deep models
- Modern LLMs: Pre-LN (LayerNorm before each sublayer) — more stable, better gradient flow

**FFN dimension:** Typically 4× the model dimension. If d_model=1024, FFN hidden dim = 4096.

**Model size calculation (rule of thumb):**
```
Parameters ≈ 12 · n_layers · d_model²
(for standard Transformer without embedding)
```

---

## 23. BERT — Encoder-Only Models

> **Interview Q:** *How is BERT pre-trained? What makes it bidirectional?*

### BERT Architecture

Encoder-only Transformer — uses full bidirectional self-attention. Every token can attend to every other token in both directions simultaneously.

**BERT-base:** 12 layers, 768 hidden dim, 12 attention heads, 110M parameters.
**BERT-large:** 24 layers, 1024 hidden dim, 16 attention heads, 340M parameters.

### Pre-training Tasks

**1. Masked Language Modelling (MLM):**
- Randomly mask 15% of tokens
- Of those: 80% replaced with [MASK], 10% replaced with random token, 10% unchanged
- Model predicts the original token at masked positions
- Enables bidirectional context — model sees both left and right of masked token

```
Input:  "The [MASK] sat on the [MASK]"
Target: "The cat sat on the mat"
```

**2. Next Sentence Prediction (NSP):**
- Given two sentences A and B, predict if B follows A in the original text (50% yes, 50% random)
- Teaches sentence-level relationships
- Later shown to be less useful — RoBERTa removes it

### Fine-Tuning BERT

Add a task-specific head on top of [CLS] token representation:

```
Classification: [CLS] repr → linear → softmax → class probabilities
NER:            Each token repr → linear → softmax → entity label
QA:             Each token → start/end logits → span extraction
```

### BERT Variants

| Model | Key Difference |
|---|---|
| RoBERTa | More data, longer training, no NSP, dynamic masking. Better than BERT. |
| DistilBERT | 40% smaller, 60% faster via knowledge distillation, retains 97% of BERT |
| ALBERT | Parameter-sharing across layers, factorised embeddings. Smaller, comparable quality |
| DeBERTa | Disentangled attention on content and position separately. Top NLU benchmark |
| XLM-RoBERTa | Multilingual RoBERTa (100 languages) |

---

## 24. GPT — Decoder-Only Models

> **Interview Q:** *How is GPT different from BERT? What is autoregressive generation?*

### GPT Architecture

Decoder-only Transformer — uses **causal (masked) self-attention**. Each token can only attend to itself and previous tokens. No cross-attention (no encoder).

**Pre-training objective: Next-Token Prediction (Causal Language Modelling)**
```
Predict token tᵢ given all previous tokens t₁, t₂, ..., tᵢ₋₁

Loss = -Σᵢ log P(tᵢ | t₁, ..., tᵢ₋₁)
```

This single objective, applied at scale, causes the model to learn grammar, facts, reasoning, and emergent capabilities.

### Autoregressive Generation

At inference, GPT generates one token at a time:
```
1. Feed prompt tokens → get logits for next token
2. Sample next token from logits (using temperature, top-p)
3. Append sampled token to input
4. Repeat until [EOS] or max length
```

**KV Cache:** At each step, K and V for all previous tokens are cached — reuse without recomputation. Without KV cache: O(n²) per token. With KV cache: O(n) per token.

### GPT Family Timeline

| Model | Params | Context | Key Note |
|---|---|---|---|
| GPT-1 | 117M | 512 | Proved pre-train + fine-tune works for NLP |
| GPT-2 | 1.5B | 1024 | "Too dangerous to release" — now used everywhere |
| GPT-3 | 175B | 2048 | In-context learning; few-shot without fine-tuning |
| InstructGPT | 1.3B | 2048 | RLHF alignment → ChatGPT origin |
| GPT-4 | ~1T MoE? | 128K | Multimodal, frontier capability |

### BERT vs GPT Summary

| | BERT | GPT |
|---|---|---|
| Architecture | Encoder-only | Decoder-only |
| Attention | Bidirectional | Causal (left-to-right only) |
| Pre-training | MLM + NSP | Next-token prediction |
| Generation | Poor | Excellent (autoregressive) |
| Understanding | Excellent | Good (improves with scale) |
| Fine-tune for | Classification, NER, QA | Generation, chat, code |

---

## 25. T5, BART — Encoder-Decoder Models

> **Interview Q:** *When would you use an encoder-decoder model vs encoder-only vs decoder-only?*

### T5 (Text-To-Text Transfer Transformer)

**Key idea:** Frame ALL NLP tasks as text-to-text:
```
Translation:    Input: "translate English to French: The house is nice"
                Output: "La maison est belle"
Summarisation:  Input: "summarise: [long article]"
                Output: "[short summary]"
Classification: Input: "sentiment: This movie was great"
                Output: "positive"
```

**Architecture:** Standard encoder-decoder Transformer. Encoder sees full input bidirectionally; decoder generates output autoregressively.

**Variants:** T5-small (60M) to T5-11B. FLAN-T5 = T5 fine-tuned on 1800+ tasks with instruction prompts.

### BART (Bidirectional Auto-Regressive Transformer)

BERT-like encoder + GPT-like decoder. Pre-trained with denoising objectives:
- Text infilling (mask spans of text)
- Sentence shuffling
- Token deletion

**Best for:** Summarisation (CNN/DailyMail SOTA), translation, dialogue.

### Architecture Comparison — When to Use Each

| | Encoder-only (BERT) | Decoder-only (GPT) | Encoder-Decoder (T5) |
|---|---|---|---|
| **Best for** | Understanding tasks | Generation tasks | Conditional generation |
| **Tasks** | Classification, NER, QA, embeddings | Chat, code, open generation | Translation, summarisation, QA |
| **Context** | Full bidirectional | Causal | Input bidirectional, output causal |
| **Efficiency** | Efficient for fixed-output | Can generate any length | Heavier (two stacks) |

---

## 26. Positional Encoding — Sinusoidal, RoPE, ALiBi

> **Interview Q:** *Why do Transformers need positional encoding? What is RoPE?*

Transformers process all tokens in parallel — without explicit position information, "dog bites man" and "man bites dog" would be identical. Positional encodings inject order information.

### Sinusoidal PE (Original Transformer)

Fixed, non-learned. Added to token embeddings:
```
PE(pos, 2i)   = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))
```
Different frequency sine/cosine for each dimension. Can extrapolate to longer sequences (never seen during training) due to continuous nature.

### Learned Absolute PE

Treat position as a feature with a learned embedding table (same as token embedding). Simpler but can't extrapolate beyond training length.

### RoPE (Rotary Position Embedding)

Used in: Llama 2/3, Mistral, Falcon, Gemma, PaLM.

**Key idea:** Instead of adding position to embeddings, rotate Q and K vectors by an angle proportional to position. The dot product QᵢKⱼᵀ then naturally encodes the relative position (j - i).

```
Rotate q and k by position-dependent angle θ:
q̃ = q · R(mθ)    (m = position)
k̃ = k · R(nθ)
q̃ · k̃ᵀ = q · Rᵀ((m-n)θ) · k  → depends only on relative position (m-n)
```

**Advantages:** Encodes relative position (not absolute), better length generalisation, compatible with efficient attention.

### ALiBi (Attention with Linear Biases)

Instead of positional embeddings, add a fixed linear penalty to attention scores based on distance between tokens:
```
Attention score = QKᵀ/√dₖ - m·|i - j|
```
Where m is a per-head slope. Further apart tokens get penalised more. Simple, trains on short contexts, generalises well to longer contexts at inference.

---

## 27. Efficient Transformers — Flash Attention, MoE, GQA

> **Interview Q:** *What is Flash Attention and why does it matter?*

### Flash Attention

Standard attention stores the full n×n attention matrix in HBM (slow GPU memory) during the forward pass:
- Memory: O(n²)
- Slow: many HBM read/write operations

**Flash Attention** reorders the computation to avoid materialising the full attention matrix. Uses tiling: processes small blocks of Q, K, V at a time, keeping intermediate results in fast SRAM:

```
Memory: O(n) instead of O(n²)
Speed:  2-4× faster on long sequences
Output: Mathematically identical to standard attention
```

Flash Attention 2 adds further optimisations (better work partitioning, fewer non-matmul ops). Now the standard in all production LLMs.

### Grouped Query Attention (GQA)

Standard Multi-Head Attention (MHA): Each head has its own Q, K, V projections.

**Problem:** KV cache scales as O(n_heads × seq_len × d_head) — expensive for long contexts.

**Multi-Query Attention (MQA):** All heads share a single K, V. Reduces KV cache by n_heads× but can hurt quality.

**Grouped Query Attention (GQA):** Multiple query heads share a group of K, V heads (e.g., 32 query heads share 8 K, V heads — ratio 4:1). Balance between MHA quality and MQA efficiency.

Used in: Llama 3, Mistral, Gemma 2, Falcon.

### Mixture of Experts (MoE)

Instead of one FFN applied to every token, have E expert FFNs and a learned router that selects the top-k experts per token:

```
Router(x) → select top-k experts
Output = Σ_{k in selected} gate_score_k · Expert_k(x)
```

**Benefit:** Total parameters scale (more capacity), active parameters (compute) stays fixed. Mixtral 8×7B has 46.7B total params but only 12.9B active per token → GPT-3.5-level quality at ~3× lower inference cost.

**Challenges:** Load balancing (avoid routing all tokens to same expert), communication overhead in multi-GPU settings.

---

## 28. Vision Transformers (ViT)

> **Interview Q:** *How does ViT apply Transformers to images?*

### Architecture

```
Image (224×224×3)
     ↓
Split into 16×16 patches → 196 patches (196 = (224/16)²)
     ↓
Flatten each patch → 768-dim vector (16×16×3)
Linear projection → patch embeddings
     ↓
Prepend [CLS] token → 197 tokens total
Add position embeddings
     ↓
Standard Transformer Encoder × N
     ↓
[CLS] token output → classification head
```

**Key insight:** Treat image patches as "tokens" in a sequence. Self-attention captures global dependencies from the start (unlike CNNs which build up receptive field gradually through many layers).

**ViT requirements:** Large-scale pre-training (>ImageNet) needed to match CNN performance because Transformers lack CNN's inductive biases (translation equivariance, locality). Pre-train on JFT-300M → fine-tune → beats ResNet.

### ViT Variants

| Model | Innovation |
|---|---|
| DeiT | Data-efficient ViT; knowledge distillation from CNN teacher |
| Swin Transformer | Hierarchical ViT with shifted windows; local attention |
| MAE (Masked Autoencoder) | Pre-train by masking 75% of patches; reconstruct — like BERT for images |
| DINO | Self-supervised ViT with knowledge distillation; emergent segmentation |
| SAM (Segment Anything) | ViT encoder for universal segmentation |

---

## 29. Interview Q&A — Classical ML

**Q: What is the difference between supervised and unsupervised learning?**
> Supervised learning trains on labelled data (input-output pairs) to predict outputs for new inputs. The learning signal is the error between predictions and true labels. Unsupervised learning finds structure in data without labels — clustering groups similar points, dimensionality reduction compresses representations, density estimation models data distributions. Semi-supervised leverages both small labelled and large unlabelled sets.

**Q: Explain overfitting and how you detect and fix it.**
> Overfitting occurs when a model learns the training data too well — including noise — and fails to generalise. Detection: training accuracy much higher than validation accuracy; validation loss increasing while training loss decreases. Fixes: (1) More data; (2) Simpler model; (3) Regularisation (L1/L2, dropout); (4) Data augmentation; (5) Early stopping; (6) Cross-validation to properly estimate generalisation.

**Q: What is the difference between bagging and boosting?**
> Bagging (e.g., Random Forest): train independent models on bootstrap samples in parallel, aggregate predictions. Reduces variance; base learners are deep/high-variance trees. Boosting (e.g., XGBoost): train models sequentially, each correcting the previous's errors. Reduces bias; base learners are shallow trees. Boosting generally achieves higher accuracy on tabular data but is slower to train and more prone to overfitting.

**Q: How does gradient boosting work?**
> At each stage, fit a new model to the residuals (negative gradient of the loss) from the previous ensemble. Add this model to the ensemble with a learning rate. Repeat for n_estimators iterations. Conceptually: gradient descent in function space — each new model is a step in the direction that most reduces the loss.

**Q: What is cross-validation and why is it important?**
> Cross-validation estimates how well a model will generalise to independent data. In k-fold CV, data is split into k folds; the model is trained k times, each time holding out one fold for evaluation. Report mean ± std of scores. Critical because: single train-test split has high variance (depends on which data ends up in test set); CV gives a more reliable estimate of true generalisation error and catches overfitting during hyperparameter tuning.

**Q: Why is accuracy not always a good metric?**
> For imbalanced datasets (e.g., 99% negative, 1% positive), a model predicting "always negative" gets 99% accuracy while being useless. Better metrics: F1-score (harmonic mean of precision and recall), AUC-ROC (rank-based, threshold-independent), PR-AUC (better than ROC-AUC when positive class is rare). Always choose metrics aligned with the business objective — sometimes false negatives are 10× more costly than false positives.

---

## 30. Interview Q&A — Deep Learning

**Q: Explain backpropagation.**
> Backpropagation is an algorithm to efficiently compute gradients of the loss w.r.t. all parameters using the chain rule. The forward pass computes activations and loss. The backward pass propagates error signals from the output back through the network: at each layer, multiply the incoming error signal by the local derivative (activation derivative) and by the transpose of the weight matrix. Gradients accumulate for each weight. Weight updates follow gradient descent.

**Q: What is the vanishing gradient problem and how is it solved?**
> In deep networks (especially RNNs), gradients shrink exponentially as they propagate backwards — layers near the input receive near-zero gradients and don't learn. Causes: sigmoid/tanh activations (derivatives ≤ 0.25; after many multiplications → 0), deep networks. Solutions: (1) ReLU activations (derivative=1 for positive inputs); (2) Residual connections (skip connections provide gradient bypass); (3) Batch/Layer normalisation (normalises activations to prevent saturation); (4) LSTM/GRU (cell state allows gradient to flow with minimal decay); (5) Good weight initialisation (He init for ReLU).

**Q: Explain dropout and its effect at test time.**
> Dropout randomly deactivates p fraction of neurons during each training forward pass, forcing the network to learn redundant representations and preventing co-adaptation. At test time, dropout is disabled and all neurons are active. To compensate, during training activations are scaled up by 1/(1-p) (inverted dropout) so expected output magnitude matches test time without any scaling adjustment.

**Q: What is batch normalisation and what problems does it solve?**
> Batch normalisation normalises layer inputs to zero mean and unit variance across a mini-batch, then applies learned scale and shift. Solves: (1) Internal covariate shift — distributions of layer inputs keep changing as earlier layers update, making training unstable; (2) Allows higher learning rates; (3) Reduces sensitivity to initialisation; (4) Slight regularisation effect (adds noise from batch statistics). Limitation: behaviour differs at train vs. test time; fails with very small batch sizes.

**Q: When do you use CNN vs RNN vs Transformer?**
> CNN: Spatial data with local patterns (images, audio spectrograms); translation invariance needed; computationally efficient. RNN/LSTM: Sequential data with short-to-medium range dependencies; streaming/online processing; low-latency requirements. Transformer: Long-range dependencies; parallel training on large datasets; state-of-the-art for NLP, increasingly for vision (ViT). In practice (2024+): Transformers dominate NLP; CNNs still competitive in vision (ConvNeXt); RNNs mostly replaced except for streaming/edge cases.

**Q: What is transfer learning and when would you not use it?**
> Transfer learning reuses weights from a pre-trained model on a large source task for a smaller target task. Almost always beneficial when target data is limited or expensive. Cases where you might not use it: (1) Target domain is completely unrelated to any available pre-trained model; (2) You have an enormous amount of target task data (pre-training from scratch may generalise better); (3) Strict latency/size constraints incompatible with pre-trained model sizes; (4) Privacy — pre-trained models may carry memorised training data.

**Q: What is the difference between a generative and discriminative model?**
> Discriminative models learn P(Y|X) — the conditional distribution of labels given inputs. Optimised directly for prediction. Examples: logistic regression, SVM, BERT fine-tuned for classification. Generative models learn P(X) or P(X,Y) — the joint or marginal distribution of data. Can generate new samples. Examples: VAE, GAN, GPT, Naive Bayes. In practice: discriminative models often have better classification accuracy; generative models are needed for synthesis, augmentation, and density estimation.

---

## 31. Interview Q&A — Transformers & NLP

**Q: Explain self-attention from scratch.**
> For each token in the sequence, compute three vectors: Query (what I'm looking for), Key (what I offer), Value (what I output if selected). Attention score between token i and j = dot product of Qᵢ and Kⱼ, scaled by √d_k to prevent saturation, then softmax-normalised. The output for token i is a weighted sum of all Value vectors, weighted by these attention scores. Intuitively: every token can directly attend to every other token — captures arbitrary long-range dependencies in one step.

**Q: Why is scaled dot-product attention scaled by √d_k?**
> Dot products grow in magnitude with dimension d_k — in d_k dimensions, the expected magnitude of the dot product is √d_k. Without scaling, large dot products push softmax into saturation regions where gradients are near zero, making learning slow. Dividing by √d_k keeps the distribution of dot products in a range where softmax has meaningful gradients.

**Q: What is the difference between BERT and GPT?**
> BERT is encoder-only with bidirectional attention — every token sees every other token. Pre-trained with MLM (predict masked tokens). Excellent at understanding tasks (classification, NER, QA) but not designed for generation. GPT is decoder-only with causal attention — each token sees only previous tokens. Pre-trained with next-token prediction. Excellent at generation; scales extremely well. Modern LLMs use decoder-only because generation is the core capability and scaling laws are more favourable.

**Q: What is positional encoding and why is it needed?**
> Transformers process all tokens in parallel via self-attention, unlike RNNs which process sequentially. Without positional information, the model treats "dog bites man" and "man bites dog" identically. Positional encoding injects order information by adding position-specific vectors to token embeddings. Original: fixed sinusoidal functions. Modern LLMs: RoPE (Rotary PE) — rotates Q and K vectors by position-dependent angles, encoding relative position in the dot product naturally.

**Q: What is attention complexity and how do efficient transformers address it?**
> Standard self-attention is O(n²d) in time and O(n²) in memory (for the attention matrix). For a 100K token context, that's 10^10 attention scores. Efficient approaches: Flash Attention — reorders computation to avoid materialising the full n×n matrix, reduces memory to O(n) with same output. Sparse attention — attend to only a subset of tokens. Sliding window attention (Longformer, Mistral). Linear attention approximations. Flash Attention is now standard in all production LLMs.

**Q: What is the difference between pre-training and fine-tuning a Transformer?**
> Pre-training: train on a massive general-purpose corpus (trillions of tokens) with a self-supervised objective (next-token prediction, MLM). Learns language, world knowledge, reasoning, and code. Compute-intensive (thousands of GPU-hours). Fine-tuning: adapt pre-trained weights to a specific downstream task using labelled data. Much cheaper (hours to days). The model's general knowledge is retained while task-specific behaviour is added. RLHF/DPO further aligns the model to human preferences after SFT fine-tuning.

**Q: What is a Vision Transformer (ViT)?**
> ViT applies the standard Transformer architecture to images by treating image patches as tokens. A 224×224 image is split into 16×16 patches (196 patches), each flattened and linearly projected into a token embedding. A [CLS] token is prepended, positional embeddings added, and the sequence processed by standard Transformer encoder layers. The [CLS] output is used for classification. ViT requires large-scale pre-training (unlike CNNs) because it lacks the translation equivariance and local connectivity inductive biases of CNNs.

**Q: What are emergent capabilities in LLMs?**
> Capabilities that appear discontinuously at certain model scales — not present in smaller models and not predicted by smooth extrapolation of loss curves. Examples: in-context learning (few-shot prompting emerges around GPT-3 scale), chain-of-thought reasoning (emerges ~100B parameters), arithmetic, multi-step code generation, following complex instructions. Hypothesised to emerge because sufficient scale allows the model to internally represent and compose more abstract reasoning structures — though debate exists on whether they're truly discontinuous or just hard to measure at small scale.

---

## 32. Quick Reference Cheat Sheet

### ML Algorithm Selection

```
Tabular data, fast training:     XGBoost / LightGBM (almost always baseline)
Linear relationships:             Linear/Logistic Regression
High-dimensional sparse:          SVM (with RBF) or Logistic Regression
Image classification:             ResNet, EfficientNet, ViT
Object detection:                 YOLO, DETR, Faster RCNN
NLP classification/NER/QA:        BERT / RoBERTa (fine-tuned)
NLP generation/chat:              GPT / Llama / Mistral (instruction-tuned)
Text + image understanding:       CLIP, LLaVA, GPT-4V
Time series:                      Prophet, N-BEATS, PatchTST (Transformer for TS)
Anomaly detection:                Isolation Forest, Autoencoder, One-Class SVM
Clustering (convex clusters):     K-Means
Clustering (arbitrary shape):     DBSCAN
Visualisation (2D):               UMAP > t-SNE > PCA
```

### Loss Function Selection

```
Binary classification:      Binary Cross-Entropy + Sigmoid output
Multi-class classification: Categorical Cross-Entropy + Softmax
Regression (normal):        MSE
Regression (with outliers): Huber Loss
Imbalanced classification:  Focal Loss
Generative (VAE):           Reconstruction + KL Divergence
Ranking:                    Triplet Loss, Contrastive Loss
```

### Optimiser Defaults

```
Most deep learning:         Adam (lr=1e-3, β₁=0.9, β₂=0.999)
LLM pre-training:           AdamW (+ cosine LR schedule + warmup)
LLM fine-tuning:            AdamW or Lion
CNN training (best acc):    SGD + Momentum (lr=0.1, momentum=0.9)
RNN:                        RMSProp or Adam
```

### Attention Quick Reference

```
Self-attention complexity:  O(n²d) time, O(n²) memory
Flash Attention:            O(nd) memory, same output, 2-4× faster
Multi-Head: h heads, each of dim d/h, concat → d
QKV: Q=what I want, K=what I have, V=what I give
Attention = softmax(QKᵀ / √dₖ) · V
Bidirectional (BERT): sees all tokens
Causal (GPT): sees only past tokens (mask future)
Cross-attention: Q from decoder, K,V from encoder
```

### Architecture Selection

```
Understanding tasks:         BERT / RoBERTa (encoder-only)
Generation tasks:            GPT / Llama / Mistral (decoder-only)
Seq2seq tasks:               T5 / BART (encoder-decoder)
Image classification:        ViT / EfficientNet
Image generation:            Stable Diffusion (diffusion) / StyleGAN
Multimodal:                  CLIP / LLaVA / Gemini
Efficient inference:         GQA + Flash Attention + Quantization
```

### Regularisation Toolkit

```
Overfitting (general):       More data > Dropout > L2 > Early stopping > Simpler model
Overfitting (images):        Data augmentation (RandAugment, MixUp, CutMix)
Overfitting (NLP):           Dropout, label smoothing, weight decay
Underfitting:                Larger model, more layers, more features, fewer constraints
Imbalanced data:             Class weights > SMOTE > Focal loss > Threshold tuning
```

### Normalisation Layers

```
BatchNorm: normalise across batch dimension; good for CNNs; bad for small batches
LayerNorm: normalise across feature dimension; good for Transformers; batch-size independent
RMSNorm:   like LayerNorm without mean subtraction; faster; used in Llama/Mistral
GroupNorm: normalise within groups of channels; good for small batch CV
InstanceNorm: per-sample per-channel; style transfer
```

### Gradient Problems & Fixes

```
Vanishing gradient:  ReLU, residual connections, LayerNorm, LSTM, He init
Exploding gradient:  Gradient clipping (clip_grad_norm_, threshold=1.0)
Dying ReLU:          Leaky ReLU, lower LR, He init, proper data normalisation
Saddle points:       Momentum, Adam, noise in SGD
Local minima:        SGD noise, cyclical LR, large batch → often not an issue at scale
```

---

*Sources: GeeksforGeeks ML/DL interviews 2025, InterviewCoder 120 DL questions, Analytixlabs DL Interview Guide, ThinkCloudly DL Questions, Skywork AI Transformer explainer, Medium Transformers Q&A, IGMGuru DL scenarios, Simplilearn, FinalRound AI — question banks from 2024–2026 interviews at Google, Meta, Amazon, OpenAI, Anthropic, Microsoft, Databricks.*
