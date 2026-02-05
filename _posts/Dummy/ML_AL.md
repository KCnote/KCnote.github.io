---
layout: post
title: "LeetCode 49. Group Anagrams"
date: 2025-12-19 00:00:00 +0900
author: kang
categories: [leetcode, medium]
tags: [coding test, medium, leetcode]
pin: false
math: true
mermaid: true
---


Python - 
SNU - Joonseok Lee Lecture ML/AL

Ability to perceive or infer information and apply to adaptive behavior

Statistical Machine Learning.

ML: more suitable ways for machines
- Field of study in artificial intelligence concerned with the development and study of statistical algorithms that can learn from data and generalize to unseen data and thus perform tasks without explicit instruction.
- based on data-driven approaches, minimizing manual instructions

DL: mimic human intelligence works

-
Supervised learning: given pairs of example and label. once trained, a model predicts label for an unseen example
Unsupervised learning: given a list of examples without label. a model learns patterns exclusively from data samples

Usually Difficulty level : Unsupervised learning > Supervised learning

-Supervised learning
step1. design the form of our model (ex. y = ax, inference object : a)
step2. define the goal of model
step3. find the parameters that best achieves the goal with training data.
step4. given an unseen x, estimate its label \hat{y}, --> computing.

parametric/non-parametric method that can be characterized

parametric: assume a functional form and predict parameters
non-parametric: not assume a functional form, get as close to the data points as possible witout being too rough or wiggly -> So need to large-scale dataset

>Step 2.
mean-squared prediction error 
$$
\mathbb{E}\big[(Y - \hat{f}(X))^2 \mid X = x\big]
= \mathbb{E}\big[(f(X) + \varepsilon - \hat{f}(X))^2 \mid X = x\big]
= \underbrace{(f(x) - \hat{f}(x))^2}_{\text{Reducible}}
+ \underbrace{\mathrm{Var}(\varepsilon)}_{\text{Irreducible}}.
$$

>Step 3.
estimate the unknown function -> optimization
possible in math : good
impossible in math : gradient descent

>Step 4.
If we check performance with training data
$$
\mathrm{MSE}_{\mathrm{Tr}}
= \mathrm{Ave}_{i \in \mathrm{Tr}}\left[\, y_i - f(x_i) \,\right]^2
= \frac{1}{N}\sum_{i \in \mathrm{Tr}} \left[\, y_i - f(x_i) \,\right]^2
$$

--> overfit, biased

If we check performance with test data
$$
\mathrm{MSE}_{\mathrm{Te}}
= \mathrm{Ave}_{i \in \mathrm{Te}}\left[\, y_i - \hat{f}(x_i) \,\right]^2
= \frac{1}{M}\sum_{i \in \mathrm{Te}} \left[\, y_i - \hat{f}(x_i) \,\right]^2
$$

---
Trade-off in model selection
1. good fit vs overfit, underfit
2. prediction accuracy vs interpretability
3. parsimony vs complex

$$
\underbrace{\mathbb{E}\big[\, y_0 - \hat{f}(x_0) \,\big]^2}_{\text{Expected squared prediction error}}
=
\underbrace{\mathrm{Var}\big(\hat{f}(x_0)\big)}_{\text{Variance of the estimator}}
+
\underbrace{\big[\mathrm{Bias}\big(\hat{f}(x_0)\big)\big]^2}_{\text{Squared bias}}
+
\underbrace{\mathrm{Var}(\varepsilon)}_{\text{Irreducible error (noise)}}
$$

$$
\mathrm{Bias}\big(\hat{f}(x_0)\big)
= \mathbb{E}\big[\hat{f}(x_0)\big] - f(x_0)
$$


- Linear Regression
    Assuming that the dependence of y on set of x is linear.

$$
y = \beta_0 + \beta_1 x + \varepsilon
$$

$$
\beta_0,\ \beta_1 \ \text{: coefficients (intercept and slope)}, 
\qquad
\varepsilon \ \text{: error}
$$ 

----
$$
\mathrm{RSS} = e_1^2 + e_2^2 + \cdots + e_n^2
$$

$$
\mathrm{RSS} = (y_1 - \hat{y}_1)^2 + \cdots + (y_n - \hat{y}_n)^2
$$

$$
\mathrm{RSS}
= (y_1 - \hat{\beta}_0 - \hat{\beta}_1 x_1)^2
+ \cdots +
(y_n - \hat{\beta}_0 - \hat{\beta}_1 x_n)^2
$$

$$
\mathrm{RSS}=\sum_{i=1}^{n}\big(y_i-(\beta_0+\beta_1 x_i)\big)^2
$$
----
$$
\frac{\partial}{\partial \beta_0}
\sum_{i=1}^{n}\big(y_i-(\beta_0+\beta_1 x_i)\big)^2
= -2\sum_{i=1}^{n}\big(y_i-(\beta_0+\beta_1 x_i)\big)=0
$$

$$
\sum_{i=1}^{n} y_i - n\beta_0 - \beta_1\sum_{i=1}^{n}x_i = 0
\quad\Longleftrightarrow\quad
\beta_0=\bar{y}-\beta_1\bar{x},
\qquad
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i,\;
\bar{y}=\frac{1}{n}\sum_{i=1}^{n}y_i
$$
----
$$
\frac{\partial}{\partial \beta_1}
\sum_{i=1}^{n}\big(y_i-(\beta_0+\beta_1 x_i)\big)^2
= -2\sum_{i=1}^{n}x_i\big(y_i-(\beta_0+\beta_1 x_i)\big)=0
$$

$$
\sum_{i=1}^{n}x_i(y_i-\bar{y})
-\beta_1\sum_{i=1}^{n}x_i(x_i-\bar{x})=0
$$

$$
\hat{\beta}_1=
\frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}
{\sum_{i=1}^{n}(x_i-\bar{x})^2},
\qquad
\hat{\beta}_0=\bar{y}-\hat{\beta}_1\bar{x}
$$

----

$$
\mathrm{RSE}
= \sqrt{\frac{\mathrm{RSS}}{n-2}}
= \sqrt{\frac{\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}{\,n-2\,}}
$$

$$
\mathrm{RMSE}
= \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}
$$

$$
R^2
= \frac{\mathrm{TSS}-\mathrm{RSS}}{\mathrm{TSS}}
= 1-\frac{\mathrm{RSS}}{\mathrm{TSS}},
\qquad
\mathrm{TSS}=\sum_{i=1}^{n}(y_i-\bar{y})^2
$$


$$
r = \frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}
{\sqrt{\sum_{i=1}^{n}(x_i-\bar{x})^2}\;
 \sqrt{\sum_{i=1}^{n}(y_i-\bar{y})^2}},
\qquad
R^2=r^2 \quad (\text{simple linear regression})
$$
---

1) When predictors are uncorrelated — ideal case

Each coefficient can be estimated and tested independently
Interpretation is clear

2) When predictors are correlated (multicollinearity)

Variance of coefficients increases → unstable estimates
Interpretation becomes difficult:
when one predictor changes, others tend to change as well
Coefficient signs and magnitudes may become unreliable

3) Be careful with causality

In observational data, correlation ≠ causation
Causal claims require experiments or strong assumptions

---
$$
\text{Sales}
= \beta_0 + \beta_1 \cdot \text{TV}
+ \beta_2 \cdot \text{Radio}
+ \beta_3 \cdot \text{Newspaper}
+ \varepsilon
$$

$$
\beta_0 = 2.939, \quad
\beta_1 = 0.046, \quad
\beta_2 = 0.189, \quad
\beta_3 = -0.001
$$

$$
\begin{array}{c|cccc}
 & \text{TV} & \text{Radio} & \text{Newspaper} & \text{Sales} \\
\hline
\text{TV} & 1.0000 & 0.0548 & 0.0567 & 0.7822 \\
\text{Radio} & 0.0548 & 1.0000 & 0.3541 & 0.5762 \\
\text{Newspaper} & 0.0567 & 0.3541 & 1.0000 & 0.2283 \\
\text{Sales} & 0.7822 & 0.5762 & 0.2283 & 1.0000
\end{array}
$$

$$
\text{Larger coefficient does not necessarily imply higher correlation.}
$$

$$
\text{(Regression coefficients depend on the scale of predictors.)}
$$

Key Insight

A larger regression coefficient does NOT necessarily mean higher correlation.

Regression coefficients depend on units and scale of predictors, while correlation is scale-invariant.

Newspaper shows weak correlation with Sales and near-zero coefficient → little predictive value.

---

Likelihood is the joint probability (or probability density) of the observed data viewed as a function of the parameters of a statistical model.

Intuitively, the probability that the event would happen.

$$
L(\theta; x_1, \ldots, x_n) = p_\theta(x_1, \ldots, x_n)
$$

With the i.i.d. assumption, the probability for each sample is independent:

$$
L(\theta; x_1, \ldots, x_n)
= p_\theta(x_1)\cdot p_\theta(x_2)\cdots p_\theta(x_n)
$$

$$
L(\theta; x_1, \ldots, x_n)
= \prod_{i=1}^{n} p_\theta(x_i)
$$

Log-likelihood is simply the log of the likelihood.

$$
\ell(\theta; x_1, \ldots, x_n)
= \log \prod_{i=1}^{n} p_\theta(x_i)
= \sum_{i=1}^{n} \log p_\theta(x_i)
$$

The value of x which makes f(x) the highest (or lowest) is the same as that which makes log f(x) the highest (or lowest).

$$
\arg\max_x f(x) = \arg\max_x \log f(x)
$$

Why we use log-likelihood:

1. Numerical stability: multiplying many probabilities makes the value extremely close to 0 (underflow).  
   Taking log converts products into sums and avoids numerical underflow.

2. Same optimum: log is a strictly monotonic function, so maximizing likelihood or log-likelihood gives the same parameter.

3. Easier optimization: sums are easier to differentiate than products, which simplifies solving for parameters using derivatives.

Log-likelihood is widely used because of its mathematical convenience.

Maximum Likelihood Estimator (MLE)

A method of estimating the parameters of an assumed probability distribution given some observed data.

Definition: Maximum Likelihood Estimator (MLE) estimates the parameters by maximizing the likelihood function so that, under the assumed statistical model, the observed data is most probable.

$$
\hat{\theta}(x_1, \ldots, x_n) = \arg\max_{\theta} L(\theta; x_1, \ldots, x_n)
$$

$$
= \arg\max_{\theta} \ell(\theta; x_1, \ldots, x_n)
$$

$$
= \arg\max_{\theta} \sum_{i=1}^{n} \log p_{\theta}(x_i)
$$

MLE is the parameter \(\theta\) that gives maximum probability to the observed samples among all possible \(\theta\).

if we can calculate parameters with formula = closed-form solution


## Maximum Likelihood Estimator (MLE) — Optimization

### How to Compute Maximum Likelihood

- A function reaches a local maximum or minimum when its **first derivative (gradient) is zero**.
- Therefore, we find MLE by solving:

$$
\nabla \ell(\theta) = 0
$$

or equivalently, for each parameter component:

$$
\frac{\partial \ell(\theta)}{\partial \theta_j} = 0 \quad \text{for } j = 1, \dots, d
$$

---

### Second-Order Condition (Maximum vs Minimum)

- To ensure the solution is a **local maximum (not minimum)**, check the Hessian matrix:

$$
H(\theta) = \left[ \frac{\partial^2 \ell(\theta)}{\partial \theta_i \, \partial \theta_j} \right]
$$

- The log-likelihood has a local maximum if the Hessian is **negative definite**:

$$
v^T H(\theta) v < 0 \quad \text{for any nonzero vector } v
$$

---

### Key Takeaway

- MLE is obtained by solving **gradient = 0**.
- Use the **Hessian (second derivative)** to verify the solution is a maximum.
- Negative definite Hessian ⇒ local maximum of log-likelihood.


## Maximum Likelihood Estimator (MLE) Example: Bernoulli Distribution

### Bernoulli Model

For a Bernoulli random variable \(x \in \{0,1\}\), the probability mass function is:

$$
p_\theta(x) = \theta^x (1-\theta)^{1-x}
$$

- \(x \in \{0,1\}\) is one observed sample.
- \(\theta\) is the unknown parameter, and it represents \(\mathbb{P}(x=1)\).

---

### Maximum Likelihood

The MLE chooses \(\theta\) that maximizes the likelihood (equivalently, log-likelihood):

$$
\hat{\theta}(x_1, \ldots, x_n)
= \arg\max_{\theta} L(\theta; x_1, \ldots, x_n)
= \arg\max_{\theta} \ell(\theta; x_1, \ldots, x_n)
$$

Under the i.i.d. assumption:

$$
\ell(\theta; x_1, \ldots, x_n)
= \sum_{i=1}^{n} \log p_\theta(x_i)
= \sum_{i=1}^{n} \log\Big(\theta^{x_i}(1-\theta)^{1-x_i}\Big)
$$

Equivalently:

$$
\ell(\theta; x_1, \ldots, x_n)
= \sum_{i=1}^{n} \Big[ x_i \log \theta + (1-x_i)\log(1-\theta) \Big]
$$

---

### (Optional) Closed-form Result

Maximizing the log-likelihood gives:

$$
\hat{\theta} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

So, the MLE for \(\theta\) is simply the sample mean of the Bernoulli observations.


## Maximum Likelihood Estimator (MLE) — Bernoulli (Derivation)

### Log-likelihood

For Bernoulli observations \(x_i \in \{0,1\}\):

$$
\ell(\theta)
= \sum_{i=1}^{n} x_i \log \theta
+ \sum_{i=1}^{n} (1 - x_i) \log(1 - \theta)
$$

---

### First Derivative

To find the maximum, set the derivative to zero:

$$
\frac{\partial \ell(\theta)}{\partial \theta}
= \frac{1}{\theta} \sum_{i=1}^{n} x_i
- \frac{1}{1-\theta} \sum_{i=1}^{n} (1 - x_i)
= 0
$$

Solve:

$$
\frac{1}{\theta} \sum_{i=1}^{n} x_i
= \frac{1}{1-\theta} \sum_{i=1}^{n} (1 - x_i)
$$

$$
(1-\theta) \sum_{i=1}^{n} x_i
= \theta \sum_{i=1}^{n} (1 - x_i)
$$

$$
\sum_{i=1}^{n} x_i
= n\theta
$$

$$
\hat{\theta} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

---

### Second Derivative (Check Maximum)

$$
\frac{\partial^2 \ell(\theta)}{\partial \theta^2}
= -\frac{1}{\theta^2} \sum_{i=1}^{n} x_i
- \frac{1}{(1-\theta)^2} \sum_{i=1}^{n} (1 - x_i)
$$

- This is **negative**, so the solution is a **maximum**.

---

### Result

- The MLE of Bernoulli parameter \(\theta\) is the **sample mean**:

$$
\hat{\theta} = \bar{x}
$$


## Maximum Likelihood Estimator (MLE) — Gaussian Distribution

### Gaussian Model

For a Gaussian (Normal) distribution:

$$
p_{\theta}(x) = \frac{1}{\sigma \sqrt{2\pi}} 
\exp\left(-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2\right)
$$

- Observations: \(x_i\)
- Parameters: \(\theta = (\mu, \sigma)\)
  - \(\mu\): mean
  - \(\sigma\): standard deviation

---

### Maximum Likelihood

The MLE finds parameters that maximize the likelihood (or log-likelihood):

$$
\hat{\theta}(x_1, \ldots, x_n)
= \arg\max_{\theta} L(\theta; x_1, \ldots, x_n)
= \arg\max_{\theta} \ell(\theta; x_1, \ldots, x_n)
$$

Under the i.i.d. assumption:

$$
\ell(\theta)
= \sum_{i=1}^{n} \log p_{\theta}(x_i)
$$

$$
= \sum_{i=1}^{n} \log 
\left(
\frac{1}{\sigma \sqrt{2\pi}} 
\exp\left(-\frac{1}{2}\left(\frac{x_i-\mu}{\sigma}\right)^2\right)
\right)
$$

---

### Simplified Log-Likelihood

$$
\ell(\mu, \sigma)
= -n \log \sigma 
- \frac{n}{2} \log (2\pi)
- \frac{1}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2
$$

---

### Closed-form Results

Maximizing the log-likelihood gives:

$$
\hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

$$
\hat{\sigma}^2 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \hat{\mu})^2
$$

---

### Key Takeaway

- The MLE of Gaussian mean is the **sample mean**.
- The MLE of variance is the **average squared deviation** from the mean.
- Note: This uses \(1/n\) (MLE), not \(1/(n-1)\) (unbiased estimator).

## Maximum Likelihood Estimator (MLE) — Gaussian (Derivation)

### Log-Likelihood

For Gaussian observations:

$$
\ell(\mu, \sigma)
= \sum_{i=1}^{n} \log \left(
\frac{1}{\sigma \sqrt{2\pi}}
\exp\left(-\frac{1}{2}\left(\frac{x_i-\mu}{\sigma}\right)^2\right)
\right)
$$

Simplify:

$$
\ell(\mu, \sigma)
= -n \log \sigma
- \frac{n}{2} \log (2\pi)
- \frac{1}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2
$$

---

### First Derivative w.r.t. \(\mu\)

Set derivative to zero:

$$
\frac{\partial \ell}{\partial \mu}
= \sum_{i=1}^{n} \frac{x_i - \mu}{\sigma^2} = 0
$$

$$
\sum_{i=1}^{n} x_i = n\mu
$$

$$
\hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

---

### First Derivative w.r.t. \(\sigma\)

$$
\frac{\partial \ell}{\partial \sigma}
= -\frac{n}{\sigma}
+ \frac{1}{\sigma^3} \sum_{i=1}^{n} (x_i - \mu)^2 = 0
$$

Solve:

$$
\frac{n}{\sigma}
= \frac{1}{\sigma^3} \sum_{i=1}^{n} (x_i - \mu)^2
$$

$$
\sigma^2 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \mu)^2
$$

---

### Final Result

$$
\hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

$$
\hat{\sigma}^2 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \hat{\mu})^2
$$

---

### Key Takeaway

- MLE mean = **sample mean**
- MLE variance = **average squared deviation**
- Uses \(1/n\), not \(1/(n-1)\) (unbiased estimator)



## MLE for Linear Regression

### Linear Model Assumption

We assume a linear relationship between input \(x\) and output \(y\):

$$
y = \beta^T x = \beta_0 + \beta_1 x_1 + \cdots + \beta_d x_d
$$

Example:

$$
\text{Sales} = \beta_0 + \beta_1 \cdot \text{TV}
+ \beta_2 \cdot \text{Radio}
+ \beta_3 \cdot \text{Newspaper} + \varepsilon
$$

---

### Probabilistic Assumption

We assume additive Gaussian noise:

$$
y = \beta^T x + \varepsilon, \quad
\varepsilon \sim \mathcal{N}(0, \sigma^2)
$$

This implies:

$$
y \mid x \sim \mathcal{N}(\beta^T x, \sigma^2)
$$

---

### Interpretation

- Mean:

$$
\mathbb{E}[y \mid x] = \beta^T x
$$

- Variance:

$$
\mathrm{Var}(y \mid x) = \sigma^2 \quad \text{(constant)}
$$

---

### Key Takeaway

- Linear regression can be viewed as **MLE under Gaussian noise**.
- The model assumes:
  - Linear mean function
  - Constant variance (homoscedasticity)


# MLE for Linear Regression — Equivalence to Least Squares

## Model
Assume a linear model with Gaussian noise:

$$
y_i = \beta^T x_i + \epsilon_i, \quad \epsilon_i \sim \mathcal{N}(0,\sigma^2)
$$

Then the conditional distribution is:

$$
y_i \mid x_i \sim \mathcal{N}(\beta^T x_i, \sigma^2)
$$

---

## Log-Likelihood

The log-likelihood of the data is:

$$
\ell(\beta) = \sum_{i=1}^{n} \log p(y_i \mid x_i)
= \sum_{i=1}^{n} \log \left( \frac{1}{\sigma\sqrt{2\pi}} 
\exp\left(-\frac{1}{2}\frac{(y_i - \beta^T x_i)^2}{\sigma^2}\right) \right)
$$

Simplify:

$$
\ell(\beta) 
= \sum_{i=1}^{n} \log \frac{1}{\sigma\sqrt{2\pi}} 
- \frac{1}{2\sigma^2} \sum_{i=1}^{n} (y_i - \beta^T x_i)^2
$$

The first term does **not depend on** $\beta$, so it can be ignored when optimizing.

---

## MLE Optimization

Maximizing log-likelihood is equivalent to minimizing squared error:

$$
\arg\max_{\beta} \ell(\beta)
= \arg\min_{\beta} \sum_{i=1}^{n} (y_i - \beta^T x_i)^2
$$

This is exactly the **Least Squares** objective.

---

## Conclusion

Under the assumptions:

- Linear model: $y = \beta^T x + \epsilon$
- Gaussian noise with constant variance: $\epsilon \sim \mathcal{N}(0,\sigma^2)$

**Maximum Likelihood Estimation (MLE) = Least Squares.**

# Linear Regression with Matrix Notation

## Objective

We want to find the optimal parameter vector **β** that minimizes the Residual Sum of Squares (RSS):

$$
\hat{\beta} = \arg\min_{\beta} (Y - X\beta)^T (Y - X\beta)
$$

---

## Step 1. Define RSS

$$
RSS(\beta) = (Y - X\beta)^T (Y - X\beta)
$$

---

## Step 2. Take Derivative w.r.t β

Differentiate RSS with respect to **β**:

$$
\frac{\partial RSS(\beta)}{\partial \beta}
= -X^T(Y - X\beta) - (Y - X\beta)^T X
$$

Using the identity:

$$
a^T b = b^T a
$$

we get:

$$
= -X^T(Y - X\beta) - X^T(Y - X\beta)
$$

$$
= -2X^T(Y - X\beta)
$$

---

## Step 3. Set Derivative to Zero (Optimality Condition)

$$
-2X^T(Y - X\beta) = 0
$$

$$
X^T Y - X^T X \beta = 0
$$

---

## Step 4. Solve for β

$$
X^T X \beta = X^T Y
$$

$$
\boxed{
\beta = (X^T X)^{-1} X^T Y
}
$$

This is the **Normal Equation** for Linear Regression.

---

## Dimension Check

| Term | Dimension |
|------|-----------|
| \(X\) | \(n \times d\) |
| \(Y\) | \(n \times 1\) |
| \(X^T X\) | \(d \times d\) |
| \((X^T X)^{-1}\) | \(d \times d\) |
| \(X^T Y\) | \(d \times 1\) |
| \(\beta\) | \(d \times 1\) |

---

## Scalar Version (For Single Feature)

$$
\beta_1 =
\frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}
{\sum_{i=1}^{n} (x_i - \bar{x})^2}
$$

This is equivalent to the matrix form when there is only **one feature**.

---

## Summary

- Linear regression minimizes **RSS**
- Take derivative w.r.t **β**
- Set to zero → solve linear system
- Result:

$$
\beta = (X^T X)^{-1} X^T Y
$$

---

## Notes

- Requires \(X^T X\) to be **invertible**
- If not invertible → use:
  - Pseudo-inverse
  - Ridge regression

---

# Statistical Properties of \( \hat{\beta} \)

## Model Assumption

Assume the linear model:

$$
Y = X\beta + \epsilon, \quad \epsilon \sim \mathcal{N}(0, \sigma^2 I)
$$

Then:

$$
Y|X \sim \mathcal{N}(X\beta, \sigma^2 I)
$$

---

## 1. Expectation of \( \hat{\beta} \)

$$
\hat{\beta} = (X^T X)^{-1} X^T Y
$$

Take expectation conditioned on \(X\):

$$
\mathbb{E}[\hat{\beta} | X] = \mathbb{E}[(X^T X)^{-1} X^T Y | X]
$$

Since \((X^T X)^{-1} X^T\) is constant w.r.t \(Y\):

$$
= (X^T X)^{-1} X^T \mathbb{E}[Y | X]
$$

$$
= (X^T X)^{-1} X^T (X\beta)
$$

$$
= (X^T X)^{-1} (X^T X) \beta
$$

$$
\boxed{
\mathbb{E}[\hat{\beta} | X] = \beta
}
$$

**Conclusion:** The estimator is **unbiased**.

---

## 2. Variance of \( \hat{\beta} \)

$$
\mathrm{Var}[\hat{\beta} | X] = \mathrm{Var}[(X^T X)^{-1} X^T Y | X]
$$

Using:

- \(\mathrm{Var}[AX] = A \mathrm{Var}[X] A^T\)
- \(\mathrm{Var}[Y|X] = \sigma^2 I\)

$$
= (X^T X)^{-1} X^T \mathrm{Var}[Y|X] ((X^T X)^{-1} X^T)^T
$$

$$
= (X^T X)^{-1} X^T (\sigma^2 I) X (X^T X)^{-1}
$$

$$
= \sigma^2 (X^T X)^{-1} X^T X (X^T X)^{-1}
$$

$$
\boxed{
\mathrm{Var}[\hat{\beta} | X] = \sigma^2 (X^T X)^{-1}
}
$$

---

## Interpretation

- The estimator \(\hat{\beta}\) is **unbiased**
- Variance decreases as \(X^T X\) grows (i.e., more data)
- More samples → **smaller variance → more stable estimate**

---

## Summary

| Property | Result |
|----------|--------|
| Expectation | \(\mathbb{E}[\hat{\beta}|X] = \beta\) |
| Bias | 0 (Unbiased) |
| Variance | \(\sigma^2 (X^T X)^{-1}\) |
| Effect of More Data | Smaller variance |



# Bias, Variance, and MSE

## 1. Definitions

### Bias
Bias is the systematic error of an estimator.

$$
\mathrm{Bias}(\hat{\theta}) = \mathbb{E}[\hat{\theta}] - \theta
$$

---

### Variance
Variance measures the expected squared deviation from the mean.

$$
\mathrm{Var}(\hat{\theta}) = \mathbb{E}\left[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2\right]
$$

---

### Mean Squared Error (MSE)

Mean Squared Error measures the expected squared difference between the estimator and the true value.

$$
\mathrm{MSE}(\hat{\theta}) = \mathbb{E}\left[\|\hat{\theta} - \theta\|^2\right]
= \mathbb{E}\left[\sum_{j=1}^{d} (\hat{\theta}_j - \theta_j)^2 \right]
$$

---

## 2. Bias–Variance Decomposition Theorem

**Theorem:** The MSE of an estimator equals the sum of its variance and squared bias.

$$
\mathrm{MSE}(\hat{\theta}) = \mathrm{tr}(\mathrm{Var}(\hat{\theta})) + \|\mathrm{Bias}(\hat{\theta})\|^2
$$

---

## 3. Proof (Scalar Case)

We prove (dimension-wise):

$$
\mathbb{E}\left[(\hat{\theta} - \theta)^2\right]
= \mathbb{E}\left[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2\right]
+ (\mathbb{E}[\hat{\theta}] - \theta)^2
$$

Start by expanding:

$$
\mathbb{E}\left[(\hat{\theta} - \theta)^2\right]
= \mathbb{E}\left[(\hat{\theta} - \mathbb{E}[\hat{\theta}] + \mathbb{E}[\hat{\theta}] - \theta)^2\right]
$$

Expand the square:

$$
= \mathbb{E}\left[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2
+ 2(\hat{\theta} - \mathbb{E}[\hat{\theta}])(\mathbb{E}[\hat{\theta}] - \theta)
+ (\mathbb{E}[\hat{\theta}] - \theta)^2 \right]
$$

Taking expectation:

$$
= \mathbb{E}[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2]
+ 2\mathbb{E}[(\hat{\theta} - \mathbb{E}[\hat{\theta}])](\mathbb{E}[\hat{\theta}] - \theta)
+ (\mathbb{E}[\hat{\theta}] - \theta)^2
$$

Since:

$$
\mathbb{E}[\hat{\theta} - \mathbb{E}[\hat{\theta}]] = 0
$$

we get:

$$
\mathrm{MSE}(\hat{\theta}) = \mathrm{Var}(\hat{\theta}) + \mathrm{Bias}(\hat{\theta})^2
$$

---

## 4. MSE of Linear Regression

Using the bias–variance decomposition:

$$
\mathrm{MSE}(\hat{\beta} \mid X)
= \mathrm{Var}(\hat{\beta} \mid X) + \mathrm{Bias}(\hat{\beta} \mid X)^2
$$

For ordinary least squares (OLS):

- Linear regression is **unbiased**
  
$$
\mathrm{Bias}(\hat{\beta} \mid X) = 0
$$

- Variance of the estimator:

$$
\mathrm{Var}(\hat{\beta} \mid X) = \sigma^2 (X^T X)^{-1}
$$

Therefore:

$$
\mathrm{MSE}(\hat{\beta} \mid X) = \sigma^2 (X^T X)^{-1}
$$

---

## Notes

- Linear regression estimator is unbiased.
- As sample size increases, variance decreases.
- With infinite data, variance converges to 0.
- OLS (MLE) is the **Best Linear Unbiased Estimator (BLUE)** under Gauss–Markov assumptions.








----
one-hot coding : widely-used to represent a categorical variable


interactions: (negative relation -> beta = (-) / positive relation -> beta = (+))

$$
y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \underbrace{\beta_3 x_1 x_2}_{\text{Interaction term}} + \varepsilon
$$

$$
\text{Sales} = \beta_0 + \beta_1 \cdot \text{TV} + \beta_2 \cdot \text{Radio}
+ \beta_3 \cdot \underbrace{(\text{TV} \cdot \text{Radio})}_{\text{Interaction term}} + \varepsilon
$$

## Hierarchy for Interactions

### Key Idea
- Sometimes an **interaction term** can be statistically significant even when the **main effects are not**.

---

### Hierarchy Principle
If an interaction is included in a model, the corresponding main effects **must also be included**.

Example:
- If the model contains **TV × Radio**, then both **TV** and **Radio** should be in the model.
->xyz -> xy yz xz should be in the model
---

### Why this matters
- Interaction terms are **hard to interpret without main effects**.
- The meaning of the interaction changes if main effects are omitted.
- Interaction terms implicitly contain information about main effects.

---

### Takeaway
- Always include **main effects when using interaction terms**.
- Improves **interpretability and model correctness**.


---
Nonlinear Relationships: not just an interaction between two different variables. actually we can mulitply the same variable multiple times

## Nonlinear Terms in a Linear Model

### Idea
A simple way to model **nonlinear relationships** in a linear model is to include **transformed predictors**.

---

### Example

$$
\text{mpg} = \beta_0 + \beta_1 \cdot \text{horsepower}
+ \beta_2 \cdot \text{horsepower}^2 + \varepsilon
$$

---

### Is this still a linear model?

Yes.  
It is still a **linear regression in the coefficients** because:

- Let  
$$
x_1 = \text{horsepower}, \quad
x_2 = \text{horsepower}^2
$$

- Then the model becomes
$$
y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \varepsilon
$$

So the model is **linear in parameters** even though it is **nonlinear in the original variable**.

---

### Example Coefficients

| Term | Coefficient |
|------|------------|
| Intercept | 56.9001 |
| horsepower | -0.4662 |
| horsepower² | 0.0012 |

---

### Takeaway

- Nonlinear relationship can be modeled using **polynomial terms**.
- Still a linear model if it is **linear in coefficients**.
- This is called **polynomial regression**.


---
Feature Selection
- sometimes inverse matrix do not exist
- sometimes inverse matrix value is little bit weird
 x + y = 3
 2x + 2y = 6.0000000000000000000001
 machine learning -> developer didn't select variable whether is good variable or not, just give the variable machine and it will be distinguish variable or make a decision

- Best Subset Selection
 compute all of combination of variables. 

## Best Subset Selection

### Idea
Compute least squares for **all possible subsets of predictors**, then choose the best model based on a criterion that balances **training error and model size**.

---

### Procedure

**1. Null model**

- Let M0 be the model with **no predictors**.
- It predicts the **sample mean** for all observations.

---

**2. For k = 1,2,...,p**

- Fit all models that contain **exactly k predictors**  
  Number of models:

$$
\binom{p}{k} = \frac{p!}{k!(p-k)!}
$$

- Among them, choose the best model Mk using:
  - Smallest **RSS**, or
  - Largest **R^2**

---

**3. Final model selection**

Choose the single best model among  
M0, M1, ..., Mp using a performance metric such as:

- Prediction error
- R^2
- Adjusted R^2
- AIC / BIC / Cp

---

### Key Takeaway

- Searches **all possible combinations** of predictors.
- Finds the globally best subset (for the chosen metric).
- Computationally expensive when p is large.

## Issues with Best Subset Selection

### 1. Computational limitation

- Best subset selection becomes **infeasible when the number of predictors p is large**.
- The number of possible models is:

$$
2^p
$$

- Example: when p = 40, there are **over 1 billion models**, which makes exhaustive search impractical.

---

### 2. Statistical problems when p is large

- Larger search space increases the chance of selecting models that fit **training data well but generalize poorly**.
- This phenomenon is related to the **curse of dimensionality**.
- A large model space may lead to:
  - **Overfitting**
  - **High variance** in coefficient estimates

---

### 3. Practical alternative

- **Stepwise selection methods** explore a much smaller subset of models.
- They provide a practical trade-off between:
  - Model quality
  - Computational cost


---

-Stepwise Selection (forward)

## Forward Stepwise Selection

### Idea
Starts with a model containing **no predictors**, then adds predictors **one at a time**.  
At each step, the variable that gives the **largest improvement** to the model fit is added.

---

### Procedure

**1. Null model**

- Let M0 be the model with **no predictors**.

---

**2. For k = 0,1,...,p-1**

- Consider all (p - k) models formed by adding **one new predictor** to Mk.

- Choose the best among them and call it M(k+1), where best is defined by:
  - Smallest **RSS**, or
  - Largest **R^2**

---

**3. Final model selection**

Select the single best model among  
M0, M1, ..., Mp using a metric such as:

- Prediction error
- R^2
- Adjusted R^2
- AIC / BIC / Cp

---

### Key Takeaway

- Much **faster than best subset selection**.
- Searches a **restricted set of models**.
- May not find the global best model.


Stepwise selection is computationally more efficient than best subset selection.

Complexity

Stepwise selection:
$$
O(p^2)
$$
Best subset selection:
$$
O(2^p)
$$

---

-Stepwise Selection (Backward)
Computational Advantage of Stepwise Selection

## Backward Stepwise Selection

### Idea
Starts with the **full model** containing all \( p \) predictors, then iteratively removes the **least useful predictor** one at a time.

---

### Procedure

**1. Full model**

- Let \( M_p \) be the model that contains **all \( p \) predictors**.

---

**2. For \( k = p, p-1, \dots, 1 \)**

- Consider all \( k \) models formed by removing **one predictor** from \( M_k \).  
  Each model contains \( k-1 \) predictors.

- Choose the best among them and call it \( M_{k-1} \), where best is defined by:
  - Smallest **RSS**, or
  - Largest **\( R^2 \)**

---

**3. Final model selection**

Select the single best model among  
\( M_0, M_1, \dots, M_p \) using a metric such as:

- Prediction error
- \( R^2 \)
- Adjusted \( R^2 \)
- AIC / BIC / Cp

---

### Key Takeaway

- Starts from the **full model** and removes predictors step-by-step.
- Computationally efficient compared to best subset selection.
- Requires \( n > p \) to fit the initial full model.


- (forward)stagewise selection - Usually not good

## Forward Stagewise Selection

### Idea
Add predictors **one at a time without changing previously fitted coefficients**.  
The same variable may be selected multiple times, gradually adjusting its coefficient.

---

### Procedure

**1. Null model**

- Let M0 be the model with **no predictors**.

---

**2. For k = 1, 2, ..., K (hyperparameter)**

- For each predictor j = 1, 2, ..., p:
  - Fit the model by adding a small step in coefficient for predictor j.
- Among these p candidate models, choose the best model Mk using:
  - Smallest **RSS**, or
  - Largest **R²**

---

**3. Final model selection**

Select the single best model among  
M0, M1, ..., MK using a metric such as:

- Prediction error
- R²
- Adjusted R²
- AIC / BIC / Cp

---

### Key Takeaway

- Coefficients are updated **gradually and incrementally**.
- A predictor can be chosen **multiple times**.
- Closely related to **Lasso and boosting-style methods**.
- More stable and controlled compared to forward stepwise selection.



| Method    | Coefficients Update              | Flexibility | Greediness  |
| --------- | -------------------------------- | ----------- | ----------- |
| Stepwise  | Refit all coefficients each step | Higher      | Greedy      |
| Stagewise | Only update new coefficient      | Lower       | More greedy |


---
Choosing the opitmal model

## How to Choose a Model

### Question
Which model is the best one?

Common answers:
- Smallest RSS
- Largest R^2

---

### Observation

- The model containing **all predictors** will always have:
  - The **smallest RSS**
  - The **largest R^2**
- In this sense, adding more variables seems to have nothing to lose.
- One might think: even if we include all variables, useless ones will simply get coefficients close to **zero**, so there is no harm.

However, this reasoning is **not correct for generalization**.

---

### Training vs Test Error

- Our goal is to choose a model with **low test error**, not necessarily low training error.
- Training error is usually a **poor estimate of test error**.
- A model with many variables may fit training data very well but perform poorly on unseen data.

---

### Overfitting Risk

- Using all variables can lead to **overfitting**.
- The model may capture noise instead of true signal.
- This increases **variance** and harms prediction on new data.

---

### Key Principle

- Model selection should be based on **test error (generalization performance)**.
- RSS and R^2 computed on training data alone are **not reliable criteria** for choosing among models with different numbers of predictors.


## Evaluation Strategy

### Goal

- We want to evaluate whether the model has learned **general patterns** in the data,
  rather than simply memorizing the training data.
- Similar to studying for an exam: practice on exercises to learn general skills,
  then apply them to new unseen problems.

---

### Hold-out Test Data

- We must set aside a portion of data for **testing / evaluation** (test data).
- This data **must NOT be used during training** for any purpose.
- Example: In Kaggle competitions, test labels are hidden from participants.

---

### How to Split Data

**Common approach**
- Random uniform split
- Typical ratio: **10%–20% for test set**

**Problem-dependent approach**
- Time-series / timestamp-based split:
  - Train on past data
  - Evaluate on future data
- Used to check whether the model can **predict future from past**.

---

### Key Takeaway

- Evaluation should measure **generalization**, not memorization.
- Always keep a **strictly separate test set**.
- Proper data splitting is critical for reliable model evaluation.


## Choosing Hyperparameters

### Why Hyperparameters Matter

- In many machine learning models, we must set **hyperparameters**.
- Examples:
  - Model complexity parameters (e.g., number of predictors \( p \))
  - Learning parameters (e.g., learning rate)

---

### How to Choose Hyperparameters

- Some hyperparameters may be chosen based on intuition, but **data-driven selection is usually more effective**.
- Common approach:
  - Train multiple models with different hyperparameter combinations.
  - Compare their performance and choose the best one.

---

### Validation Set

- To choose hyperparameters, we need **unseen data during training**.
- We must **NOT use the test set** for hyperparameter tuning.
- Instead, split part of the training data into a **validation set**.

---

### Key Takeaway

- Hyperparameters control **model behavior and complexity**.
- Use a **validation set** to select hyperparameters.
- Keep the **test set strictly for final evaluation only**.


---
## Cross Validation

### Procedure

**Step 1: Train using the training set**

- Use the training portion (e.g., 70%) to fit the model.

---

**Step 2: Evaluate using the validation set**

- Measure performance on the validation portion (e.g., 20%).

---

**Step 3: Hyperparameter tuning**

- Repeat Steps 1–2 with different hyperparameter settings.
- Select the best hyperparameters based on validation performance.

---

**Step 4: Retrain with best hyperparameters**

- Train the model again using **training + validation data**.

---

**Step 5: Final evaluation**

- Evaluate the final model using the **evaluation (test) set** (e.g., 10%).

---

### Important Rule

- The **evaluation/test set must NOT be used** until all training and tuning (Step 4) are complete.

---

### Typical Split Example

- Training: 70%
- Validation: 20%
- Test/Eval: 10%

---

### Key Takeaway

- Validation set is used for **model selection and hyperparameter tuning**.
- Test/Eval set is used only for **final unbiased performance estimation**.
- Ensures proper measurement of **generalization performance**.


---
## K-Fold Cross Validation

### Idea

- The dataset is split into **K equal-sized folds (blocks)**.
- The model is trained **K times**, each time using:
  - **K-1 folds for training**
  - **1 fold for validation**
- Each fold is used **once as validation**.
- The final performance is the **average of the K validation scores**.

---

### Procedure

1. Split dataset into **K folds**.
2. For each fold i = 1,...,K:
   - Train on all folds except fold i.
   - Evaluate on fold i.
3. Average the K evaluation results:

$$
\text{CV Error} = \frac{1}{K} \sum_{i=1}^{K} \text{Error}_i
$$

---

### Example (K = 5)

- Fold 1 → validation, others → training  
- Fold 2 → validation, others → training  
- Fold 3 → validation, others → training  
- Fold 4 → validation, others → training  
- Fold 5 → validation, others → training  

Each fold is used **exactly once** as validation.

---

### Key Takeaway

- Uses **all data for both training and validation** (in different rounds).
- Provides a **more stable and reliable estimate** of test error.
- Common choices:
  - K = 5
  - K = 10
