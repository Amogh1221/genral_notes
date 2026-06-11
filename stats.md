# 📊 Statistics & Probability for ML Engineers

> **Interview-Ready | MLE/DS Focused | Math + Intuition + Code**

---

## Table of Contents

1. [Probability Basics](#1-probability-basics)
2. [Bayes Theorem](#2-bayes-theorem)
3. [Random Variables & Distributions](#3-random-variables--distributions)
4. [Expectation, Variance & Covariance](#4-expectation-variance--covariance)
5. [Common Distributions](#5-common-distributions)
6. [Hypothesis Testing](#6-hypothesis-testing)
7. [Confidence Intervals](#7-confidence-intervals)
8. [Maximum Likelihood Estimation (MLE)](#8-maximum-likelihood-estimation-mle)
9. [Bias-Variance Tradeoff](#9-bias-variance-tradeoff)
10. [Information Theory](#10-information-theory)
11. [PCA — Math Behind It](#11-pca--math-behind-it)
12. [A/B Testing](#12-ab-testing)
13. [Interview Q&A](#13-interview-qa)

---

## 1. Probability Basics

> **Interview Q:** *What is the difference between probability and likelihood?*

### Definitions

- **Probability:** Given a model/parameters, what is the chance of observing data X?
  `P(X | θ)` — parameters fixed, data varies.
- **Likelihood:** Given observed data X, how likely are different parameters θ?
  `L(θ | X)` — data fixed, parameters vary.

```
P(rain | cloudy) = 0.7   → probability (model fixed)
L(cloudy model | it rained) → likelihood (data fixed)
```

### Rules of Probability

```
# Addition Rule (OR)
P(A ∪ B) = P(A) + P(B) - P(A ∩ B)
# If mutually exclusive: P(A ∪ B) = P(A) + P(B)

# Multiplication Rule (AND)
P(A ∩ B) = P(A) * P(B|A)
# If independent: P(A ∩ B) = P(A) * P(B)

# Conditional Probability
P(A|B) = P(A ∩ B) / P(B)

# Law of Total Probability
P(A) = Σ P(A|Bᵢ) * P(Bᵢ)   for all partitions Bᵢ
```

### Independence vs Mutual Exclusivity

| | Independent | Mutually Exclusive |
|---|---|---|
| Definition | Knowing B doesn't change P(A) | A and B can't both happen |
| Formula | P(A∩B) = P(A)·P(B) | P(A∩B) = 0 |
| Can they coexist? | Yes | Only if P(A)=0 or P(B)=0 |
| ML example | Two unrelated features | Dog vs Cat (single label) |

---

## 2. Bayes Theorem

> **Interview Q:** *Explain Bayes Theorem and give an ML example.*

### Formula

```
P(θ|X) = P(X|θ) · P(θ) / P(X)

Posterior = (Likelihood × Prior) / Evidence

Where:
  P(θ|X)  = Posterior  — what we believe after seeing data
  P(X|θ)  = Likelihood — how well θ explains the data
  P(θ)    = Prior      — what we believed before seeing data
  P(X)    = Evidence   — normalisation constant (often ignored)
```

### Classic Example — Disease Testing

```
Disease prevalence: P(D) = 0.01 (1% of population)
Test sensitivity:   P(+|D) = 0.99 (true positive rate)
Test specificity:   P(-|¬D) = 0.95 → P(+|¬D) = 0.05 (false positive rate)

Q: If you test positive, what's the actual probability you have the disease?

P(D|+) = P(+|D) · P(D) / P(+)

P(+) = P(+|D)·P(D) + P(+|¬D)·P(¬D)
     = 0.99 × 0.01 + 0.05 × 0.99
     = 0.0099 + 0.0495 = 0.0594

P(D|+) = 0.0099 / 0.0594 ≈ 0.167

Only 16.7%! Even with a 99% accurate test,
a positive result only means ~17% chance of disease
because the disease is rare (low prior).
```

**ML Connection:** Naive Bayes classifier uses this directly. The "naive" assumption is feature independence: `P(features|class) = ΠP(featureᵢ|class)`.

### Bayesian vs Frequentist

| | Bayesian | Frequentist |
|---|---|---|
| Parameters | Random variables with distributions | Fixed unknown constants |
| Prior | Yes — incorporate prior beliefs | No — data speaks for itself |
| Output | Posterior distribution over θ | Point estimate + confidence interval |
| ML example | Bayesian neural networks, GP | Most standard ML (MLE training) |
| When to use | Small data, domain knowledge available | Large data, no strong prior |

---

## 3. Random Variables & Distributions

### Random Variable Types

```
Discrete RV:   takes countable values (0, 1, 2, ...)
               → PMF (Probability Mass Function): P(X = x)
               → Example: number of heads in 10 flips

Continuous RV: takes uncountable values (any real number)
               → PDF (Probability Density Function): f(x)
               → P(X = x) = 0 for any single point; P(a ≤ X ≤ b) = ∫f(x)dx
               → Example: height, temperature, model output
```

### CDF — Cumulative Distribution Function

```
F(x) = P(X ≤ x)

Properties:
  F(-∞) = 0
  F(+∞) = 1
  F is non-decreasing
  For continuous: f(x) = dF(x)/dx
```

---

## 4. Expectation, Variance & Covariance

> **Interview Q:** *What is variance? How is it different from standard deviation?*

### Expectation (Mean)

```
Discrete:   E[X] = Σ xᵢ · P(X = xᵢ)
Continuous: E[X] = ∫ x · f(x) dx

Properties:
  E[aX + b] = aE[X] + b          (linearity)
  E[X + Y]  = E[X] + E[Y]        (always, even if X,Y not independent)
  E[XY]     = E[X]·E[Y]          (ONLY if X,Y independent)
```

### Variance

```
Var(X) = E[(X - μ)²] = E[X²] - (E[X])²

Standard Deviation: σ = √Var(X)   (same units as X — more interpretable)

Properties:
  Var(aX + b) = a²·Var(X)         (shift b doesn't change spread)
  Var(X + Y)  = Var(X) + Var(Y)   (ONLY if independent)
  Var(X + Y)  = Var(X) + Var(Y) + 2·Cov(X,Y)  (general case)
```

### Covariance & Correlation

```
Cov(X, Y) = E[(X - μₓ)(Y - μᵧ)] = E[XY] - E[X]·E[Y]

Correlation: ρ(X,Y) = Cov(X,Y) / (σₓ · σᵧ)   → always in [-1, 1]

ρ = +1: perfect positive linear relationship
ρ = -1: perfect negative linear relationship
ρ =  0: no LINEAR relationship (could still have non-linear relationship!)

Key: Independent → Cov = 0. But Cov = 0 ≠ Independent.
```

**ML Connection:** Correlation between features → multicollinearity (bad for linear models). PCA decorrelates features by finding eigenvectors of the covariance matrix.

---

## 5. Common Distributions

> **Interview Q:** *Name 5 distributions and when you'd use each in ML.*

### Bernoulli Distribution

```
Single trial, two outcomes (0 or 1).
P(X=1) = p,  P(X=0) = 1-p

E[X] = p
Var(X) = p(1-p)

ML use: Binary classification output (single prediction).
```

### Binomial Distribution

```
n independent Bernoulli trials.
P(X=k) = C(n,k) · pᵏ · (1-p)^(n-k)

E[X] = np
Var(X) = np(1-p)

ML use: Number of correct predictions in n trials.
        Count of positive class in a batch.
```

### Normal (Gaussian) Distribution

```
X ~ N(μ, σ²)

f(x) = (1/σ√2π) · exp(-(x-μ)²/2σ²)

E[X] = μ
Var(X) = σ²

Properties:
  68% of data within μ ± σ
  95% of data within μ ± 2σ
  99.7% of data within μ ± 3σ

Standard Normal: Z = (X - μ)/σ ~ N(0,1)

ML use:
  - Assumption behind linear/logistic regression (errors ~ N)
  - Weight initialisation (Gaussian init)
  - Variational Autoencoders (latent space ~ N(0,I))
  - Central Limit Theorem: mean of large samples is Normal
```

### Poisson Distribution

```
Number of events in a fixed interval (time/space).
P(X=k) = (λᵏ · e^(-λ)) / k!

E[X] = λ
Var(X) = λ   (mean = variance — useful diagnostic)

ML use:
  - Count data modeling (clicks, word occurrences)
  - Arrival rates in queuing systems
  - When Var ≈ Mean in your data → Poisson might fit
```

### Exponential Distribution

```
Time between Poisson events.
f(x) = λ·e^(-λx)  for x ≥ 0

E[X] = 1/λ
Var(X) = 1/λ²

Memoryless property: P(X > s+t | X > s) = P(X > t)

ML use:
  - Survival analysis, time-to-event modeling
  - Modeling inter-arrival times
```

### Uniform Distribution

```
Continuous: f(x) = 1/(b-a) for a ≤ x ≤ b
E[X] = (a+b)/2
Var(X) = (b-a)²/12

ML use:
  - Random initialisation (uniform init for weights)
  - Random search hyperparameter tuning
  - Generating random numbers for sampling
```

### Beta Distribution

```
X ~ Beta(α, β)  — values in [0,1]

E[X] = α/(α+β)
Var(X) = αβ/((α+β)²(α+β+1))

ML use:
  - Prior over probabilities (Bayesian classification)
  - Modeling class imbalance priors
  - Beta-VAE
```

### Distribution Cheat Sheet

| Distribution | Parameters | Support | E[X] | Var(X) | ML Use |
|---|---|---|---|---|---|
| Bernoulli | p | {0,1} | p | p(1-p) | Binary output |
| Binomial | n, p | {0..n} | np | np(1-p) | Count successes |
| Normal | μ, σ² | ℝ | μ | σ² | Errors, weights |
| Poisson | λ | {0,1,2..} | λ | λ | Count events |
| Exponential | λ | [0,∞) | 1/λ | 1/λ² | Time between events |
| Uniform | a, b | [a,b] | (a+b)/2 | (b-a)²/12 | Random init |
| Beta | α, β | [0,1] | α/(α+β) | — | Probability priors |

---

## 6. Hypothesis Testing

> **Interview Q:** *What is a p-value? What does p < 0.05 actually mean?*

### Framework

```
Step 1: State H₀ (null hypothesis) — the "boring" claim, what we try to disprove.
        State H₁ (alternative hypothesis) — what we want to show.

Step 2: Choose significance level α (usually 0.05 = 5% false positive tolerance)

Step 3: Collect data, compute test statistic.

Step 4: Compute p-value.

Step 5: If p < α → reject H₀ (result is "statistically significant")
        If p ≥ α → fail to reject H₀ (not enough evidence)
```

### p-value — What It Really Means

```
p-value = P(observing data this extreme | H₀ is true)

p = 0.03 means:
  "If the null hypothesis were true, there's only a 3% chance
   of seeing data this extreme purely by chance."

p < 0.05 does NOT mean:
  ❌ There's a 95% chance H₁ is true.
  ❌ The effect is large or practically significant.
  ❌ H₀ is false.

It only means: the data is unlikely under H₀.

Very common ML interview trap: p-value is about H₀, not H₁.
```

### Type I and Type II Errors

```
                    Reality: H₀ True    Reality: H₀ False
Reject H₀           Type I Error (α)    Correct (Power = 1-β)
Fail to Reject H₀   Correct             Type II Error (β)

Type I  (False Positive): Rejecting H₀ when it's actually true.
        Rate = α (significance level). You control this.

Type II (False Negative): Failing to reject H₀ when H₁ is true.
        Rate = β. Depends on sample size, effect size.

Power = 1 - β = probability of correctly detecting a real effect.

Trade-off: Reducing α (stricter) → increases β (more false negatives).
```

**ML Connection:**
- Spam filter: Type I = marking good email as spam (bad). Type II = missing spam (annoying).
- Cancer detection: Type I = false cancer alarm. Type II = missing cancer (dangerous).
- Choose α based on which error is more costly.

### Common Statistical Tests

| Test | When to Use | Null Hypothesis |
|---|---|---|
| z-test | Known population variance, large n | μ = μ₀ |
| t-test (one sample) | Unknown variance, small n | μ = μ₀ |
| t-test (two sample) | Compare two group means | μ₁ = μ₂ |
| Paired t-test | Same subjects, two conditions | μ_diff = 0 |
| Chi-square test | Categorical data, independence | Variables are independent |
| F-test / ANOVA | Compare 3+ group means | All means equal |
| Mann-Whitney U | Non-parametric, compare distributions | Same distribution |

### Statistical vs Practical Significance

```
With n = 10,000,000:
  Even a 0.001% difference in conversion rate will be p < 0.001.
  But does that 0.001% matter for the business? Maybe not.

Large n → small p-values even for tiny effects.
Always report effect size (Cohen's d, relative improvement) alongside p-value.
```

---

## 7. Confidence Intervals

> **Interview Q:** *What does a 95% confidence interval mean?*

### Definition

```
A 95% CI means:
  If we repeated the experiment 100 times and computed a CI each time,
  approximately 95 of those intervals would contain the true parameter.

It does NOT mean:
  ❌ There's a 95% chance the true value is in THIS specific interval.
  ❌ 95% of data falls in this range (that's a prediction interval).

Once computed, the interval either contains the true value or it doesn't.
The 95% refers to the PROCEDURE, not this specific interval.
```

### Formula for Mean (known σ)

```
CI = x̄ ± z_(α/2) · (σ/√n)

For 95% CI: z_(0.025) = 1.96
For 99% CI: z_(0.005) = 2.576

Width of CI:
  ↑ n (more data)    → narrower CI (more precise)
  ↑ σ (more spread)  → wider CI (less precise)
  ↑ confidence level → wider CI (more conservative)
```

### ML Connection

```
When reporting model accuracy, always report a CI:
  "Model accuracy: 87.3% ± 1.2% (95% CI)"

Bootstrap CI: resample your test set 1000 times,
              compute metric each time,
              take 2.5th and 97.5th percentile.
```

---

## 8. Maximum Likelihood Estimation (MLE)

> **Interview Q:** *What is MLE? How does it connect to training a neural network?*

### Core Idea

```
Find the parameters θ that maximise the probability of observing the data X.

θ_MLE = argmax_θ P(X | θ) = argmax_θ L(θ | X)

In practice: maximise log-likelihood (same solution, easier math)
θ_MLE = argmax_θ log L(θ | X) = argmax_θ Σ log P(xᵢ | θ)
```

### MLE for Gaussian → MSE Loss

```
Assume y_i = f(x_i; θ) + ε,  ε ~ N(0, σ²)

Then: P(y_i | x_i, θ) = N(f(x_i;θ), σ²)

Log-likelihood:
  log L = Σ log N(yᵢ; f(xᵢ;θ), σ²)
        = -n/2 log(2πσ²) - 1/(2σ²) Σ(yᵢ - f(xᵢ;θ))²

Maximising log L = Minimising Σ(yᵢ - f(xᵢ;θ))²

→ MLE with Gaussian noise assumption = MSE loss!
   Training neural networks with MSE = doing MLE.
```

### MLE for Bernoulli → Cross-Entropy Loss

```
Binary classification: y ∈ {0,1}, model predicts p = σ(f(x;θ))

P(y|x,θ) = p^y · (1-p)^(1-y)

Log-likelihood:
  log L = Σ [yᵢ log pᵢ + (1-yᵢ) log(1-pᵢ)]

Maximising log L = Minimising -Σ[yᵢ log pᵢ + (1-yᵢ) log(1-pᵢ)]

→ Binary cross-entropy loss!
   MLE with Bernoulli = minimising cross-entropy.
```

**Key Insight:** Every standard loss function is a negative log-likelihood under some distributional assumption.

| Loss Function | Assumed Distribution | Model Type |
|---|---|---|
| MSE | Gaussian errors | Regression |
| Binary cross-entropy | Bernoulli | Binary classification |
| Categorical cross-entropy | Categorical/Multinomial | Multi-class |
| MAE | Laplace errors | Robust regression |

---

## 9. Bias-Variance Tradeoff

> **Interview Q:** *Explain bias-variance tradeoff. How does it relate to overfitting?*

### Decomposition

```
Expected Test Error = Bias² + Variance + Irreducible Noise

Bias²:    Error from wrong assumptions in the model.
          High bias → underfitting → model too simple to capture pattern.
          E.g.: Fitting a line to quadratic data.

Variance: Error from sensitivity to small fluctuations in training data.
          High variance → overfitting → model too complex, memorises noise.
          E.g.: High-degree polynomial that wiggles through every training point.

Irreducible Noise (σ²): Noise in the data itself. Cannot be reduced by any model.
```

### Formal Derivation

```
For a test point x with true value y = f(x) + ε, ε ~ N(0,σ²):

E[(y - ŷ)²] = (E[ŷ] - f(x))² + E[(ŷ - E[ŷ])²] + σ²
              =      Bias²      +      Variance      + Noise

Where ŷ = model prediction (varies across training sets)
```

### Examples Across Models

| Model | Bias | Variance | Comment |
|---|---|---|---|
| Linear Regression (simple) | High | Low | Underfits complex data |
| Decision Tree (depth=1) | High | Low | Too simple |
| Decision Tree (full) | Low | High | Overfits |
| Random Forest | Low | Medium | Variance reduced by averaging |
| k-NN (k=1) | Low | High | Very sensitive to training data |
| k-NN (k=large) | High | Low | Too smooth |
| Neural Net (small) | High | Low | Underfits |
| Neural Net (large, unregularised) | Low | High | Overfits |

### How to Fix

```
High Bias (Underfitting):
  → More complex model
  → Add features / feature engineering
  → Reduce regularisation
  → Train longer

High Variance (Overfitting):
  → More training data
  → Regularisation (L1/L2, dropout)
  → Reduce model complexity
  → Ensemble methods (bagging)
  → Early stopping
  → Data augmentation
```

---

## 10. Information Theory

> **Interview Q:** *What is entropy? Why is cross-entropy used as a loss function?*

### Entropy

```
Entropy measures uncertainty/information content.
H(X) = -Σ P(x) · log₂ P(x)   (bits)
H(X) = -Σ P(x) · ln P(x)      (nats — used in ML)

Uniform distribution (max uncertainty) → maximum entropy
One outcome certain (no uncertainty)   → zero entropy

Example:
  Fair coin: H = -(0.5 log 0.5 + 0.5 log 0.5) = 1 bit
  Biased coin (p=0.9): H = -(0.9 log 0.9 + 0.1 log 0.1) ≈ 0.47 bits
```

### KL Divergence

```
KL(P || Q) = Σ P(x) · log(P(x) / Q(x))

Measures how different Q is from the "true" distribution P.
KL ≥ 0 always. KL = 0 iff P = Q.
NOT symmetric: KL(P||Q) ≠ KL(Q||P)

ML use: VAE loss, knowledge distillation, RL policy optimisation.
```

### Cross-Entropy

```
H(P, Q) = -Σ P(x) · log Q(x)

Relationship: H(P, Q) = H(P) + KL(P || Q)

When P is the true labels (one-hot) and Q is model predictions:
  H(P) = 0 (true labels have zero entropy — they're fixed)
  H(P, Q) = KL(P || Q)

→ Minimising cross-entropy loss = minimising KL divergence from true distribution.
   This is why cross-entropy is the right loss for classification.
```

### Mutual Information

```
I(X; Y) = H(X) - H(X|Y) = H(Y) - H(Y|X)

How much does knowing Y reduce uncertainty about X?
I(X;Y) = 0 → X and Y are independent.

ML use:
  - Feature selection (filter features with high MI with target)
  - Information gain in Decision Trees (ID3 algorithm)
  - MINE (Mutual Information Neural Estimation)
```

---

## 11. PCA — Math Behind It

> **Interview Q:** *How does PCA work mathematically? What is it actually doing?*

### The Goal

```
Reduce dimensionality by finding directions of maximum variance.
Project data onto fewer dimensions while losing minimum information.
```

### Step-by-Step Algorithm

```
Step 1: Centre the data
  X_centred = X - mean(X)   (subtract column means)

Step 2: Compute covariance matrix
  C = (1/n) · Xᵀ · X    (d×d matrix, d = number of features)

Step 3: Compute eigenvectors and eigenvalues of C
  C · v = λ · v
  Eigenvector v = direction of variance
  Eigenvalue λ = amount of variance in that direction

Step 4: Sort eigenvalues in descending order.
  The eigenvector with the largest eigenvalue = first principal component
  (direction of maximum variance)

Step 5: Select top k eigenvectors → matrix W (d×k)

Step 6: Project data
  X_reduced = X_centred · W    (n×k matrix)
```

### Explained Variance Ratio

```
Explained variance by component i = λᵢ / Σλⱼ

Cumulative explained variance: choose k such that
Σᵢ₌₁ᵏ (λᵢ / Σλⱼ) ≥ 0.95   (retain 95% of variance)

Scree plot: plot eigenvalues vs component number,
            look for the "elbow" to choose k.
```

### What PCA Is NOT Doing

```
❌ PCA does not find the most discriminative directions (that's LDA)
❌ PCA does not handle non-linear structure (use t-SNE or UMAP for that)
❌ PCA components are not necessarily interpretable features
✓ PCA finds directions of maximum variance
✓ PCA components are orthogonal (decorrelated)
✓ PCA is optimal for linear dimensionality reduction (minimises reconstruction error)
```

### PCA via SVD (How sklearn actually does it)

```
X = U · Σ · Vᵀ   (SVD decomposition)

Principal components = columns of V
Singular values σᵢ → eigenvalues = σᵢ²/n

SVD is numerically more stable than computing covariance matrix explicitly.
```

---

## 12. A/B Testing

> **Interview Q:** *How would you design an A/B test to evaluate a new ML model?*

### Framework

```
Step 1: Define the metric (primary KPI)
  → Click-through rate, conversion rate, revenue per user, model accuracy

Step 2: State hypotheses
  H₀: new model = old model (no difference)
  H₁: new model ≠ old model (two-tailed) or > (one-tailed)

Step 3: Determine sample size (power analysis)
  n = 2 · (z_α/2 + z_β)² · σ² / δ²

  Where:
    α = significance level (0.05)
    β = type II error rate (0.2 → power = 80%)
    σ = estimated standard deviation
    δ = minimum detectable effect (smallest difference worth caring about)

Step 4: Randomise and run experiment
  → Random assignment of users to control/treatment
  → Run until sample size reached (don't stop early!)

Step 5: Analyse results
  → Compute test statistic, p-value
  → If p < α: deploy new model
  → Report confidence interval for the effect size
```

### Common Pitfalls

```
1. Peeking problem: stopping early when p < 0.05 inflates false positive rate.
   Fix: pre-determine sample size and run to completion.

2. Multiple testing: running 20 tests, 1 will be significant by chance.
   Fix: Bonferroni correction (α_adjusted = α/m for m tests).

3. Network effects: users in A affect users in B (social networks).
   Fix: cluster-based randomisation (assign by user clusters).

4. Novelty effect: users engage more with anything new temporarily.
   Fix: run experiment long enough for novelty to wear off.

5. Simpson's Paradox: trend reverses when subgroups are combined.
   Fix: stratified analysis.
```

---

## 13. Interview Q&A

**Q: What is the Central Limit Theorem and why does it matter for ML?**
> The CLT states that the mean of a large number of independent, identically distributed random variables approaches a Normal distribution, regardless of the original distribution. Minimum n ≈ 30 for most distributions. This is why: (1) we can use z-tests/t-tests on non-normal data with large samples, (2) gradient descent noise is approximately Gaussian (sum of many data contributions), (3) A/B testing works with non-normal metrics at scale.

**Q: What is the difference between standard deviation and standard error?**
> Standard deviation (σ) measures spread of individual data points around the mean. Standard error (SE = σ/√n) measures how much the sample mean varies across different samples — it's the standard deviation of the sampling distribution of the mean. SE decreases with more data; SD stays roughly constant. Use SD to describe data spread; use SE to quantify uncertainty of an estimate.

**Q: What is regularisation from a probabilistic perspective?**
> L2 regularisation (Ridge) = placing a Gaussian prior on the weights → MAP (Maximum A Posteriori) estimation instead of MLE. L1 regularisation (Lasso) = placing a Laplace prior on weights → MAP with Laplace prior. Regularisation is not a hack — it's Bayesian inference with a prior.

**Q: When would you use mean vs median?**
> Mean is sensitive to outliers (one extreme value shifts it). Median is robust to outliers. Use mean when data is roughly symmetric/normal (house prices in a homogeneous neighborhood). Use median when data is skewed or has outliers (income data, housing in mixed areas). In ML: mean loss is sensitive to outlier samples → consider robust losses (MAE, Huber).

**Q: What is the difference between covariance and correlation?**
> Covariance measures the direction of the linear relationship between two variables, but its magnitude depends on the scale of the variables (not interpretable by itself). Correlation normalises covariance by the product of standard deviations, giving a unitless value in [-1,1]. Always report correlation, not raw covariance. Use covariance when building the covariance matrix for PCA (scale is meaningful there).

**Q: What is overfitting and how do you detect it?**
> Overfitting is when the model learns training data too specifically — capturing noise rather than the true signal. High training accuracy, low test accuracy. Detection: plot learning curves (training loss keeps decreasing, validation loss starts increasing). Fix: regularisation, more data, simpler model, early stopping, dropout.

**Q: Explain the difference between parametric and non-parametric models.**
> Parametric models assume a fixed functional form with a fixed number of parameters (linear regression, logistic regression, neural networks — after architecture is fixed). Non-parametric models make no assumption about functional form — complexity grows with data (k-NN, kernel SVM, Gaussian Processes, Decision Trees). Parametric: faster, less data needed but may underfit if assumptions wrong. Non-parametric: more flexible but slower and needs more data.

**Q: What is the law of large numbers?**
> As n → ∞, the sample mean converges to the true population mean. This is why: training on more data generally gives better models, test set evaluation becomes more reliable with more test samples, Monte Carlo estimation converges. Weak LLN: convergence in probability. Strong LLN: almost sure convergence.

**Q: What is multicollinearity and why is it a problem?**
> When two or more features are highly correlated with each other. Problem for linear/logistic regression: coefficients become unstable (large variance), hard to interpret individual feature importance, standard errors blow up. Not a problem for predictions per se — only for coefficient interpretation. Detection: Variance Inflation Factor (VIF) > 5-10. Fix: PCA, drop one of the correlated features, Ridge regularisation.

**Q: What is the difference between MLE and MAP?**
> MLE: find θ that maximises P(data|θ) — no prior, just data. MAP: find θ that maximises P(θ|data) ∝ P(data|θ)·P(θ) — multiplies likelihood by prior. MAP = MLE + regularisation (the prior IS the regulariser). With uninformative (flat) prior, MAP = MLE. With Gaussian prior, MAP = L2 regularisation. With Laplace prior, MAP = L1 regularisation.

---

*Key References: Bishop's PRML, Murphy's PML, Stanford CS229 notes, DeepMind Statistics for ML.*
