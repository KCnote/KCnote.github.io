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

parametric/non-parametric method that can be characterized

parametric: assume a functional form and predict parameters
non-parametric: not assume a functional form, get as close to the data points as possible witout being too rough or wiggly -> So need to large-scale dataset


-Supervised learning
step1. design the form of our model (ex. y = ax, inference object : a)
step2. define the goal of model
step3. find the parameters that best achieves the goal with training data.
step4. given an unseen x, estimate its label \hat{y}, --> computing.


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



-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
-----------------------
# Classification

## Qualitative Variables

- **Qualitative variables** take values in an **unordered set** $C$.

Examples:

- Eye color:
  $$
  \text{eye color} \in \{\text{brown}, \text{blue}, \text{green}\}
  $$

- Email type:
  $$
  \text{email} \in \{\text{spam}, \text{ham}\}
  $$

---

## Classification Task

- Given:
  - a **feature vector** $$\mathbf{x}$$
  - a **qualitative response** $$y$$ taking values in a set $$C$$

- The goal of **classification** is to learn a function:

  $$
  f(\mathbf{x}) \in C
  $$

- That is, the function $$f$$ takes the feature vector $$\mathbf{x}$$ as input and predicts the corresponding class label $$y$$.

---

## Probabilistic Interpretation

- In many cases, we are more interested in estimating **class probabilities** rather than just predicting a single class.

- That is, we want to estimate:

  $$
  P(y = c \mid \mathbf{x}) \quad \text{for each } c \in C
  $$

---

## Example: Fraud Detection

- For example, in insurance:
  - It is often **more valuable** to estimate the probability that a claim is fraudulent
  - than to simply classify it as **fraudulent** or **not fraudulent**

- Probabilistic outputs allow:
  - risk-based decision making
  - threshold tuning
  - cost-sensitive classification


# Can We Use Linear Regression for Classification?

## Binary Coding for Default Classification

Suppose for the **Default** classification task, we encode the response variable as:

$$
y =
\begin{cases}
0 & \text{if No} \\
1 & \text{if Yes}
\end{cases}
$$

---

## Question

Can we simply perform a **linear regression** of $y$ on $\mathbf{x}$ and classify as **Yes** if:

$$
\hat{y} > 0.5
$$

---

## Observations

- In the case of a **binary outcome**, linear regression may sometimes perform reasonably well as a classifier.
- It can be shown to be related to **Linear Discriminant Analysis (LDA)** in certain settings.
- Since in the population:

$$
\mathbb{E}[y \mid \mathbf{x}] = P(y = 1 \mid \mathbf{x})
$$

we might think regression is suitable for this task.

---

## Limitation of Linear Regression for Classification

However, **linear regression has a major drawback**:

- It may produce predicted probabilities **less than 0** or **greater than 1**.
- This makes it **inappropriate for modeling probabilities** directly.
- This limitation motivates the use of **Logistic Regression** for classification problems.

---

# Problems with Linear Regression for Classification

## Multiclass Classification Issue

Consider using **linear regression** for a classification problem where the response variable has three or more possible categories.

Example (medical diagnosis based on symptoms):

$$
y =
\begin{cases}
1 & \text{if stroke} \\
2 & \text{if drug overdose} \\
3 & \text{if epileptic seizure}
\end{cases}
$$

---

## Why This Is Problematic

- This numeric coding **implicitly introduces an ordering** among the classes.
- It suggests that:

$$
\text{difference(stroke, drug overdose)} = \text{difference(drug overdose, epileptic seizure)}
$$

- However, in reality, **these categories are not ordinal** and such numeric distances are meaningless.
- Linear regression therefore imposes an **incorrect structure** on the classification problem.

---

## Conclusion

- Linear regression is **not appropriate** for multiclass classification.
- Proper approaches include:
  - Multinomial Logistic Regression
  - Softmax-based classifiers
  - Linear Discriminant Analysis (LDA)
  - Other probabilistic classification methods

---

# Problem Setting

## Binary Classification Setup

For simplicity, we start with a **binary classification** setting:

$$
\mathbf{x} \in \mathbb{R}^d, \quad y \in \{0, 1\}
$$

---

## Modeling Class Probability

Let us consider the probability $p$ that an example belongs to **class 1**.

- We want to model:

$$
p = P(y = 1 \mid \mathbf{x})
$$

- Suppose we compute a **linear regression score** $s$.
- Our goal is to find a function that:
  - takes $s$ as input
  - outputs a valid probability $p$

---

## Desired Properties

The mapping from $s$ to $p$ should behave as follows:

- $p$ is **close to 1** if $s$ is large
- $p$ is **close to 0** if $s$ is very small (large negative)
- $p$ is **close to 0.5** if $s$ is near 0

---

## Sigmoid (Logistic) Function

The **sigmoid function** satisfies all of these properties:

$$
\sigma(s) = \frac{1}{1 + e^{-s}}
$$

- Maps any real value $s \in (-\infty, +\infty)$ to a probability in $(0,1)$
- Smooth and monotonic
- Forms the basis of **Logistic Regression**

---


# Logistic Regression

## Probability for Class 1

- Since the model output lies in the range **[0, 1]**, it can be interpreted as a **probability**.
- The probability that an input feature vector $\mathbf{x}$ belongs to **class 1** is:

$$
P(y = 1 \mid \mathbf{x}) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

---

## Probability for Class 0 (Negative Class)

- Because this is a **binary classification** problem, the probabilities must sum to 1:

$$
P(y = 1 \mid \mathbf{x}) + P(y = 0 \mid \mathbf{x}) = 1
$$

- Therefore, the probability of the negative class (class 0) is:

$$
P(y = 0 \mid \mathbf{x}) = 1 - P(y = 1 \mid \mathbf{x})
$$

- Substituting the logistic function:

$$
P(y = 0 \mid \mathbf{x})
= 1 - \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
= \frac{1}{1 + e^{\boldsymbol{\beta}^T \mathbf{x}}}
$$

---

## Interpretation

- This model is called **logistic regression**
- It models the **conditional distribution**:

$$
P(y \mid \mathbf{x})
$$

- Despite its name, logistic regression is a **classification method**, because:
  - The target variable $y$ is **discrete**
  - The output represents **class probabilities**, not continuous values

---

# Logistic Regression — Interpretation

## Model Form

The logistic regression model estimates the probability of class 1 as:

$$
P(y = 1 \mid \mathbf{x}) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x)}}
$$

where:
- $\beta_0$ is the intercept
- $\beta_1$ is the coefficient for feature $x$

---

## Coefficient Interpretation

Suppose we estimated:

- $\hat{\beta}_0 = -10.6513$
- $\hat{\beta}_1 = 0.0055$ (for **balance**)

Then:

- Since $\hat{\beta}_1 > 0$, an **increase in balance increases the probability of default**.
- At a high level, this interpretation is similar to **linear regression** (positive coefficient → positive association).

---

## Log-Odds Interpretation

Logistic regression is fundamentally a **linear model in log-odds space**.

The log-odds (logit) is defined as:

$$
\log \left( \frac{p}{1 - p} \right)
$$

Logistic regression assumes this quantity has a **linear relationship** with the input:

$$
\log \left( \frac{P(y=1 \mid x)}{1 - P(y=1 \mid x)} \right) = \beta_0 + \beta_1 x
$$

---

## Meaning of the Coefficient

- A **one-unit increase in $x$** increases the **log-odds** of class 1 by $\beta_1$.
- In this example:

$$
\text{Increase in log-odds of default} = 0.0055 \text{ per unit increase in balance}
$$

This is the precise interpretation of logistic regression coefficients.

---

## Key Insight

- Logistic regression is **linear in log-odds**, not in probability.
- The sigmoid function converts this linear log-odds into a **valid probability between 0 and 1**.
- This is why logistic regression is suitable for classification.

---
# MLE of Logistic Regression

## Objective

- Logistic regression estimates parameters **β** by **maximizing the conditional distribution**:

$$
P(y \mid \mathbf{x})
$$

- The goal is to find the **Maximum Likelihood Estimator (MLE)** of the parameter vector $\boldsymbol{\beta}$.

---

## Likelihood Function

For a binary response $y_i \in \{0,1\}$, the model defines:

$$
P(y_i = 1 \mid \mathbf{x}_i) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}
$$

and

$$
P(y_i = 0 \mid \mathbf{x}_i) = 1 - P(y_i = 1 \mid \mathbf{x}_i)
$$

---

## Likelihood of the Dataset

Given $n$ independent observations $\{(\mathbf{x}_i, y_i)\}_{i=1}^n$, the likelihood function is:

$$
\mathcal{L}(\boldsymbol{\beta})
= \prod_{i: y_i = 1} P(y_i \mid \mathbf{x}_i)
  \prod_{i: y_i = 0} \left(1 - P(y_i \mid \mathbf{x}_i)\right)
$$

This likelihood represents the probability of observing the given zeros and ones in the dataset.

---

## Maximum Likelihood Estimation

- We choose $\boldsymbol{\beta}$ to **maximize** the likelihood:

$$
\hat{\boldsymbol{\beta}} = \arg\max_{\boldsymbol{\beta}} \; \mathcal{L}(\boldsymbol{\beta})
$$

- Equivalently, we often maximize the **log-likelihood** for numerical stability and convenience.

---

## Key Idea

- Logistic regression does **not** minimize squared error.
- Instead, it maximizes the likelihood of the observed class labels.
- This leads naturally to the **log-loss / cross-entropy loss** used in classification.

---

# MLE of Logistic Regression — Log-Likelihood Derivation

## Likelihood Function

For binary outcomes $y_i \in \{0,1\}$, the likelihood function of logistic regression is:

$$
\mathcal{L}(\boldsymbol{\beta})
= \prod_{i: y_i = 1} p(y_i \mid \mathbf{x}_i)
  \prod_{i: y_i = 0} \left(1 - p(y_i \mid \mathbf{x}_i)\right)
$$

where

$$
p(y_i = 1 \mid \mathbf{x}_i) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}
$$

---

## Log-Likelihood

Taking the logarithm of the likelihood:

$$
\log \mathcal{L}(\boldsymbol{\beta})
= \log \prod_{i: y_i = 1} p(y_i \mid \mathbf{x}_i)
+ \log \prod_{i: y_i = 0} \left(1 - p(y_i \mid \mathbf{x}_i)\right)
$$

Using the property $\log \prod = \sum \log$, we obtain:

$$
= \sum_{i: y_i = 1} \log p(y_i \mid \mathbf{x}_i)
+ \sum_{i: y_i = 0} \log \left(1 - p(y_i \mid \mathbf{x}_i)\right)
$$

---

## Unified Expression

We can write both cases in a single summation:

$$
\log \mathcal{L}(\boldsymbol{\beta})
= \sum_{i=1}^{n}
\left[
y_i \log p(y_i \mid \mathbf{x}_i)
+ (1 - y_i) \log \left(1 - p(y_i \mid \mathbf{x}_i)\right)
\right]
$$

---

## Substituting the Logistic Function

Substituting:

$$
p(y_i \mid \mathbf{x}_i) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}
$$

gives:

$$
\log \mathcal{L}(\boldsymbol{\beta})
= \sum_{i=1}^{n}
\left[
- y_i \log \left(1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}\right)
- (1 - y_i) \log \left(1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i}\right)
\right]
$$

---

## Key Takeaway

- Logistic regression parameters are estimated by **maximizing the log-likelihood**.
- The resulting objective is **concave**, ensuring a unique global optimum.
- The negative log-likelihood corresponds to the **binary cross-entropy loss**.

---

# MLE of Logistic Regression — No Closed-Form Solution

## Maximum Likelihood Estimator for $\boldsymbol{\beta}$

From the previous derivation, the log-likelihood is:

$$
\log \mathcal{L}(\boldsymbol{\beta})
= \sum_{i=1}^{n}
\left[
- y_i \log(1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i})
- (1 - y_i) \log(1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i})
\right]
$$

---

## Optimality Condition

To find the MLE, we set the gradient to zero:

$$
\frac{\partial}{\partial \boldsymbol{\beta}} \log \mathcal{L}(\boldsymbol{\beta}) = 0
$$

This leads to the following equation:

$$
- \sum_{i=1}^{n} \frac{\mathbf{x}_i y_i}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}
- \sum_{i=1}^{n} \frac{\mathbf{x}_i (1 - y_i)}{1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i}}
= 0
$$

---

## No Closed-Form Solution

- Unlike **linear regression**, this equation **cannot be solved analytically**.
- There is **no closed-form solution** for $\boldsymbol{\beta}$.

---

## What Do We Do Instead?

We must use **numerical optimization methods**, such as:

- Gradient Descent
- Stochastic Gradient Descent (SGD)
- Newton–Raphson / Iteratively Reweighted Least Squares (IRLS)
- Quasi-Newton methods (e.g., BFGS, L-BFGS)

These methods iteratively update $\boldsymbol{\beta}$ to maximize the log-likelihood.

---

## Key Insight

- Logistic regression is **convex**, so optimization converges to a **unique global optimum**.
- Even without a closed-form solution, the model is **efficiently solvable** in practice.

---

# Optimization

## What is Optimization?

According to Wikipedia:

> **Mathematical optimization** (or mathematical programming) is the selection of a **best element**,
> with regard to **some criterion**, from some set of available alternatives.

---

## Key Components of Optimization

Every optimization problem consists of the following elements:

### 1. Candidates (Alternatives)
- A set of possible solutions
- Examples:
  - Model parameters
  - Actions
  - Decisions

---

### 2. Criterion (Objective Function)
- A function that measures how good a candidate is
- Commonly written as:

$$
\mathcal{L}(\hat{y}, y)
$$

or

$$
J(\theta)
$$

- Examples:
  - Loss function
  - Cost function
  - Likelihood function

---

### 3. Best Element

- The goal is to find the element that **minimizes** or **maximizes** the criterion:

$$
\theta^* = \arg\min_{\theta} J(\theta)
$$

or

$$
\theta^* = \arg\max_{\theta} J(\theta)
$$

---

## Optimization in Machine Learning

In machine learning:

- The **alternatives** are model parameters
- The **criterion** is usually a loss function
- Optimization is the process of learning parameters that best fit the data

Examples:
- Linear regression → minimize squared error
- Logistic regression → maximize likelihood (or minimize cross-entropy)

---

# Primitive Ideas in Optimization

Before using advanced optimization algorithms, several simple and intuitive strategies can be considered.

---

## Basic Approaches

### 1. Exhaustive Search
- Try **all possible combinations** of parameters
- Guarantees finding the global optimum
- Computationally expensive and usually impractical for large problems

---

### 2. Random Search
- Randomly sample candidate solutions
- Often surprisingly effective in high-dimensional spaces
- Simple but not guaranteed to find the optimum

---

### 3. Visualization
- Visualize the objective function (when possible)
- Helps understand:
  - Shape of the loss surface
  - Local minima / maxima
  - Convexity / non-convexity

---

### 4. Greedy Search
- Optimize **one variable at a time**
- Iteratively improve the objective
- Simple but may get stuck in local optima

---

## Key Insight

These primitive ideas help build intuition for more advanced optimization methods such as:

- Gradient Descent
- Stochastic Gradient Descent (SGD)
- Newton’s Method
- Convex Optimization Techniques

---
# Following the Slope — Gradient Descent

## Objective

We have a **cost function** (or loss function) $\mathcal{L}(\boldsymbol{\beta})$ that we want to **minimize**.

$$
\min_{\boldsymbol{\beta}} \; \mathcal{L}(\boldsymbol{\beta})
$$

---

## Core Idea

At the current parameter value $\boldsymbol{\beta}$:

1. Compute the **gradient** of the loss:
   
   $$
   \nabla_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})
   $$

2. Take a **small step in the direction of the negative gradient**.

3. Repeat until convergence.

---

## Update Rule

The standard **gradient descent update** is:

$$
\boldsymbol{\beta}^{(t+1)}
=
\boldsymbol{\beta}^{(t)}
-
\eta \, \nabla_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta}^{(t)})
$$

where:

- $\eta$ = learning rate (step size)
- $\nabla_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})$ = gradient
- Iteratively moves toward the **minimum of the loss function**

---

## Why Follow the Negative Gradient?

- The gradient points in the direction of **steepest increase**.
- Moving in the **negative gradient direction** gives the **steepest decrease**.
- This leads the parameters toward the **minimum** of the function.

---

## Interpretation

- Gradient is the **best linear approximation** of the function at a point.
- Each update improves the parameter slightly.
- Starting from a random initial value, repeated updates move toward the minimum.

---

## Key Insight

Gradient descent is the foundation of most machine learning optimization methods, including:

- Linear Regression
- Logistic Regression
- Neural Networks
- Deep Learning

---
# Gradient Descent

## Update Equation (Vector Form)

The gradient descent update rule in vector notation is:

$$
\boldsymbol{\beta}^{\text{new}} = \boldsymbol{\beta}^{\text{old}} - \alpha \, \nabla_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})
$$

where:

- $\alpha$ = learning rate (step size)
- $\nabla_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})$ = gradient of the loss
- Moves parameters in the direction of **steepest decrease**

---

## Update Equation (Single Parameter)

For a single parameter $\beta_i$:

$$
\beta_i^{\text{new}} = \beta_i^{\text{old}} - \alpha \, \frac{\partial}{\partial \beta_i} \mathcal{L}(\boldsymbol{\beta})
$$

- If slope $< 0$ → parameter increases
- If slope $> 0$ → parameter decreases

---

## Pseudocode

```python
beta = rand(vector)

while True:
    beta_grad = evaluate_gradient(L, data, beta)
    beta = beta - alpha * beta_grad

    if norm(beta_grad) <= threshold:
        break
```

---

## Intuition

- The gradient indicates the direction of **steepest ascent**
- Moving in the **negative gradient** direction reduces the loss
- Iteratively updating parameters leads toward a **minimum**

---

## Notes

- Proper choice of learning rate $\alpha$ is critical:
  - Too small → slow convergence
  - Too large → divergence or oscillation
- Convergence is often detected when the gradient norm becomes very small

---

# Gradient Descent: Potential Problems

Gradient descent is a powerful optimization method, but it has several limitations and challenges.

---

## 1. Non-Convex Surface

- The loss surface may **not be convex**.
- This means:
  - There may be **multiple local minima**
  - There may be **saddle points**
- The gradient can be **zero even when not at the global minimum**.

---

## 2. Requires Differentiable Cost Function

Gradient descent works only when the cost function is **differentiable**.

- Some loss functions (e.g., **0/1 loss**) are **not differentiable**.
- In practice, we often use a **smooth surrogate loss**, such as:
  - Logistic loss
  - Cross-entropy loss
  - Squared error loss

---

## 3. Slow Convergence

- Gradient descent can be **slow**, especially for large datasets.
- The gradient is computed using **all data samples**, so updates use the **average gradient**.
- For very large datasets:
  - Computing gradients over all data points can be **computationally expensive**.
  - Convergence to a (local) minimum may take a long time.

---

## Practical Improvements

To address these issues, modern optimization methods are often used:

- Stochastic Gradient Descent (SGD)
- Mini-batch Gradient Descent
- Momentum
- Adam / RMSProp
- Second-order methods (Newton, Quasi-Newton)

---

# Stochastic Gradient Descent (SGD)

## Core Idea

Instead of computing gradients using **all training examples**, compute them using only a **randomly sampled subset** of the data.

---

## Variants

### 1. Pure Stochastic Gradient Descent
- Update parameters using **one single sample at a time**
- Fast updates but very noisy gradients

---

### 2. Mini-batch Gradient Descent (Most Common)

- Use a **mini-batch** of samples to estimate the gradient
- Typical batch sizes:
  
  $$
  32, 64, 128, 256, \dots, 8192
  $$

- The optimal batch size depends on:
  - The problem
  - The dataset
  - Hardware constraints (especially memory)

---

## Effect of Batch Size

- When the mini-batch is **small**:
  - Doubling the batch size significantly stabilizes the gradient estimate
- When the mini-batch becomes **large**:
  - Improvement becomes smaller
  - Computational cost increases roughly linearly
  - This is known as **diminishing returns**

---

## Advantages of SGD

- Much faster than full gradient descent for large datasets
- Allows scalable training on massive data
- Often helps escape shallow local minima or saddle points due to noise

---

## Practical Notes

- Mini-batch SGD is the **standard method** in deep learning
- Often combined with:
  - Momentum
  - Adam
  - RMSProp
  - Learning rate scheduling

---

# Revised Supervised Learning Framework

## Overview

Machine learning is a **data-driven approach**.  
Rather than manually specifying rules, we design a model and let data determine the best parameters.

---

## Step-by-Step Framework

### Step 1. Model Design
- Humans design the **form of the model**
- Example (logistic regression):

$$
P(y = 1 \mid \mathbf{x}) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

---

### Step 2. Define the Learning Goal
- Define what the model should achieve
- Typically:

$$
y \approx f(\mathbf{x})
$$

as accurately as possible

---

### Step 3. Learn Parameters from Training Data

Given training data:

$$
\{(\mathbf{x}_i, y_i)\}_{i=1}^{n}
$$

we aim to find the **best parameters** $\boldsymbol{\beta}$.

#### Step 3-1. Prediction
- Feed training input $\mathbf{x}$ into the model
- Obtain a prediction $\hat{y}$

---

#### Step 3-2. Loss Evaluation
- Compare the prediction $\hat{y}$ with the ground-truth label $y$
- Measure how good or bad the prediction is using a **loss function**

> This comparison step defines the **loss function**.

For logistic regression, this loss is known as **log-loss** (cross-entropy):

$$
\ell(y, \hat{y})
=
- y \log \hat{y}
- (1 - y) \log (1 - \hat{y})
$$

---

#### Step 3-3. Parameter Update
- Update parameters $\boldsymbol{\beta}$ to reduce the loss
- Typically done using **gradient descent** or its variants

---

#### Step 3-4. Iteration
- Repeat prediction → loss computation → parameter update
- Continue until:

$$
\hat{y} \approx y
$$

for most training samples

---

### Step 4. Inference (Prediction on Unseen Data)

- Given a new, unseen input $\mathbf{x}$
- Estimate its label $\hat{y}$ by computing:

$$
P(y \mid \mathbf{x})
$$

---

## Key Insight

- The **loss function** connects prediction and optimization
- Model training is essentially:
  - **Minimizing loss**
  - **Learning parameters from data**
- Logistic regression uses **log-loss** as its optimization objective

---

# Extension of Logistic Regression

## Logistic Regression Model

The logistic regression model for binary classification is:

$$
P(y = 1 \mid \mathbf{x}) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

For a single input variable, this becomes:

$$
P(y = 1 \mid x) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x)}}
$$

Example coefficient interpretation:

- Intercept: $\beta_0 = -10.6513$
- Balance coefficient: $\beta_1 = 0.0055$

---

## Limitation of Basic Logistic Regression

So far, we assumed:

- Only **one input variable** $x$
- A **binary classification** problem with **two classes** (positive and negative)

---

## Extension Questions

Can logistic regression be extended to:

- Multiple input variables (**$p > 1$ features**)?
- Multi-class classification (**$K > 2$ classes**)?

---

## Yes — Logistic Regression Can Be Extended

### Multiple Input Variables

For multiple features:

$$
\mathbf{x} = (x_1, x_2, \dots, x_p)
$$

the model becomes:

$$
P(y = 1 \mid \mathbf{x}) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

where:

$$
\boldsymbol{\beta} = (\beta_0, \beta_1, \dots, \beta_p)
$$

---

### Multi-Class Classification

For more than two classes ($K > 2$), logistic regression is extended using:

- **Multinomial Logistic Regression**
- **Softmax Function**

The probability of class $k$ is:

$$
P(y = k \mid \mathbf{x}) =
\frac{e^{\boldsymbol{\beta}_k^T \mathbf{x}}}
{\sum_{j=1}^{K} e^{\boldsymbol{\beta}_j^T \mathbf{x}}}
$$

---

## Key Insight

- Logistic regression naturally extends to **multiple features**
- Multi-class problems are handled using **softmax / multinomial logistic regression**
- This forms the foundation of many modern classification models

---

# Logistic Regression with 2+ Variables

## Linear Model for Log-Odds

Similarly to the linear regression case, logistic regression builds a **linear model for the log-odds**.

The log-odds (logit) is defined as:

$$
\log \left( \frac{p(\mathbf{x}; \boldsymbol{\beta})}{1 - p(\mathbf{x}; \boldsymbol{\beta})} \right)
$$

For $p$ input variables, we model this quantity linearly as:

$$
\log \left( \frac{p(\mathbf{x}; \boldsymbol{\beta})}{1 - p(\mathbf{x}; \boldsymbol{\beta})} \right)
=
\beta_0 + \beta_1 x_1 + \cdots + \beta_p x_p
$$

---

## Number of Parameters

- The model contains **$p + 1$ parameters**:
  - $\beta_0$ : intercept
  - $\beta_1, \ldots, \beta_p$ : feature coefficients
- Here, $p$ is the number of input variables (features)

---

## Probability Form

By applying the sigmoid function, we obtain the probability form:

$$
p(\mathbf{x}; \boldsymbol{\beta})
=
\frac{e^{\beta_0 + \beta_1 x_1 + \cdots + \beta_p x_p}}
{1 + e^{\beta_0 + \beta_1 x_1 + \cdots + \beta_p x_p}}
$$

---

## Vector Notation

Using vector notation:

$$
\mathbf{x} = (1, x_1, x_2, \ldots, x_p)
$$

$$
\boldsymbol{\beta} = (\beta_0, \beta_1, \ldots, \beta_p)
$$

the model can be written compactly as:

$$
P(y = 1 \mid \mathbf{x})
=
\frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

> Here, both $\mathbf{x}$ and $\boldsymbol{\beta}$ are **$(p+1)$-dimensional**.

---

## Key Insight

- Logistic regression naturally generalizes to **any number of input variables**
- The model form remains unchanged; only the **dimension of $\mathbf{x}$ and $\boldsymbol{\beta}$ increases**
- This formulation is the foundation for high-dimensional classification models

---

# Logistic Regression with 3+ Classes

This document presents a **step-by-step derivation** showing how binary logistic regression naturally extends to **multi-class classification**, leading to the **softmax function**.

---

## 1. Review: Binary Logistic Regression

Binary (2-class) logistic regression can be written in two equivalent forms.

### 1.1 Log-Odds (Logit) Form

The log-odds are linearly related to the input:

$$
\log \left( \frac{p(y=1 \mid \mathbf{x})}{1 - p(y=1 \mid \mathbf{x})} \right)
= \boldsymbol{\beta}^T \mathbf{x}
$$

---

### 1.2 Probability (Sigmoid) Form

The conditional probabilities are modeled using the sigmoid function:

$$
p(y=1 \mid \mathbf{x}) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

$$
p(y=0 \mid \mathbf{x}) = \frac{1}{1 + e^{\boldsymbol{\beta}^T \mathbf{x}}}
$$

---

## 2. The Multi-Class Challenge

Now suppose we have **three classes instead of two**.

- We cannot directly combine:
  
$$
p(y=0 \mid \mathbf{x}), \; p(y=1 \mid \mathbf{x}), \; p(y=2 \mid \mathbf{x})
$$

- The sigmoid formulation is inherently **binary**
- A new formulation is required

---

## 3. Introducing a Common Denominator

To prepare for multi-class extension, we rewrite the binary case using a **shared denominator**.

### 3.1 Rewriting \( p(y=1 \mid x) \)

Multiply numerator and denominator by  
$$
e^{\frac{1}{2}\boldsymbol{\beta}^T \mathbf{x}}
$$

$$
p(y=1 \mid \mathbf{x})
=
\frac{e^{\frac{1}{2}\boldsymbol{\beta}^T \mathbf{x}}}
{e^{\frac{1}{2}\boldsymbol{\beta}^T \mathbf{x}} + e^{-\frac{1}{2}\boldsymbol{\beta}^T \mathbf{x}}}
$$

---

### 3.2 Rewriting \( p(y=0 \mid x) \)

Similarly, multiply numerator and denominator by  
$$
e^{-\frac{1}{2}\boldsymbol{\beta}^T \mathbf{x}}
$$

$$
p(y=0 \mid \mathbf{x})
=
\frac{e^{-\frac{1}{2}\boldsymbol{\beta}^T \mathbf{x}}}
{e^{\frac{1}{2}\boldsymbol{\beta}^T \mathbf{x}} + e^{-\frac{1}{2}\boldsymbol{\beta}^T \mathbf{x}}}
$$

---

## 4. Renaming Parameters (Score Interpretation)

Define new parameters:

$$
\boldsymbol{\beta}_1' = \frac{1}{2}\boldsymbol{\beta},
\quad
\boldsymbol{\beta}_0' = -\frac{1}{2}\boldsymbol{\beta}
$$

Then:

$$
p(y=1 \mid \mathbf{x}) =
\frac{e^{\boldsymbol{\beta}_1'^T \mathbf{x}}}
{e^{\boldsymbol{\beta}_0'^T \mathbf{x}} + e^{\boldsymbol{\beta}_1'^T \mathbf{x}}}
$$

$$
p(y=0 \mid \mathbf{x}) =
\frac{e^{\boldsymbol{\beta}_0'^T \mathbf{x}}}
{e^{\boldsymbol{\beta}_0'^T \mathbf{x}} + e^{\boldsymbol{\beta}_1'^T \mathbf{x}}}
$$

Interpretation:
- Each class has a **linear score**
- Probabilities are obtained by **normalizing exponentiated scores**

---

## 5. Natural Extension to 3+ Classes

For **K classes**, define a score for each class:

$$
s_i = \boldsymbol{\beta}_i^T \mathbf{x}
$$

The probability of class \( i \) is:

$$
p(y=i \mid \mathbf{x})
=
\frac{e^{s_i}}
{e^{s_1} + e^{s_2} + \cdots + e^{s_K}}
=
\frac{e^{s_i}}{\sum_{j=1}^{K} e^{s_j}}
$$

This is known as the **Softmax Function**.

---

## 6. Probability Validity

The softmax function satisfies all probability requirements:

- **Non-negativity**:
  
$$
0 \le p(y=i \mid \mathbf{x}) \le 1
$$

- **Normalization**:

$$
\sum_{i=1}^{K} p(y=i \mid \mathbf{x}) = 1
$$

---

## 7. Key Takeaways

- Sigmoid is a **special case of softmax** with \( K=2 \)
- Softmax provides a **principled extension** of logistic regression
- Each class has its own parameter vector
- Widely used in:
  - Multinomial logistic regression
  - Neural networks
  - Deep learning classifiers

---

# Summary of Logistic Regression

## Maximum Likelihood Estimation (MLE)

Logistic regression learns parameters by **maximizing the conditional likelihood**:

$$
\hat{\boldsymbol{\beta}} = \arg\max_{\boldsymbol{\beta}} \sum_{i=1}^{n} \log p(y_i \mid \mathbf{x}_i)
$$

This is called the **conditional log-likelihood**.

---

## Expanding the Likelihood

Using the logistic model:

$$
p(y=1 \mid \mathbf{x}) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

$$
p(y=0 \mid \mathbf{x}) = \frac{1}{1 + e^{\boldsymbol{\beta}^T \mathbf{x}}}
$$

the objective becomes:

$$
\hat{\boldsymbol{\beta}} =
\arg\max_{\boldsymbol{\beta}}
\sum_{i=1}^{N}
\left[
y_i \log \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}
+ (1 - y_i) \log \frac{1}{1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i}}
\right]
$$

---

## Equivalent Minimization Form

Maximizing the log-likelihood is equivalent to **minimizing the negative log-likelihood**:

$$
\hat{\boldsymbol{\beta}} =
\arg\min_{\boldsymbol{\beta}}
\sum_{i=1}^{N}
\left[
- y_i \log(1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i})
- (1 - y_i) \log(1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i})
\right]
$$

---

## Log-Loss (Binary Cross-Entropy)

The objective above is known as the **log-loss** (or **binary cross-entropy**):

$$
\ell(\boldsymbol{\beta})
=
\sum_{i=1}^{N}
\left[
- y_i \log(1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i})
- (1 - y_i) \log(1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i})
\right]
$$

---

## Key Insight

- Logistic regression **maximizes conditional likelihood**
- Equivalent to **minimizing log-loss**
- Optimization is typically done using:
  - Gradient descent
  - Stochastic gradient descent
  - Newton / quasi-Newton methods

---

--------------------------------------
----------
# Classification Problem

## Problem Definition

Given a feature vector **x** and a qualitative response **y** taking values in a set **C**,  
the classification task is to build a function:

$$
f(\mathbf{x})
$$

that takes the feature vector **x** as input and predicts its label **y**, i.e.,

$$
f(\mathbf{x}) \in C
$$

---

## Training Data Assumption

We usually assume that we have **n training samples** drawn from some probability distribution:

$$
p(\mathbf{x}, y)
$$

$$
(\mathbf{x}^{(i)}, y^{(i)}), \quad i = 1, \dots, n
$$

These samples are assumed to be **i.i.d.**

---

## What Does i.i.d. Mean?

**i.i.d. = independent and identically distributed**

This assumption has two parts:

### 1. Independent
Each sample is independent of the others:

$$
p((\mathbf{x}^{(1)}, y^{(1)}), \dots, (\mathbf{x}^{(n)}, y^{(n)}))
= \prod_{i=1}^{n} p(\mathbf{x}^{(i)}, y^{(i)})
$$

- Knowing one sample gives **no information** about another
- Simplifies likelihood and optimization

---

### 2. Identically Distributed
All samples come from the **same underlying distribution**:

$$
(\mathbf{x}^{(1)}, y^{(1)}) \sim p(\mathbf{x}, y), \; \dots, \; (\mathbf{x}^{(n)}, y^{(n)}) \sim p(\mathbf{x}, y)
$$

- Training and test data are assumed to follow the **same distribution**
- This is critical for **generalization**

---

## Classification as Function Approximation

Classification can be viewed as **function approximation**:

$$
\mathbf{x} \rightarrow y
$$

We approximate this mapping using:

$$
f(\mathbf{x})
$$

---

## Binary Classification Setup

In this course, we focus on **binary classification**, where:

$$
y \in \{+1, -1\}
$$

---

## Key Insight

- Classification learns a mapping from input **x** to label **y**
- Training data are assumed to be **i.i.d.**
- The i.i.d. assumption enables:
  - Likelihood factorization
  - Statistical learning theory
  - Reliable generalization

---


# Risk & Bayes Classifier

## Risk = Expected Loss

Risk is defined as the **expected loss** of a classifier.

$$
R(f) = \mathbb{E}_{p(x,y)}[\mathcal{L}(y, f(x))]
$$

$$
R(f) = \sum_x \sum_y p(x,y)\mathcal{L}(y, f(x))
$$

The **Bayes classifier** is the classifier that minimizes this risk:

$$
f^* = \arg\min_f \; \mathbb{E}_{p(x,y)}[\mathcal{L}(y, f(x))]
$$

$$
f^*(x) = \arg\min_{\hat{y}} \; \mathbb{E}_{p(y|x)}[\mathcal{L}(y, \hat{y})]
= \arg\min_{\hat{y}} \sum_y p(y|x)\mathcal{L}(y, \hat{y})
$$

---

## Binary Classification Case

Assume:

$$
y \in \{+1, -1\}
$$

Then:

$$
f^*(x) = \arg\min_{\hat{y}} \sum_y p(y|x)\mathcal{L}(y, \hat{y})
$$

Expanding:

$$
f^*(x) =
\arg\min_{\hat{y}} \big[
p(y=1|x)\mathcal{L}(1,\hat{y})
+
p(y=-1|x)\mathcal{L}(-1,\hat{y})
\big]
$$

Decision rule:

- If $$p(y=1|x) > p(y=-1|x)$$ → predict **+1**
- If $$p(y=1|x) < p(y=-1|x)$$ → predict **-1**

---

## Log-Odds Form

$$
f^*(x) = \text{sign}\left(
\log \frac{p(y=1|x)}{p(y=-1|x)}
\right)
$$

Interpretation:

- If log-odds > 0 → class +1
- If log-odds < 0 → class -1

---

## Using Bayes Rule

Posterior:

$$
p(y|x) = \frac{p(x|y)p(y)}{p(x)}
$$

Substitute into log-odds:

$$
f^*(x) =
\text{sign}\left(
\log \frac{p(x|y=1)p(y=1)}
{p(x|y=-1)p(y=-1)}
\right)
$$

Since $$p(x)$$ cancels out, we only need:

- Likelihood: $$p(x|y)$$
- Prior: $$p(y)$$

---

## Key Concepts

- **Risk** = Expected loss
- **Bayes classifier** = Minimizes expected loss
- Optimal decision = choose class with largest posterior probability
- Log-odds converts probability comparison into linear decision rule
- Bayes rule converts posterior into likelihood × prior

---

## Connection to Logistic Regression

Logistic regression models:

$$
\log \frac{p(y=1|x)}{p(y=-1|x)} = w^T x + b
$$

So logistic regression is a **parametric approximation of the Bayes optimal classifier** under a linear log-odds assumption.


bayes classifier는 진짜 확률분포를 알고있다는 가정하에 최적의 바운더리를 알게된다

# Classification Strategies → Discriminant Analysis (Gaussian) → 1D Full Derivation + MLE

> Based on the provided slides (Classification Strategies + Discriminant
> Analysis + 1D expansion + MLE for Gaussian parameters).\
> All math uses `$$ ... $$` (not `\[` `\]`).

------------------------------------------------------------------------

## 0. Setup and Notation

-   Input: $$x$$ (first treat as **scalar**; later can generalize)
-   Label: $$y \in \{+1,-1\}$$
-   Prior:
    -   $$\alpha = p(y=+1)$$
    -   $$1-\alpha = p(y=-1)$$
-   Likelihood model (Gaussian):
    -   $$p(x\mid y=+1) = \mathcal{N}(x\mid \mu_{1}, \sigma_{1}^{2})$$
    -   $$p(x\mid y=-1) = \mathcal{N}(x\mid \mu_{-1}, \sigma_{-1}^{2})$$

Gaussian pdf (1D):

$$
\mathcal{N}(x\mid \mu,\sigma^2)
= \frac{1}{\sqrt{2\pi\sigma^2}}
\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

------------------------------------------------------------------------

## 1. Classification Strategies (Generative vs Discriminative)

We want a classifier that minimizes risk (expected loss):

$$
f^*=\arg\min_f\;\mathbb{E}_{p(x,y)}[\mathcal{L}(y,f(x))]
= \arg\min_f\sum_x\sum_y p(x,y)\mathcal{L}(y,f(x))
$$

But we usually do **not** know $$p(x,y)$$, so we estimate it from
training data.

### Two strategies to predict $$p(y\mid x)$$

**(A) Generative classifier** - Estimate $$p(x\mid y)$$ and $$p(y)$$
from data (often via MLE) - Then compute posterior via Bayes rule:

$$
p(y\mid x)=\frac{p(x\mid y)p(y)}{p(x)}
$$

Example: Bayes classifier / Discriminant Analysis (LDA/QDA).

**(B) Discriminative classifier** - Directly estimate $$p(y\mid x)$$
without Bayes rule

Example: Logistic regression.

------------------------------------------------------------------------

## 2. Discriminant Analysis: Bayes Decision Rule

Bayes classifier (0--1 loss) predicts the class with largest posterior:

$$
f^*(x)=\arg\max_{y\in\{+1,-1\}} p(y\mid x)
$$

Using Bayes rule and cancelling $$p(x)$$, this becomes:

$$
f^*(x)=\arg\max_{y\in\{+1,-1\}} p(x\mid y)p(y)
$$

Equivalently, in **log-likelihood ratio** form:

$$
f^*(x)
= \text{sign}\left(
\log\frac{p(x\mid y=+1)p(y=+1)}{p(x\mid y=-1)p(y=-1)}
\right)
$$

Substitute priors:

$$
f^*(x)
= \text{sign}\left(
\log\frac{p(x\mid y=+1)\alpha}{p(x\mid y=-1)(1-\alpha)}
\right)
$$

If the expression inside `sign( )` is \> 0 → predict +1, otherwise -1.

------------------------------------------------------------------------

## 3. Plugging in Gaussian Likelihoods (1D)

Assume Gaussian likelihoods:

$$
p(x\mid y=+1)=\mathcal{N}(x\mid \mu_1,\sigma_1^2),\quad
p(x\mid y=-1)=\mathcal{N}(x\mid \mu_{-1},\sigma_{-1}^2)
$$

Then:

$$
f^*(x)
= \text{sign}\left(
\log
\frac{\mathcal{N}(x\mid \mu_1,\sigma_1^2)\alpha}
{\mathcal{N}(x\mid \mu_{-1},\sigma_{-1}^2)(1-\alpha)}
\right)
$$

Now expand the Gaussian ratio step-by-step.

### 3.1 Write the ratio explicitly

$$
\frac{\mathcal{N}(x\mid \mu_1,\sigma_1^2)}
{\mathcal{N}(x\mid \mu_{-1},\sigma_{-1}^2)}
=
\frac{\frac{1}{\sqrt{2\pi\sigma_1^2}}\exp\left(-\frac{(x-\mu_1)^2}{2\sigma_1^2}\right)}
{\frac{1}{\sqrt{2\pi\sigma_{-1}^2}}\exp\left(-\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}\right)}
$$

Cancel $$\sqrt{2\pi}$$:

$$
=
\frac{\frac{1}{\sqrt{\sigma_1^2}}\exp\left(-\frac{(x-\mu_1)^2}{2\sigma_1^2}\right)}
{\frac{1}{\sqrt{\sigma_{-1}^2}}\exp\left(-\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}\right)}
=
\frac{\sqrt{\sigma_{-1}^2}}{\sqrt{\sigma_1^2}}
\cdot
\exp\left(
-\frac{(x-\mu_1)^2}{2\sigma_1^2}
+
\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}
\right)
$$

### 3.2 Take log and include priors

Define discriminant value:

$$
g(x)
=
\log
\frac{\mathcal{N}(x\mid \mu_1,\sigma_1^2)\alpha}
{\mathcal{N}(x\mid \mu_{-1},\sigma_{-1}^2)(1-\alpha)}
$$

Then:

$$
g(x)
=
\log\left(\frac{\sqrt{\sigma_{-1}^2}}{\sqrt{\sigma_1^2}}\right)
+
\left(
-\frac{(x-\mu_1)^2}{2\sigma_1^2}
+
\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}
\right)
+
\log\left(\frac{\alpha}{1-\alpha}\right)
$$

Use $$\log(\sqrt{a})=\frac12\log(a)$$:

$$
\log\left(\frac{\sqrt{\sigma_{-1}^2}}{\sqrt{\sigma_1^2}}\right)
=
\frac12\log\left(\frac{\sigma_{-1}^2}{\sigma_1^2}\right)
=
-\frac12\log\left(\frac{\sigma_1^2}{\sigma_{-1}^2}\right)
$$

So an equivalent form (matching the slide style) is:

$$
g(x)
=
-\frac12\log\left(\frac{\sigma_{1}^2}{\sigma_{-1}^2}\right)
-\frac{(x-\mu_1)^2}{2\sigma_1^2}
+\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}
+
\log\left(\frac{\alpha}{1-\alpha}\right)
$$

Decision rule:

$$
f^*(x)=\text{sign}(g(x))
$$

> This is the **fully expanded** 1D Gaussian Bayes discriminant (no
> omission).

------------------------------------------------------------------------

## 4. Maximum Likelihood Estimation (MLE) of Parameters

We are given training set $$\{(x_i,y_i)\}_{i=1}^n$$.

Let: - $$\mathcal{I}_1 = \{i: y_i=+1\}$$, size
$$n_1 = |\mathcal{I}_1|$$ - $$\mathcal{I}_{-1} = \{i: y_i=-1\}$$, size
$$n_{-1} = |\mathcal{I}_{-1}|$$ - $$n = n_1 + n_{-1}$$

### 4.1 MLE for prior $$\alpha$$

Likelihood for labels under Bernoulli prior:

$$
p(\{y_i\}) = \alpha^{n_1}(1-\alpha)^{n_{-1}}
$$

Log-likelihood:

$$
\ell(\alpha)= n_1\log\alpha + n_{-1}\log(1-\alpha)
$$

Differentiate and set to 0:

$$
\frac{d\ell}{d\alpha} = \frac{n_1}{\alpha} - \frac{n_{-1}}{1-\alpha} = 0
$$

Solve:

$$
n_1(1-\alpha)=n_{-1}\alpha
\Rightarrow n_1 = (n_1+n_{-1})\alpha = n\alpha
\Rightarrow \hat{\alpha} = \frac{n_1}{n}
$$

So $$p(y=-1)=1-\hat{\alpha}=\frac{n_{-1}}{n}$$.

------------------------------------------------------------------------

## 4.2 MLE for $$\mu_1$$ (mean of positive class)

For class +1, conditional likelihood:

$$
\prod_{i\in\mathcal{I}_1}
\frac{1}{\sqrt{2\pi\sigma_1^2}}
\exp\left(-\frac{(x_i-\mu_1)^2}{2\sigma_1^2}\right)
$$

Log-likelihood (dropping constants not depending on $$\mu_1$$):

$$
\ell(\mu_1) = -\sum_{i\in\mathcal{I}_1}\frac{(x_i-\mu_1)^2}{2\sigma_1^2}
$$

Differentiate:

$$
\frac{\partial \ell}{\partial \mu_1}
=
-\sum_{i\in\mathcal{I}_1}\frac{1}{2\sigma_1^2}\cdot 2(x_i-\mu_1)(-1)
=
\sum_{i\in\mathcal{I}_1}\frac{x_i-\mu_1}{\sigma_1^2}
$$

Set to 0:

$$
\sum_{i\in\mathcal{I}_1}(x_i-\mu_1)=0
\Rightarrow
\sum_{i\in\mathcal{I}_1}x_i - n_1\mu_1 = 0
\Rightarrow
\hat{\mu}_1 = \frac{1}{n_1}\sum_{i\in\mathcal{I}_1}x_i
$$

> This matches the slide note: the summation is only over examples in
> the positive class.

Similarly:

$$
\hat{\mu}_{-1} = \frac{1}{n_{-1}}\sum_{i\in\mathcal{I}_{-1}}x_i
$$

------------------------------------------------------------------------

## 4.3 MLE for $$\sigma_1^2$$ (variance of positive class)

For class +1, full log-likelihood terms involving $$\sigma_1^2$$:

$$
\ell(\sigma_1^2)
=
\sum_{i\in\mathcal{I}_1}
\left(
-\frac12\log(2\pi\sigma_1^2)
-\frac{(x_i-\mu_1)^2}{2\sigma_1^2}
\right)
$$

Differentiate w.r.t. $$\sigma_1^2$$.

First term:

$$
\frac{\partial}{\partial \sigma_1^2}\left(-\frac12\log(2\pi\sigma_1^2)\right)
= -\frac12\cdot\frac{1}{\sigma_1^2}
$$

Second term:

$$
\frac{\partial}{\partial \sigma_1^2}
\left(
-\frac{(x_i-\mu_1)^2}{2\sigma_1^2}
\right)
=
-\frac{(x_i-\mu_1)^2}{2}\cdot \frac{\partial}{\partial \sigma_1^2}(\sigma_1^{-2})
=
-\frac{(x_i-\mu_1)^2}{2}\cdot (-1)\sigma_1^{-4}
=
\frac{(x_i-\mu_1)^2}{2(\sigma_1^2)^2}
$$

Combine for all $$i\in\mathcal{I}_1$$:

$$
\frac{\partial \ell}{\partial \sigma_1^2}
=
\sum_{i\in\mathcal{I}_1}
\left(
-\frac{1}{2\sigma_1^2}
+
\frac{(x_i-\mu_1)^2}{2(\sigma_1^2)^2}
\right)
$$

Set to 0 and multiply by $$2(\sigma_1^2)^2$$:

$$
\sum_{i\in\mathcal{I}_1}
\left(
-(\sigma_1^2)
+
(x_i-\mu_1)^2
\right)=0
$$

So:

$$
\sum_{i\in\mathcal{I}_1}(x_i-\mu_1)^2 = n_1\sigma_1^2
\Rightarrow
\hat{\sigma}_1^2 = \frac{1}{n_1}\sum_{i\in\mathcal{I}_1}(x_i-\hat{\mu}_1)^2
$$

Similarly:

$$
\hat{\sigma}_{-1}^2 = \frac{1}{n_{-1}}\sum_{i\in\mathcal{I}_{-1}}(x_i-\hat{\mu}_{-1})^2
$$

Note: This is the **MLE** variance 
dividing by 
$$n_k$$
, not
 $$n_k-1$$

------------------------------------------------------------------------

## 5. Summary (What the slides conclude)

-   $$\hat{\alpha}
 = \frac{n_1}{n}$$ and
    $$1-\hat{\alpha}=\frac{n_{-1}}{n}$$
-   
 are class-wise sample means $$\hat{\mu}_1,\hat{\mu}_{-1}$$
-   
 
are class-wise MLE $$\hat{\sigma}_1^2,\hat{\sigma}_{-1}^2$$
    variances
-   Plug these into:

$$
g(x)
=
-\frac12\log\left(\frac{\sigma_{1}^2}{\sigma_{-1}^2}\right)
-\frac{(x-\mu_1)^2}{2\sigma_1^2}
+\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}
+
\log\left(\frac{\alpha}{1-\alpha}\right)
$$

and predict:

$$
f^*(x)=\text{sign}(g(x))
$$

------------------------------------------------------------------------

# Appendix: Same derivation in "ratio" form (often shown on slides)

Start from:

$$
f^*(x)=\text{sign}\left(\log\frac{\mathcal{N}(x|\mu_1,\sigma_1^2)\alpha}{\mathcal{N}(x|\mu_{-1},\sigma_{-1}^2)(1-\alpha)}\right)
$$

Expand the log into sum of logs:

$$
= \text{sign}\left(
\log\mathcal{N}(x|\mu_1,\sigma_1^2)
-\log\mathcal{N}(x|\mu_{-1},\sigma_{-1}^2)
+\log\alpha-\log(1-\alpha)
\right)
$$

Then substitute:

$$
\log\mathcal{N}(x|\mu,\sigma^2)
=
-\frac12\log(2\pi\sigma^2)
-\frac{(x-\mu)^2}{2\sigma^2}
$$

to obtain exactly the expanded discriminant above.


# Linear Discriminant Analysis (LDA) --- Full Algebraic Derivation

This document fully reconstructs the **exact algebraic expansion** shown
in the slides:

-   Start from Gaussian discriminant
-   Assume equal variances
-   Expand quadratic terms
-   Show cancellation → **linear decision boundary**
-   Compare with QDA

All formulas use `$$`.

------------------------------------------------------------------------

# 1. Start from Gaussian Bayes Discriminant (1D)

Recall from Gaussian discriminant analysis:

$$
f^*(x)
= \text{sign}\left(
-\frac12\log\frac{\sigma_1^2}{\sigma_{-1}^2}
-\frac{(x-\mu_1)^2}{2\sigma_1^2}
+\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}
+ \log\frac{\alpha}{1-\alpha}
\right)
$$

------------------------------------------------------------------------

# 2. LDA Assumption (Equal Variances)

Linear Discriminant Analysis assumes:

$$
\sigma_1^2 = \sigma_{-1}^2 = \sigma^2
$$

Substitute into the discriminant:

$$
f^*(x)
= \text{sign}\left(
-\frac12\log\frac{\sigma^2}{\sigma^2}
-\frac{(x-\mu_1)^2}{2\sigma^2}
+\frac{(x-\mu_{-1})^2}{2\sigma^2}
+ \log\frac{\alpha}{1-\alpha}
\right)
$$

Since $$\log(\sigma^2/\sigma^2)=\log(1)=0$$:

$$
f^*(x)
= \text{sign}\left(
-\frac{(x-\mu_1)^2}{2\sigma^2}
+\frac{(x-\mu_{-1})^2}{2\sigma^2}
+ \log\frac{\alpha}{1-\alpha}
\right)
$$

Factor $$\frac{1}{2\sigma^2}$$:

$$
f^*(x)
= \text{sign}\left(
\frac{1}{2\sigma^2}\big[(x-\mu_{-1})^2-(x-\mu_1)^2\big]
+ \log\frac{\alpha}{1-\alpha}
\right)
$$

------------------------------------------------------------------------

# 3. Expand the Quadratic Terms

Expand both squares:

$$
(x-\mu_1)^2 = x^2 - 2\mu_1 x + \mu_1^2
$$

$$
(x-\mu_{-1})^2 = x^2 - 2\mu_{-1} x + \mu_{-1}^2
$$

Substitute into the difference:

$$
(x-\mu_{-1})^2-(x-\mu_1)^2
= (x^2 - 2\mu_{-1}x + \mu_{-1}^2) - (x^2 - 2\mu_1x + \mu_1^2)
$$

Cancel $$x^2$$:

$$
= -2\mu_{-1}x + \mu_{-1}^2 + 2\mu_1x - \mu_1^2
$$

Group terms:

$$
= 2(\mu_1-\mu_{-1})x + (\mu_{-1}^2-\mu_1^2)
$$

------------------------------------------------------------------------

# 4. Substitute Back into Discriminant

$$
f^*(x)
= \text{sign}\left(
\frac{1}{2\sigma^2}\left[2(\mu_1-\mu_{-1})x + (\mu_{-1}^2-\mu_1^2)\right]
+ \log\frac{\alpha}{1-\alpha}
\right)
$$

Distribute factor:

$$
= \text{sign}\left(
\frac{\mu_1-\mu_{-1}}{\sigma^2}x
+ \frac{\mu_{-1}^2-\mu_1^2}{2\sigma^2}
+ \log\frac{\alpha}{1-\alpha}
\right)
$$

Rearrange constant term:

$$
= \text{sign}\left(
\frac{\mu_1-\mu_{-1}}{\sigma^2}x
- \frac{\mu_1^2-\mu_{-1}^2}{2\sigma^2}
+ \log\frac{\alpha}{1-\alpha}
\right)
$$

------------------------------------------------------------------------

# 5. Final Linear Form

Define:

$$
w = \frac{\mu_1-\mu_{-1}}{\sigma^2}
$$

$$
b = -\frac{\mu_1^2-\mu_{-1}^2}{2\sigma^2} + \log\frac{\alpha}{1-\alpha}
$$

Then:

$$
f^*(x) = \text{sign}(wx + b)
$$

👉 The decision boundary is **linear**.

------------------------------------------------------------------------

# 6. Linear vs Quadratic Discriminant Analysis

## QDA (Different Variances)

If:

$$
\sigma_1^2 \neq \sigma_{-1}^2
$$

Then quadratic terms remain:

$$
(x-\mu_k)^2
$$

→ Decision boundary becomes **quadratic polynomial in x**.

------------------------------------------------------------------------

## LDA (Equal Variances)

If:

$$
\sigma_1^2 = \sigma_{-1}^2
$$

Quadratic terms cancel → boundary becomes **linear**:

$$
f^*(x) = \text{sign}(wx+b)
$$

------------------------------------------------------------------------

# 7. Interpretation

-   LDA assumes **same variance/covariance** across classes
-   Leads to **linear boundary**
-   QDA allows different variances → **curved boundary**
-   LDA is more stable when data small
-   QDA more flexible but higher variance

------------------------------------------------------------------------

# End


# Discriminant Analysis (Multi‑Dimensional, Multi‑Class, Softmax, LDA vs Logistic) --- Full Mathematical Notes

This document reconstructs **all mathematical content** from the slides:

-   Multivariate Gaussian density
-   Discriminant function in d-dim
-   Multi‑class LDA derivation
-   From discriminant scores → probabilities (softmax)
-   LDA example interpretation
-   Why discriminant analysis
-   LDA vs Logistic regression (mathematical connection)

------------------------------------------------------------------------

# 1. Multivariate Gaussian Distribution

For class $$k$$ in $$d$$‑dimensional space:

$$
p(x \mid y=k) = \mathcal{N}(x \mid \mu_k, \Sigma_k)
$$

Density:

$$
\mathcal{N}(x \mid \mu, \Sigma)
= (2\pi)^{-d/2} |\Sigma|^{-1/2}
\exp\left(
-\frac12 (x-\mu)^T \Sigma^{-1} (x-\mu)
\right)
$$

Where:

-   $$x \in \mathbb{R}^d$$
-   mean vector $$\mu_k$$
-   covariance matrix $$\Sigma_k$$

------------------------------------------------------------------------

# 2. Discriminant Function (Multivariate Case)

Bayes rule with Gaussian likelihood:

$$
\delta_k(x) = \log p(x \mid y=k) + \log \alpha_k
$$

Substitute Gaussian density:

$$
\delta_k(x)
=
-\frac12 \log |\Sigma_k|
-\frac12 (x-\mu_k)^T \Sigma_k^{-1} (x-\mu_k)
+ \log \alpha_k
$$

------------------------------------------------------------------------

## 2.1 Expand Quadratic Term

$$
(x-\mu_k)^T \Sigma_k^{-1} (x-\mu_k)
=
x^T \Sigma_k^{-1} x
- 2\mu_k^T \Sigma_k^{-1} x
+ \mu_k^T \Sigma_k^{-1} \mu_k
$$

Substitute:

$$
\delta_k(x)
=
-\frac12 \log |\Sigma_k|
-\frac12 x^T \Sigma_k^{-1} x
+ \mu_k^T \Sigma_k^{-1} x
-\frac12 \mu_k^T \Sigma_k^{-1} \mu_k
+ \log \alpha_k
$$

------------------------------------------------------------------------

# 3. LDA Assumption (Shared Covariance)

If:

$$
\Sigma_k = \Sigma
$$

Then term $$-\frac12 x^T \Sigma^{-1} x$$ is identical for all classes →
cancels in comparison.

Thus the discriminant simplifies to:

$$
\delta_k(x)
=
x^T \Sigma^{-1} \mu_k
-\frac12 \mu_k^T \Sigma^{-1} \mu_k
+ \log \alpha_k
$$

This is **linear in x**.

------------------------------------------------------------------------

# 4. Multi‑Class Classification

For $$K$$ classes:

$$
\hat{y} = \arg\max_k \delta_k(x)
$$

Equivalent form (comparing pairwise):

For any classes $$i,j$$:

$$
\delta_i(x) - \delta_j(x) > 0
$$

→ choose class $$i$$.

------------------------------------------------------------------------

# 5. From Discriminant Scores → Probabilities (Softmax)

Convert scores into posterior estimates:

$$
P(Y=k \mid X=x)
=
\frac{e^{\delta_k(x)}}
{\sum_{\ell=1}^{K} e^{\delta_\ell(x)}}
$$

Prediction:

$$
\hat{y} = \arg\max_k P(Y=k \mid X=x)
$$

------------------------------------------------------------------------

# 6. LDA Example Interpretation

Assume:

$$
\alpha_1 = \alpha_2 = \alpha_3
$$

Then only the Gaussian term matters.

-   Each ellipse = Gaussian contour
-   Same covariance → same shape/size ellipses
-   LDA boundary = linear hyperplanes
-   True Bayes boundary may be nonlinear

------------------------------------------------------------------------

# 7. Why Discriminant Analysis?

## 7.1 Stability with well‑separated classes

When classes are far apart:

-   Logistic regression parameters unstable
-   LDA remains stable

Reason: few points near boundary → logistic likelihood flat.

------------------------------------------------------------------------

## 7.2 Small sample size

If $$n$$ small and Gaussian assumption reasonable:

LDA often more stable than logistic regression.

------------------------------------------------------------------------

## 7.3 Multi‑class naturally supported

LDA handles $$K>2$$ easily using discriminant scores.

------------------------------------------------------------------------

# 8. LDA vs Logistic Regression (Mathematical Connection)

For two classes, LDA yields:

$$
\log \frac{p(y=1 \mid x)}{p(y=-1 \mid x)}
= w^T x + b
$$

Same functional form as logistic regression.

Difference:

-   Logistic regression maximizes conditional likelihood:

$$
\prod_i p(y_i \mid x_i)
$$

-   LDA maximizes joint likelihood:

$$
\prod_i p(x_i, y_i)
$$

Despite different estimation, predictions often similar.

------------------------------------------------------------------------

## 8.1 Logistic Regression can approximate QDA

If quadratic features included:

$$
x_1^2, x_2^2, x_1x_2
$$

→ logistic model can produce quadratic boundary similar to QDA.

------------------------------------------------------------------------

# End

# Credit Card Default --- Confusion Matrix, Error Types, Threshold, ROC/AUC (Full Notes)

This note reconstructs and **extends** the slide mathematically:

-   Confusion matrix & misclassification rate
-   Error types (FP, FN, TP, TN)
-   Threshold effect on error trade‑off
-   ROC curve & AUC
-   Added: precision, recall, F1, balanced error, optimal threshold
    intuition

All math uses `$$`.

------------------------------------------------------------------------

# 1. Confusion Matrix (Credit Card Default)

             True No   True Yes   Total
  ---------- --------- ---------- -------
  Pred No    9644      252        9896
  Pred Yes   23        81         104
  Total      9667      333        10000

Definitions:

$$
TN = 9644,\quad FP = 23,\quad FN = 252,\quad TP = 81
$$

------------------------------------------------------------------------

# 2. Misclassification Rate

$$
\text{Error} = \frac{FP + FN}{N}
= \frac{23 + 252}{10000}
= 0.0275 = 2.75\%
$$

------------------------------------------------------------------------

## Baseline (Always predict No)

Errors = number of Yes samples:

$$
\frac{333}{10000} = 3.33\%
$$

Thus LDA improves over naive baseline.

------------------------------------------------------------------------

# 3. Error Types

## False Positive Rate (FPR)

$$
\text{FPR} = \frac{FP}{FP + TN}
= \frac{23}{9667}
\approx 0.0024 = 0.24\%
$$

------------------------------------------------------------------------

## False Negative Rate (FNR)

$$
\text{FNR} = \frac{FN}{FN + TP}
= \frac{252}{333}
\approx 0.757 = 75.7\%
$$

Interpretation: many defaulters missed.

------------------------------------------------------------------------

## True Positive Rate (Recall / Sensitivity)

$$
\text{TPR} = \frac{TP}{TP + FN}
= 1 - \text{FNR}
$$

------------------------------------------------------------------------

## True Negative Rate (Specificity)

$$
\text{TNR} = \frac{TN}{TN + FP}
= 1 - \text{FPR}
$$

------------------------------------------------------------------------

# 4. Decision Rule via Probability Threshold

Prediction rule:

$$
\hat y =
\begin{cases}
1 & \text{if } P(Default=Yes \mid x) \ge t \\
0 & \text{otherwise}
\end{cases}
$$

Default threshold:

$$
t = 0.5
$$

Changing $$t$$ trades off FN vs FP.

------------------------------------------------------------------------

# 5. Threshold Trade‑off

Lower threshold:

-   increases TP
-   decreases FN
-   increases FP

Higher threshold:

-   decreases FP
-   increases FN

Thus:

$$
\text{Cannot minimize FP and FN simultaneously}
$$

------------------------------------------------------------------------

# 6. Overall Error vs Threshold

Let:

$$
\text{Error}(t) = \frac{FP(t) + FN(t)}{N}
$$

Behavior:

-   Small $$t$$ → high FP
-   Large $$t$$ → high FN
-   Middle $$t$$ minimizes total error

------------------------------------------------------------------------

# 7. ROC Curve

ROC plots:

$$
(\text{FPR}(t),\ \text{TPR}(t))
$$

for all thresholds $$t \in [0,1]$$.

Properties:

-   left‑top corner = perfect classifier
-   diagonal = random guessing

------------------------------------------------------------------------

## Area Under Curve (AUC)

$$
\text{AUC} = \int_0^1 \text{TPR}(u)\, d(\text{FPR}(u))
$$

Interpretation:

$$
\text{AUC} = P(\text{model ranks positive higher than negative})
$$

Higher AUC → better model.

------------------------------------------------------------------------

# 8. Additional Metrics (Useful in Practice)

## Precision

$$
\text{Precision} = \frac{TP}{TP + FP}
$$

## Recall

$$
\text{Recall} = \text{TPR}
$$

## F1 Score

$$
F1 = \frac{2 \cdot \text{Precision} \cdot \text{Recall}}
{\text{Precision} + \text{Recall}}
$$

------------------------------------------------------------------------

## Balanced Error Rate

$$
\text{BER} = \frac{\text{FPR} + \text{FNR}}{2}
$$

Useful for imbalanced data (like default detection).

------------------------------------------------------------------------

# 9. Choosing Optimal Threshold

Common criteria:

### Minimize total error

$$
t^* = \arg\min_t \text{Error}(t)
$$

### Maximize F1

$$
t^* = \arg\max_t F1(t)
$$

### Cost‑sensitive threshold

If FN more expensive:

choose lower $$t$$.

------------------------------------------------------------------------

# 10. Key Insight

-   Accuracy alone is misleading for imbalanced data
-   ROC/AUC evaluate ranking ability independent of threshold
-   Threshold selection depends on application cost

------------------------------------------------------------------------

# End


# Naive Bayes --- Full Mathematical Notes (Assumption, Example, Practical Issues)

This document reconstructs and **extends all slides mathematically**:

-   Bayes decision rule
-   Naive conditional independence assumption
-   Why Naive Bayes works despite wrong assumption
-   Full Weather dataset numerical example
-   Zero‑frequency problem (Laplace smoothing)
-   Log‑probability trick (numerical stability)

All math uses `$$`.

------------------------------------------------------------------------

# 1. Bayes Optimal Classifier

Bayes risk:

$$
f^* = \arg\min_f \mathbb{E}_{p(x,y)}[\mathcal{L}(y, f(x))]
$$

Equivalent:

$$
f^*(x) = \arg\max_y p(y \mid x)
$$

Using Bayes rule:

$$
p(y \mid x) = \frac{p(x \mid y)p(y)}{p(x)}
$$

Since $$p(x)$$ constant across classes:

$$
f^*(x) = \arg\max_y p(x \mid y)p(y)
$$

------------------------------------------------------------------------

# 2. Naive Bayes Assumption

For $$x = (x_1,\dots,x_d)$$:

$$
p(x \mid y) = \prod_{j=1}^{d} p(x_j \mid y)
$$

This assumes **conditional independence** given class.

------------------------------------------------------------------------

# 3. Why Naive Bayes Works (Even When False)

Although independence rarely holds:

-   True $$p(x|y)$$ inaccurate
-   But ranking of classes often preserved:

$$
p(x|y_1)p(y_1) > p(x|y_2)p(y_2)
$$

Still correct classification.

This is why Naive Bayes performs surprisingly well.

------------------------------------------------------------------------

# 4. Weather Dataset Example

Goal:

$$
P(\text{PlayGolf}=yes \mid x)
\quad vs \quad
P(\text{PlayGolf}=no \mid x)
$$

Given:

$$
x = (\text{Outlook=sunny, Temp=Hot, Humidity=High, Windy=False})
$$

------------------------------------------------------------------------

## 4.1 Prior Probabilities

From dataset:

$$
P(yes) = \frac{9}{14}, \quad
P(no) = \frac{5}{14}
$$

------------------------------------------------------------------------

## 4.2 Likelihood (Naive Factorization)

### For class = yes

$$
P(x|yes)
= P(\text{sunny}|yes)
P(\text{Hot}|yes)
P(\text{High}|yes)
P(\text{False}|yes)
$$

From counts:

$$
= \frac{3}{9} \cdot \frac{2}{9} \cdot \frac{3}{9} \cdot \frac{6}{9}
\approx 0.016
$$

Posterior (unnormalized):

$$
P(yes|x) \propto 0.016 \cdot \frac{9}{14}
$$

------------------------------------------------------------------------

### For class = no

$$
P(x|no)
= P(\text{sunny}|no)
P(\text{Hot}|no)
P(\text{High}|no)
P(\text{False}|no)
$$

From counts:

$$
= \frac{2}{5} \cdot \frac{2}{5} \cdot \frac{4}{5} \cdot \frac{2}{5}
\approx 0.051
$$

Posterior:

$$
P(no|x) \propto 0.051 \cdot \frac{5}{14}
$$

------------------------------------------------------------------------

## 4.3 Prediction

Since:

$$
P(no|x) > P(yes|x)
$$

Prediction:

$$
\hat y = no
$$

------------------------------------------------------------------------

# 5. Zero‑Frequency Problem

If any term:

$$
p(x_j|y) = 0
$$

Then:

$$
p(x|y) = 0
$$

→ catastrophic failure.

------------------------------------------------------------------------

## 5.1 Laplace Smoothing

Add small constant $$\epsilon$$ (usually 1):

$$
p(x_j|y) = \frac{count(x_j,y) + \epsilon}{count(y) + \epsilon K}
$$

Prevents zero probabilities.

------------------------------------------------------------------------

# 6. Numerical Stability (Log Trick)

Product becomes extremely small:

$$
p(x|y) = \prod_j p(x_j|y)
$$

Instead compute:

$$
\log p(x|y) = \sum_j \log p(x_j|y)
$$

Decision rule unchanged:

$$
\arg\max_y \log p(x|y) + \log p(y)
$$

------------------------------------------------------------------------

# 7. Key Insights

-   Naive Bayes assumes independence → rarely true
-   Still works due to correct class ranking
-   Laplace smoothing solves zero‑probability issue
-   Log‑probability avoids underflow
-   Extremely fast and effective for text classification

------------------------------------------------------------------------

# End
-----------
--------------
----------------
----------------
-----------------
-----------------
-----------------
[ML/DL] Lecture 7. Overfitting & Regularization

# Training vs Validation Curves

## 1. Overview

Training and validation curves are fundamental tools to understand how a machine learning model learns over time.

They help diagnose:

- Underfitting
- Overfitting
- Training bugs
- Optimization issues

Typically we monitor **loss vs training step (or epoch)**.

---

## 2. Typical Behavior

### Training Curve

- Training loss should **consistently decrease** as training progresses.
- This indicates the model is successfully fitting the training data.
- Small fluctuations are normal, but **overall downward trend is required**.

### Validation Curve

- Validation loss usually:
  1. Decreases at early stage
  2. Reaches a minimum
  3. Starts increasing again

This happens because the model begins to **overfit** the training data.

---

## 3. Overfitting Pattern

Typical pattern:

- Training loss → keeps decreasing
- Validation loss → decreases then increases

This means:

> Model memorizes training data instead of learning general patterns.

Solution approaches:

- Early stopping
- Regularization (L2, dropout)
- More data
- Simpler model

---

## 4. If Training Loss Does NOT Decrease

If training loss:

- stays flat
- oscillates randomly
- increases

Then **something is wrong**.

### Possible Causes (Bug / Setup Issue)

1. Learning rate too high → divergence
2. Learning rate too low → no learning
3. Gradient not flowing (e.g. wrong graph / detach)
4. Loss not connected to model parameters
5. Wrong label / data mismatch
6. Data normalization problem
7. Model stuck in saturation (e.g. sigmoid overflow)
8. Optimizer not updating (zero grad missing, wrong step order)

### Important Rule

> If training loss does not decrease over time, **assume there is a bug until proven otherwise**.

A correctly implemented model **must reduce training loss** unless:

- Data is pure noise
- Model capacity is too small

---

## 5. Ideal Training Scenario

Good training behavior:

- Training loss ↓ steadily
- Validation loss ↓ then stable or slightly ↑
- Gap between train and val not too large

Bad signs:

- Train ↓ but Val ↑ fast → Overfitting
- Train not decreasing → Bug / Optimization failure
- Both high → Underfitting

---

## 6. Practical Debug Checklist

If training loss not decreasing:

- Check gradient magnitude
- Print loss each step
- Try very small dataset (overfit test)
- Reduce learning rate
- Verify labels
- Disable regularization temporarily
- Check optimizer step order

---

## 7. Key Takeaway

- Training loss **must go down**
- Validation loss **indicates generalization**
- If training loss doesn't decrease → **debug first, tune later**

# Overfitting and High-Dimensional Data

## 1. Goal of Machine Learning

The goal is **not to perform well only on training data**, but to perform well **in general**.

We care about:

> Performance on unseen test data (generalization)

---

## 2. Why Does Evaluation Loss Get Worse?

During early training:

- The model learns **important and general patterns**
- Training loss ↓
- Validation (evaluation) loss ↓

After some point:

- The model begins to learn **noise specific to the training data**
- Training loss ↓ further
- Evaluation loss ↑

This phenomenon is called:

> **Overfitting**

---

## 3. What is Overfitting?

Overfitting occurs when:

- The model learns **not only the general trend**
- But also the **noise in the training data**

Behavior:

- Training performance → keeps improving
- Evaluation performance → gets worse

This means the model is **memorizing**, not generalizing.

---

## 4. Loss Curve Interpretation

| Phase | Training Loss | Eval Loss | Meaning |
|------|--------------|-----------|---------|
| Early | ↓ | ↓ | Learning useful patterns |
| Optimal | Low | Lowest | Best generalization |
| Overfitting | ↓ | ↑ | Learning noise |

---

## 5. Overfitting is Worse in High-Dimensional Data

Overfitting becomes more severe when:

- Feature dimension is high
- Data contains noise
- Dataset is small

---

## 6. What Does "High-Dimensional" Really Mean?

High-dimensional means:

> The number of features (dimension) is large.

Example:

- 2D → 2 features
- 100D → 100 features
- 1000D → extremely high dimensional

---

## 7. Curse of Dimensionality

As dimension increases:

- The input space grows **exponentially**
- Data becomes sparse
- The model needs **exponentially more data**

### Key Insight

If dimension increases by **n times**,  
the required dataset size must grow approximately:

> **Exponential in n**

Example intuition:

- 1D → need 100 samples
- 10D → need ~10^10 scale coverage (conceptually)
- 100D → practically impossible without huge data

This is known as:

> **Curse of Dimensionality**

---

## 8. Why High-Dimensional Data Causes Overfitting

When dimension is high but data is limited:

- The model can easily fit noise
- Many different models explain training data equally well
- Generalization becomes poor

Result:

- Training loss ↓
- Eval loss ↑

Classic overfitting pattern.

---

## 9. Practical Implication

High-dimensional models require:

- Much larger dataset
- Strong regularization
- Feature selection / dimensionality reduction
- Careful validation

---

## 10. How to Reduce Overfitting

- Early stopping
- L2 regularization
- Dropout
- More data
- Simpler model
- Feature selection
- PCA / dimensionality reduction

---

## 11. Core Takeaway

- Overfitting = learning noise instead of signal
- Happens when model is too flexible or data is insufficient
- High dimension requires **exponentially more data**
- Always monitor validation loss

> Stop training at the point where evaluation loss is minimal.


# Model Capacity, Overfitting, and Underfitting

## 1. Overfitting and Model Capacity

Overfitting is **strongly related to model capacity**.

- If a model is **too complex**, it tends to overfit.
- If a model is **too simple**, it underfits.

---

## 2. What is Model Capacity?

Model capacity refers to:

> How complex patterns a model can represent.

Examples:

- Linear model → Low capacity
- Polynomial model → Medium capacity
- Deep neural network → Very high capacity

Higher capacity → More flexible → Can fit more complex data  
But also → Can memorize noise → Overfitting risk

---

## 3. Overfitting (High Capacity)

When the model is **unnecessarily complex**:

- It has enough capacity to **memorize noise**
- Training loss becomes very low
- But generalization becomes poor

Behavior:

- Fits training data perfectly
- Decision boundary becomes highly irregular
- Captures noise instead of true structure

---

## 4. Underfitting (Low Capacity)

When the model has **insufficient capacity**:

- Cannot learn even the general pattern
- Training loss remains high
- Validation loss also high

Behavior:

- Model too simple
- Misses important structure in data
- Poor performance everywhere

---

## 5. Capacity vs Fit Visualization (Concept)

### Low Capacity → Underfitting
- Linear boundary
- Cannot separate classes well
- High bias

### Proper Capacity → Good Fit
- Smooth boundary
- Captures real pattern
- Best generalization

### High Capacity → Overfitting
- Very complex boundary
- Fits noise and outliers
- High variance

---

## 6. Mathematical Perspective

Increasing model complexity:

----

# 📘 Full Notes — Regularization, Overfitting, Ridge & Lasso

---

## 🎯 Goal of Learning — Generalization

The objective of machine learning is **not to minimize training error**, but to **generalize well to unseen data**.

Typical behavior:

$$
\text{Training loss} \downarrow,\quad \text{Validation loss} \uparrow
$$

⚠️ This indicates **Overfitting** — the model starts memorizing noise instead of learning true patterns.

---

## 📉 Small Data Problem (e.g., Video Summarization)

Some problems have extremely limited data:

- 🎬 Video summarization  
- 🧬 Medical / rare events  
- 💸 Expensive labeling  

Even collecting **30 samples** can be costly.

### Risks
- Cross‑validation may become **misleading**
- Overfitting is hard to detect
- High variance in evaluation

### Practical Strategy
- ✔ Strict validation  
- ✔ Train **multiple capacity models**  
- ✔ Estimate overfitting onset  
- ✔ Avoid trusting a single run  

---

## ⏹️ Early Stopping

Under i.i.d. assumption, overfitting tends to begin at similar times across splits.

$$
\text{Stop at } \arg\min_{\text{epoch}} \text{Validation Metric}
$$

⚠️ Notes:

- Surrogate loss ≠ true metric  
- Metrics may overfit at different points  
- Use **most important real metric**  

---

## 🧠 Model Capacity

| Capacity | Behavior |
|----------|----------|
| 🔹 Low | Underfitting |
| ⭐ Optimal | Best Generalization |
| 🔺 High | Overfitting |

Bias–Variance:

| Capacity | Bias | Variance |
|----------|------|----------|
| Low | High | Low |
| Optimal | Balanced | Balanced |
| High | Low | High |

---

## 📏 Regularization / Shrinkage

General form:

$$
\min_{\beta} RSS + \lambda \cdot \text{Penalty}(\beta)
$$

Goal:

$$
\text{Reduce variance by shrinking coefficients } \beta
$$

---

## 🔵 Ridge Regression (L2)

Objective:

$$
\hat{\beta} = \arg\min_\beta \sum_{i=1}^{n}(y_i - x_i^T\beta)^2 + \lambda \|\beta\|_2^2
$$

Matrix solution:

$$
\hat{\beta} = (X^T X + \lambda I)^{-1} X^T Y
$$

### Properties
- Shrinks coefficients toward 0  
- Rarely exactly 0  
- Keeps all features  
- Variance ↓, Bias ↑  

Limits:

$$
\lambda = 0 \Rightarrow \text{OLS}
$$

$$
\lambda \to \infty \Rightarrow \beta \to 0
$$

---

## ⚖️ Scaling of Predictors (Critical)

Ridge is **scale sensitive**, therefore predictors must be standardized:

$$
\tilde{x}_{ij} = \frac{x_{ij} - \bar{x}_j}{\sqrt{\frac{1}{n}\sum_{i=1}^{n}(x_{ij}-\bar{x}_j)^2}}
$$

Why?

Penalty term:

$$
\lambda \sum_{j=1}^{p} \beta_j^2
$$

- Large‑scale feature → Larger penalty ❌  
- Small‑scale feature → Smaller penalty ❌  

✔ Scaling ensures **fair regularization**.

---

## 📊 Bias–Variance (Ridge Insight)

$$
\text{MSE} = \text{Bias}^2 + \text{Variance} + \sigma^2
$$

As $$\lambda$$ increases:

- Variance ↓  
- Bias ↑  
- Optimal $$\lambda$$ exists ⭐  

---

## 🔶 Lasso (L1)

Objective:

$$
\hat{\beta} = \arg\min_\beta \sum_{i=1}^{n}(y_i - x_i^T\beta)^2 + \lambda \|\beta\|_1
$$

$$
\|\beta\|_1 = \sum_{j=1}^{p} |\beta_j|
$$

### Properties
- Some coefficients become **exactly 0**  
- Produces **Sparse Model**  
- Performs feature selection  

---

## 💎 Why Lasso Produces Zeros

Constraint:

$$
\|\beta\|_1 \le t
$$

Diamond geometry → solutions at corners:

$$
\beta_j = 0
$$

✔ Natural feature selection

---

## 🌐 High‑Dimensional Effect

Higher dimension → more corners → more zeros → **sparser model**

---

## ⚔️ Ridge vs Lasso

| Property | Ridge 🔵 | Lasso 🔶 |
|----------|----------|----------|
| Penalty | $$L_2$$ | $$L_1$$ |
| Coefficients | Close to 0 | Can be exactly 0 |
| Feature selection | ❌ | ✔ |
| Stability | High | Medium |
| Sparse model | ❌ | ✔ |

---

## 🤔 Which is Better?

No universal winner.

- Sparse truth → Lasso better  
- Dense truth → Ridge better  

Best practice:

$$
\text{Choose using Cross Validation}
$$

---

## 🎛️ Hyperparameter Selection

1. Choose grid of $$\lambda$$  
2. Compute CV error  
3. Select minimum  
4. Refit with all data  

---

## 🧩 Practical Insights

- Small data → CV may deceive ⚠️  
- Always scale predictors ✔  
- Ridge → Smooth shrinkage  
- Lasso → Sparse solution  
- Capacity tuning critical  
- Estimate overfitting onset  

---

## 🌟 Final Insight

A good model is **not the one with lowest training loss**,  
but the one achieving **best generalization with controlled complexity**.


-----------
-------------
-----------
-
-------------

[ML/DL] Lecture 8. Decision Trees


# 🌳 Decision Trees — Summary

Decision trees are intuitive, interpretable models that **split the feature space into simple regions**.

---

# 1️⃣ Tree Terminology Review

## 🌲 General Tree Structure

A general tree **T** is partitioned into:

- **Root node** r  
- A set of **subtrees** attached to the root  

Example structure:

        A (root)
       / \
      B   C
     /|\
    D E F

---

## 📘 Basic Terminology

| Term | Meaning |
|------|---------|
| 🔵 Node (Vertex) | A point in the tree |
| ➖ Edge | Connection between nodes |
| 👨 Parent | Node with children |
| 👶 Child | Node below parent |
| 👥 Siblings | Nodes with same parent |
| 🌳 Root | Top node |
| 🍃 Leaf | Node with no children |
| ⬆ Ancestor | Any node above |
| ⬇ Descendant | Any node below |
| 🌿 Subtree | Tree inside tree |

---

# 2️⃣ Tree-Based Learning

## 🎯 Core Idea

Tree-based learning **segments the predictor space into simple regions**.

✔ Recursive partitioning  
✔ Forms decision rules  
✔ Works for both:

- 📈 Regression  
- 🔎 Classification  

---

## ⚙️ How It Works

1. Choose best feature & split value  
2. Divide feature space  
3. Repeat recursively  
4. Leaf → prediction  

---

## 📊 Interpretation

- Space split into **rectangular regions**
- Prediction is **constant inside each region**
- Model behaves like a **step function**

---

# 3️⃣ Introduction to Decision Trees

## ✅ Pros

- 🧠 Easy to interpret
- 🔍 Transparent decision rules
- ⚖️ No feature scaling required
- 🔄 Handles nonlinear relationships
- 📊 Works with mixed data types

---

## ❌ Cons

- 📉 Often lower prediction accuracy
- ⚠️ High variance (unstable)
- 🌪 Prone to overfitting

---

# 4️⃣ Ensemble Methods 🌲🌲🌲

Multiple trees → Stronger model

| Method | Idea |
|--------|------|
| 🎲 Bagging | Reduce variance |
| 🌲 Random Forest | Randomized bagging |
| 🚀 Boosting | Reduce bias sequentially |

➡ Dramatically improves accuracy

---

# 5️⃣ Model Flexibility vs Interpretability

| Model | Flexibility | Interpretability |
|-------|------------|------------------|
| Linear Models | Low | High |
| Decision Trees | Medium | Medium |
| Random Forest / Boosting | High | Low |
| Deep Learning | Very High | Very Low |

---

# 🧠 Key Insights

- Decision trees **partition feature space**
- Produce **interpretable rules**
- Single tree → simple but weak
- Many trees → powerful model

---

# 📌 One-Line Summary

Decision Trees split the feature space into simple regions to form interpretable decision rules 🌳

---



# 🌳 Decision Trees — Overfitting and Pruning (Complete Reconstruction)

This document reconstructs the provided slides **with minimal summarization**, preserving equations, algorithm flow, and visual interpretation.

---

# 1️⃣ Overfitting in Decision Trees

![Overfitting Illustration](k0.jpg)

A decision tree **can easily overfit**.

Example shown:

- A large region is predicted based on **only one training point**
- This leads to **very irregular decision regions**
- The model memorizes noise instead of learning structure

Result:

- Extremely low training error
- Very poor generalization
- High variance model

---

# 2️⃣ Model Selection Perspective

![Model Selection](k1.jpg)

To reduce overfitting, we move toward **simpler models**.

Key ideas:

- Fewer splits → smoother boundary
- Simpler tree → higher bias, lower variance
- Complex tree → low bias, high variance

Trade-off:

| Property | Small Tree | Large Tree |
|----------|------------|-----------|
| Complexity | Low | High |
| Boundary | Smooth | Irregular |
| Bias | High | Low |
| Variance | Low | High |
| Risk | Underfitting | Overfitting |

---

# 3️⃣ Handling Overfitting

![Handling Overfitting](k2.jpg)

Strategy 1: Build a **smaller tree**

- Fewer regions $$R_1,\dots,R_J$$
- Reduces variance
- Slight increase in bias
- Often improves interpretability

However:

A weak early split might later lead to a strong split.  
This is due to the **greedy nature** of tree building.

Thus, stopping early may be **short-sighted**.

---

# 4️⃣ Grow Large Tree then Prune

![Cost Complexity Pruning](k3.jpg)

Alternative strategy:

1. Grow a very large tree $$T_0$$
2. Prune it back to smaller trees

---

## Weakest Link (Cost-Complexity) Pruning

We define a sequence of trees indexed by $$\alpha$$:

$$
\sum_{m=1}^{|T|} \sum_{x_i \in R_m} (y_i - \hat{y}_{R_m})^2 + \alpha |T|
$$

Where:

- number of leaf nodes $$|T|$$
- controls complexity penalty $$\alpha$$
- Larger  → smaller tree $$\alpha$$
- Smaller  → larger tree $$\alpha$$
 
Goal:

Find subtree minimizing penalized loss.

Pruning works by:

- Merging leaf nodes
- Removing weakest split

---

# 5️⃣ Tree Building with Pruning

![Tree Building Process](k4.jpg)

Algorithm:

1. Grow large tree using recursive binary splitting
2. Apply weakest link pruning to obtain sequence of subtrees
3. Use K-fold cross-validation to select $$\alpha$$
4. Choose subtree corresponding to best $$\alpha$$

---

# 6️⃣ Example — Unpruned Tree

![Unpruned Tree](k5.jpg)

A very large regression tree $$T_0$$ is first grown using the training data.

Characteristics:

- Many splits
- Low training error
- High risk of overfitting

---

# 7️⃣ Example — Selecting Tree Size

![Validation Curve](k6.jpg)

We evaluate tree size using:

- Training error
- Cross-validation error
- Test error

Observation:

Best validation performance occurs at **3 leaf nodes**.

Thus, we prune the large tree down to optimal size.

---

# 📐 Mathematical Summary

## Tree Objective

$$
\min \sum_{j=1}^{J} \sum_{i \in R_j} (y_i - \hat{y}_{R_j})^2
$$

## Cost-Complexity Objective

$$
\min \sum_{m=1}^{|T|} \sum_{x_i \in R_m} (y_i - \hat{y}_{R_m})^2 + \alpha |T|
$$

---

# 🧠 Key Insights

- Decision trees easily overfit
- Small tree → high bias, low variance
- Large tree → low bias, high variance
- Best approach = grow large tree then prune
- Cross-validation selects optimal tree size

---

# 🌟 Final Insight

> Optimal decision trees balance fit and complexity using **cost-complexity pruning and cross-validation**.

---


# 🌳 Classification Trees — Full Notes

---

## 📘 1. What is a Classification Tree?

A **classification tree** is very similar to a regression tree, except:

- Regression → predicts **continuous value**
- Classification → predicts **discrete (categorical) class** 🎯

For a classification tree, prediction is:

$$
\hat{y} = \text{most frequent class in region } R_j
$$

---

## ⚙️ 2. Decision Tree Algorithm (Classification)

### Step 1 — Partition the Feature Space 🔪

We divide the predictor space into **J disjoint regions**:

$$
R_1, R_2, ..., R_J
$$

Using **recursive binary splitting (top‑down greedy)**.

For feature $x_j$ and split point $s$:

$$
R_1(j,s) = \{x \mid x_j < s\}
$$
$$
R_2(j,s) = \{x \mid x_j \ge s\}
$$

We choose $(j,s)$ minimizing classification error via impurity:

$$
\frac{n_1}{n} \, \text{Impurity}(R_1) + \frac{n_2}{n} \, \text{Impurity}(R_2)
$$

Goal 👉 make regions **as pure as possible** 🧼

---

## 📊 3. Impurity Measures

### Entropy (Cross‑Entropy) 🔥

$$
\text{Impurity}(R_j) = -\sum_{k=1}^{K} p_{j,k} \log p_{j,k}
$$

Where:

- $p_{j,k}$ = proportion of class $k$ in region $R_j$

#### Properties

- Pure region → Entropy = **0**
- Uniform distribution → Entropy = **max = log(K)**

Examples:

$$
[1,0,0,0] \Rightarrow 0
$$

$$
[0.5,0.5] \Rightarrow \log 2
$$

$$
[0.2,0.2,0.2,0.2,0.2] \Rightarrow \log 5
$$

---

## 🎯 4. Prediction in Each Region

### Majority Vote

$$
\hat{y}_j = \arg\max_k \; p_{j,k}
$$

Equivalent form:

$$
\hat{y}_j = \arg\max_k \frac{1}{n_j} \sum_{x_i \in R_j} I(y_i = k)
$$

Where:

- $n_j$ = number of samples in region
- $I(\cdot)$ = indicator function

---

## 🧠 5. Decision Boundary Behavior

| Model | Boundary Shape |
|------|---------------|
| Linear Model | Straight line 📏 |
| Decision Tree | Axis‑aligned blocks 🧱 |

Trees create **step‑wise rectangular decision boundaries**.

---

## 👍 6. Pros of Decision Trees

- Very **interpretable** 👀  
- Graphical structure easy to understand  
- Mimics **human decision making** 🧠  
- Handles **categorical features naturally**  

---

## 👎 7. Cons of Decision Trees

- Often **lower predictive accuracy** than advanced models  
- High **variance / unstable** ⚠️  
- Small data change → very different tree  

---

## 🧩 8. Summary

- Classification tree partitions space into regions
- Uses impurity (entropy / Gini) to choose splits
- Predicts by **majority vote**
- Produces **rectangular decision boundaries**
- Easy to interpret but can overfit

---

## 🚀 (Optional Next Topics)

- Gini vs Entropy comparison
- Information Gain derivation
- CART algorithm
- Pruning & Regularization
- Random Forest / Boosting

----
----
----
----
[ML/DL] Lecture 9. Ensemble Models and Boosting



# 🎲 Bootstrapping — Complete Notes

---

## 📌 1. What is Bootstrapping?

Bootstrapping is a **resampling technique** used when we **cannot sample additional data from the true distribution**.

- The true distribution is usually **unknown**
- Our goal is often to **estimate properties of that distribution**

Instead of collecting new independent data, we:

👉 Repeatedly sample **from the original dataset with replacement**

Each bootstrap dataset:

- Same size as original dataset
- Some samples appear **multiple times**
- Some samples **may not appear at all**

---

## 🔁 2. Bootstrap Procedure

Let original dataset be:

$$
Z = \{(x_1,y_1), (x_2,y_2), ..., (x_n,y_n)\}
$$

We generate **B bootstrap datasets**:

$$
Z^{*1}, Z^{*2}, ..., Z^{*B}
$$

Each created by **sampling with replacement** from $Z$.  

For each dataset, compute estimator:

$$
\hat{\alpha}^{*b}, \quad b = 1,2,...,B
$$

### 📏 Bootstrap Standard Error

$$
SE_B(\hat{\alpha}) =
\sqrt{
\frac{1}{B-1}
\sum_{b=1}^{B}
\left(\hat{\alpha}^{*b} - \bar{\alpha}^*\right)^2
}
$$

where

$$
\bar{\alpha}^* = \frac{1}{B} \sum_{b=1}^{B} \hat{\alpha}^{*b}
$$

This estimates the **standard error of the estimator**.

---

## ⚠️ 3. Limitations of Bootstrapping

Bootstrapping assumes:

### i.i.d assumption

Samples must be:

$$
\text{Independent and Identically Distributed (i.i.d)}
$$

If NOT true (e.g. **time series data**):

- Sampling individual observations breaks temporal structure
- Instead use **block bootstrap**

### Block Bootstrap

- Create blocks of consecutive observations
- Sample blocks with replacement
- Reconstruct dataset from sampled blocks

Used in:

- Time series
- Session‑based recommendation systems

---

## 🔍 4. Bootstrapping vs Cross‑Validation

### Can bootstrap estimate prediction error?

**Short answer: ❌ No**

### Reason

#### Cross‑Validation

- No overlap between training and validation sets
- Independent validation → unbiased estimate

#### Bootstrapping

- Samples drawn **with replacement**
- Bootstrap datasets **overlap heavily**
- Not independent → biased estimate

---

## 📊 5. Why does each bootstrap contain ~2/3 of data?

Probability a sample is **NOT selected** in one draw:

$$
1 - \frac{1}{n}
$$

Probability it is never selected in $n$ draws:

$$
\left(1 - \frac{1}{n}\right)^n
$$

Taking limit:

$$
\lim_{n \to \infty}
\left(1 - \frac{1}{n}\right)^n
= e^{-1} \approx 0.368
$$

So:

- About **36.8% NOT included**
- About **63.2% included**

👉 Each bootstrap sample contains **≈ 2/3 of original data**

---

## 🚨 6. Bias of Bootstrap Error

Because bootstrap datasets overlap:

- Bootstrap tends to **underestimate true prediction error**
- Validation sets are not fully independent

---

## 🧠 7. Summary

- Bootstrapping = sampling **with replacement**
- Used to estimate **variance, SE, confidence intervals**
- Requires **i.i.d assumption**
- Each bootstrap contains ~63% unique samples
- Cannot reliably estimate prediction error
- Use **cross‑validation** instead for model evaluation

---

## 🚀 Next Topics

- Bagging (Bootstrap Aggregating)
- Random Forest
- Out‑of‑Bag Error
- Bias‑Variance Decomposition



# 🌲 Bagging & Ensemble Methods — Complete Mathematical Notes

---

# 🎯 1. What is Bagging (Bootstrap Aggregation)?

Bagging creates **B bootstrap datasets** from the original training data and trains **B separate models**.

Each model:

$$
\hat{f}^{(b)}(x), \quad b = 1,2,...,B
$$

Final prediction (Regression):

$$
\hat{f}_{bag}(x) = \frac{1}{B}\sum_{b=1}^{B}\hat{f}^{(b)}(x)
$$

Final prediction (Classification):

$$
\hat{y} = \text{majority vote of } \hat{y}^{(1)},...,\hat{y}^{(B)}
$$

📌 Bagging averages **predictions**, not parameters → works for ANY model.

---

# 🧠 2. Why Bagging Works (Variance Reduction)

Bagging is similar to **wisdom of crowd**.

If we average independent estimators:

$$
Var(\bar{Z}) = \frac{\sigma^2}{n}
$$

Thus averaging reduces variance.

Total error:

$$
MSE(\hat{\theta}) = Var(\hat{\theta}) + Bias(\hat{\theta})^2
$$

👉 Bagging **reduces variance without increasing bias**.

Cost: Need to train **B models**.

---

# 📊 3. Mathematical Analysis of Bagging

Let:

$$
y_b(x) = h(x) + \epsilon_b(x)
$$

Where:

- $h(x)$ = true function  
- $\epsilon_b(x)$ = error of model $b$  

### Error of single model

$$
E_{single} = E_x[(y_b(x) - h(x))^2] = E_x[\epsilon_b(x)^2]
$$

### Error of combined model

$$
E_{comb} = E_x\left[\left(\frac{1}{B}\sum_{b=1}^{B} y_b(x) - h(x)\right)^2\right]
$$

---

## 📌 Theorem 1 — Ensemble never worse

The expected error of ensemble ≤ single model.

Using **Jensen’s inequality**:

$$
E_{single} \ge E_{comb}
$$

---

## 📌 Theorem 2 — Error can shrink by 1/B

Expand:

$$
E_{comb} = E_x\left[\left(\frac{1}{B}\sum_{b=1}^{B}\epsilon_b(x)\right)^2\right]
$$

$$
= E_x\left[\frac{1}{B^2}\sum_{b=1}^{B}\epsilon_b(x)^2 + \frac{2}{B^2}\sum_{j\ne k}\epsilon_j(x)\epsilon_k(x)\right]
$$

If models are **independent**:

$$
E[\epsilon_j(x)\epsilon_k(x)] = 0
$$

Then:

$$
E_{comb} = \frac{1}{B}E_{single}
$$

📌 If models identical → no gain  
📌 More independence → better ensemble

---

# 🌐 4. General Ensemble Learning

Ensemble = Combine multiple models to improve prediction.

## Types

### 1. Bagging
- Parallel models
- Reduce variance
- Example: Random Forest 🌲

### 2. Boosting
- Sequential models
- Reduce bias + variance
- Example: AdaBoost / Gradient Boosting ⚡

### 3. Stacking
- Combine different model types using meta‑model

---

# 🔀 5. Can Different Models Be Ensembled?

## YES — Heterogeneous Ensemble

You can combine:

- Linear model + Tree + Neural Net
- SVM + Random Forest + Logistic
- Any models with prediction output

Common combination methods:

### Averaging (Regression)

$$
\hat{y} = \sum_{m=1}^{M} w_m \hat{y}_m
$$

### Majority Vote (Classification)

$$
\hat{y} = \arg\max_k \sum_{m=1}^{M} I(\hat{y}_m = k)
$$

### Stacking (Meta‑Learning)

Train second model:

$$
\hat{y} = g(\hat{y}_1, \hat{y}_2, ..., \hat{y}_M)
$$

---

# 📌 6. When Ensemble Works Best

Ensemble improves when:

1. Models are **accurate**
2. Models are **diverse (uncorrelated errors)**
3. Individual models not identical

Key idea:

$$
Var(\text{ensemble}) = \rho\sigma^2 + \frac{1-\rho}{B}\sigma^2
$$

Where:

- $\rho$ = correlation between models
- Lower $\rho$ → stronger ensemble

---

# ⚠️ 7. Limitations

- High computation cost
- Harder to interpret
- Little gain if models highly correlated

---

# 🚀 8. Summary

- Bagging reduces **variance**
- Ensemble error ≤ single model
- Independence between models is critical
- Can combine **same or different model types**
- Foundation of Random Forest, Boosting, Stacking

---

# Next Topics

- 🌲 Random Forest (feature randomness + bagging)
- ⚡ Boosting math derivation
- 📉 Bias‑Variance decomposition in ensemble
- 🎯 Out‑of‑Bag error



# 🌲 Bagged Trees & 🌳 Random Forests — Complete Notes

---

# 🎯 1. Bagged Trees (Bootstrap Aggregation)

Bagging = Train many decision trees on **bootstrap samples** and combine predictions.

## Procedure

1. Sample many bootstrap datasets from original data
2. Train a decision tree on each dataset
3. Combine predictions

Regression:

$$
\hat{f}_{bag}(x) = \frac{1}{B}\sum_{b=1}^{B}\hat{f}^{(b)}(x)
$$

Classification:

$$
\hat{y} = \text{majority vote}(\hat{y}^{(1)},...,\hat{y}^{(B)})
$$

---

## Why Bagging Works — Variance Reduction 📉

Averaging independent estimators reduces variance:

$$
Var(\bar{Z}) = \frac{\sigma^2}{B}
$$

Total error:

$$
MSE = Bias^2 + Variance
$$

👉 Bagging mainly **reduces variance** (trees are high‑variance models).

---

## Numerical Example 🎲

Suppose a single decision tree has:

- Bias² = 0.04
- Variance = 0.25

Then:

$$
MSE_{single} = 0.29
$$

If we bag **B = 25 independent trees**:

$$
Variance_{bag} = \frac{0.25}{25} = 0.01
$$

$$
MSE_{bag} = 0.04 + 0.01 = 0.05
$$

➡ Huge improvement from **0.29 → 0.05**

---

# 🌳 2. Random Forest

Random Forest = Bagging + **Feature Randomness**

Key idea: **Decorrelate trees** to improve ensemble.

## How it Works

- Still use bootstrap sampling
- But when splitting a node:
  - Instead of using all $p$ features
  - Randomly choose **m features**
  - Split using only those m

Typical choice:

$$
m = \sqrt{p} \quad (\text{classification})
$$

$$
m = \frac{p}{3} \quad (\text{regression})
$$

---

## Why Random Forest Beats Bagging 🧠

If trees are highly correlated → averaging does not reduce variance much.

Variance of ensemble:

$$
Var_{RF} = \rho\sigma^2 + \frac{1-\rho}{B}\sigma^2
$$

Where:

- $\rho$ = correlation between trees
- Smaller $\rho$ → stronger variance reduction

Random feature selection ↓ correlation → ↓ variance → ↓ error.

---

# 📊 3. Random Forest Behavior

As number of trees increases:

- Training error ↓
- Test error stabilizes
- Overfitting rarely happens

Because averaging stabilizes prediction.

---

# 🎲 4. Example — Effect of m (Feature Subset)

Suppose:

- p = 100 features

Compare:

| m | Behavior |
|---|----------|
| m = 100 (Bagging) | Trees very similar → high correlation |
| m = 50 | Slight decorrelation |
| m = √100 = 10 | Strong decorrelation → best performance |
| m = 1 | Too random → weak trees |

Typical best choice:

$$
m \approx \sqrt{p}
$$

---

# 📌 5. Random Forest Advantages 👍

- Strong prediction accuracy
- Handles nonlinear relationships
- Works with high‑dimensional data
- Resistant to overfitting
- Implicit feature selection
- Robust to noise

---

# ⚠️ 6. Limitations 👎

- Less interpretable than single tree
- High computation for many trees
- Large memory usage
- Can struggle with very sparse signals

---

# 📈 7. Out‑of‑Bag (OOB) Error

Each tree is trained on ~63% of data (bootstrap).

Remaining ~37% = **Out‑of‑Bag samples**

Use OOB samples as validation → unbiased error estimate.

No need for cross‑validation 👍

---

# 🔍 8. Random Forest vs Bagging

| Method | Key Idea |
|--------|----------|
| Bagging | Bootstrap + averaging |
| Random Forest | Bagging + feature randomness |
| Goal | Reduce variance |
| Difference | RF decorrelates trees |

---

# 🧠 9. When Random Forest Works Best

- High variance models (decision trees)
- Nonlinear data
- Many features
- Complex decision boundary

---

# 🚀 10. Summary

- Bagging reduces variance via averaging
- Random Forest reduces variance **even more** by decorrelating trees
- More trees → stable performance
- Feature randomness is key
- OOB error gives built‑in validation

---

# Next Topics

- 🌲 Feature Importance in Random Forest
- ⚡ Gradient Boosting vs Random Forest
- 📉 Bias‑Variance comparison
- 🎯 Extra Trees (Extremely Randomized Trees)


----
----
----
----
---
---
---

svm


# 🧠✨ Support Vector Machine (SVM) — Complete Mathematical Derivation

> 🎯 Goal: Find the **maximum-margin hyperplane** that separates two classes.

---

## 📦 1. Problem Setup

Training data:

$$
\{(x_i, y_i)\}_{i=1}^n,\quad x_i \in \mathbb{R}^p,\quad y_i \in \{-1, +1\}
$$

Hyperplane:

$$
\beta^T x + \beta_0 = 0
$$

Classifier:

$$
\hat y = \mathrm{sign}(\beta^T x + \beta_0)
$$

---

## 📐 2. Distance from a Point to a Hyperplane (Full Derivation)

We compute the signed distance from a point $x$ to the hyperplane.

### 🔹 Step 1 — Closest point on the hyperplane

The closest point must lie along the normal vector $\beta$:

$$
x_\perp = x - t\beta
$$

for some scalar $t$.

### 🔹 Step 2 — Enforce hyperplane constraint

$$
\beta^T x_\perp + \beta_0 = 0
$$

Substitute:

$$
\beta^T (x - t\beta) + \beta_0 = 0
$$

$$
\beta^T x - t\beta^T\beta + \beta_0 = 0
$$

Solve for $t$:

$$
t = \frac{\beta^T x + \beta_0}{\|\beta\|^2}
$$

### 🔹 Step 3 — Distance

$$
\|x - x_\perp\| = \|t\beta\| = |t| \|\beta\|
$$

$$
\|x - x_\perp\| = \frac{|\beta^T x + \beta_0|}{\|\beta\|}
$$

Signed distance:

$$
\boxed{
\mathrm{dist}(x,H) = \frac{\beta^T x + \beta_0}{\|\beta\|}
}
$$

---

## 🚀 3. Functional Margin and Geometric Margin

Functional margin:

$$
y_i(\beta^T x_i + \beta_0)
$$

Geometric margin:

$$
\gamma_i = \frac{y_i(\beta^T x_i + \beta_0)}{\|\beta\|}
$$

Dataset margin:

$$
\gamma = \min_i \gamma_i
= \min_i \frac{y_i(\beta^T x_i + \beta_0)}{\|\beta\|}
$$

🎯 Goal: **maximize margin**.

---

## 🔁 4. Max-Margin Optimization (Full Transformation)

We want:

$$
\max_{\beta,\beta_0} \gamma
$$

### 🔹 Scaling invariance

$$
\gamma(\beta,\beta_0) = \gamma(c\beta, c\beta_0)
$$

So enforce normalization:

$$
y_i(\beta^T x_i + \beta_0) \ge 1
$$

Then:

$$
\gamma = \frac{1}{\|\beta\|}
$$

Thus:

$$
\max \gamma \Longleftrightarrow \min \|\beta\|
$$

Smooth objective:

$$
\boxed{
\min_{\beta,\beta_0} \frac12 \|\beta\|^2
\quad \text{s.t.} \quad
y_i(\beta^T x_i + \beta_0) \ge 1
}
$$

👉 This is the **Hard-Margin SVM Primal Problem**.

---

## 🧮 5. Lagrangian Formulation

Introduce multipliers $\alpha_i \ge 0$:

$$
\mathcal{L}(\beta,\beta_0,\alpha)
=
\frac12 \|\beta\|^2
+
\sum_{i=1}^n \alpha_i
\left(1 - y_i(\beta^T x_i + \beta_0)\right)
$$

---

## 📉 6. Stationarity Conditions

### 🔹 Derivative w.r.t. $\beta$

$$
\frac{\partial \mathcal{L}}{\partial \beta}
= \beta - \sum_{i=1}^n \alpha_i y_i x_i = 0
$$

$$
\boxed{
\beta = \sum_{i=1}^n \alpha_i y_i x_i
}
$$

### 🔹 Derivative w.r.t. $\beta_0$

$$
\frac{\partial \mathcal{L}}{\partial \beta_0}
= -\sum_{i=1}^n \alpha_i y_i = 0
$$

$$
\boxed{
\sum_{i=1}^n \alpha_i y_i = 0
}
$$

---

## 🔄 7. Dual Derivation (No Steps Skipped)

Substitute

$$
\beta = \sum_{i=1}^n \alpha_i y_i x_i
$$

into the Lagrangian.

### 🔹 Expand $\|\beta\|^2$

$$
\|\beta\|^2
=
\left(\sum_{i=1}^n \alpha_i y_i x_i\right)^T
\left(\sum_{j=1}^n \alpha_j y_j x_j\right)
$$

$$
=
\sum_{i=1}^n \sum_{j=1}^n
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

Thus:

$$
\frac12 \|\beta\|^2
=
\frac12 \sum_{i,j}
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

### 🔹 Expand middle term

$$
\sum_{i=1}^n \alpha_i y_i \beta^T x_i
=
\sum_{i=1}^n \sum_{j=1}^n
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

### 🔹 Combine

$$
\mathcal{L}
=
\sum_{i=1}^n \alpha_i
-
\frac12 \sum_{i,j}
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

---

## 🎯 8. Dual Optimization

$$
\boxed{
\max_{\alpha}
\sum_{i=1}^n \alpha_i
-
\frac12 \sum_{i,j}
\alpha_i \alpha_j y_i y_j x_i^T x_j
}
$$

subject to:

$$
\alpha_i \ge 0,
\quad
\sum_{i=1}^n \alpha_i y_i = 0
$$

---

## 🧠 9. Final Decision Function

Optimal weight:

$$
\beta = \sum_{i=1}^n \alpha_i y_i x_i
$$

Decision function:

$$
f(x) =
\sum_{i=1}^n \alpha_i y_i x_i^T x + \beta_0
$$

Prediction:

$$
\boxed{\hat y = \mathrm{sign}(f(x))}
$$

Only $\alpha_i > 0$ matter → **Support Vectors**.

---

## 📌 10. Complementary Slackness

$$
\alpha_i \left(1 - y_i(\beta^T x_i + \beta_0)\right) = 0
$$

- If $\alpha_i > 0$ ⇒ point lies on margin  
- If point outside margin ⇒ $\alpha_i = 0$

Only support vectors determine the boundary.

---

## 🔗 11. Kernel Form

Replace inner product:

$$
x_i^T x \rightarrow K(x_i, x)
$$

Decision function:

$$
f(x)
=
\sum_{i=1}^n \alpha_i y_i K(x_i, x) + \beta_0
$$

---

# ✨ Key Insights

- 🧠 SVM maximizes geometric margin  
- 📐 Primal minimizes weight norm  
- 🔎 Dual depends only on inner products  
- 🧷 Solution is sparse (support vectors)  
- 🔥 Kernel trick enables nonlinear boundaries


----
---
---
----
---
---
layout: post
title: "Unsupervised Learning"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Machince Learning, Unsupervised]
tags: [Machince Learning, Unsuperviseding, Overview]
pin: false
math: true
mermaid: true
---

# 🧠 Unsupervised Learning & Clustering

> Lecture-style structured notes with intuition, examples, and math

---

## 📌 Why Unsupervised Learning?

### 🎯 Goal
Unsupervised learning aims to **discover interesting structure** in data **without labels**.

- 🔍 Discover **subgroups / patterns** among observations or variables
- 📊 Find informative ways to **visualize high-dimensional data**

### 💡 Why is it important?
- Unlabeled data is **easier and cheaper** to obtain
- Labeling requires **human labor & expertise**

### ⚠️ Key Characteristic
- No single objective like prediction accuracy
- Results are often **subjective**

---

## 🧩 Clustering Problem

### 📖 Definition
Finding **natural groupings** among objects.

### ✅ Objective
- High **intra-cluster similarity**
- Low **inter-cluster similarity**

---

## 🧪 Clustering Examples

### 🧬 Gene Clustering
- Microarrays measure gene activity across conditions
- Similar expression patterns → clustered genes
- Helps infer **functions of unknown genes**

---

### 👤 User Clustering (Recommendation Systems)
- Core idea of **collaborative filtering**
- Users with similar tastes are grouped

> “Users like you also liked …”

---

### 🖼️ Image Compression

Each pixel is a vector:
$$
\mathbf{x}_i = [R_i, G_i, B_i]^T
$$

Cluster centers:
$$
\{\mu_1, \mu_2, \dots, \mu_K\}
$$

Assignment:
$$
\arg\min_k \| \mathbf{x}_i - \mu_k \|_2
$$

Fewer colors → smaller storage size

---

## 🆚 Classification vs Clustering

| Classification | Clustering |
|---------------|------------|
| Uses labels | No labels |
| Predict class | Discover structure |
| Supervised | Unsupervised |

---

## 🎭 Clustering is Subjective

There is **no single correct clustering**.

Possible groupings:
- Family-based
- Gender-based
- Occupation-based

Depends on **similarity definition**.

---

## 📏 Similarity / Distance Metrics

### L1 Distance
$$
L_1(A,B) = \sum_{i,j} |A_{ij} - B_{ij}|
$$

### L2 Distance
$$
L_2(A,B) = \sqrt{ \sum_{i,j} (A_{ij} - B_{ij})^2 }
$$

### Distance Matrix
$$
D =
\begin{bmatrix}
0 & d_{12} & d_{13} \\
d_{21} & 0 & d_{23} \\
d_{31} & d_{32} & 0
\end{bmatrix}
$$

---

## 🧱 Two Types of Clustering

### 🌲 Hierarchical Clustering
- Bottom-up (agglomerative)
- Produces a dendrogram

### 📦 Partitional Clustering
- Top-down
- Requires number of clusters K
- Example: K-means

---

## 🧠 Summary

- Unsupervised learning finds structure without labels
- Clustering is the most common technique
- Results depend on:
  - Distance metric
  - Number of clusters
  - Interpretation goal


---
layout: post
title: "K-Means"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Machince Learning, Unsupervised]
tags: [Machince Learning, Unsuperviseding, Clustering, K-Means]
pin: false
math: true
mermaid: true
---

# 🌳 Hierarchical Clustering & 📦 K-Means

> Unsupervised Learning – Clustering Algorithms (Lecture Notes)

---

## 🌳 Hierarchical Clustering

### 🔁 Agglomerative Clustering (Bottom-Up)

- **Level 0**: Start with singleton clusters  
  (each data point is its own cluster)

- For level ℓ = 1 to L:
  - Set a distance threshold $$\tau_\ell$$ (monotonically increasing)
  - Merge clusters whose distance is within $$\tau_\ell$$

➡️ This process builds a **hierarchical tree (dendrogram)**

---

## 📏 Distance Between Clusters

At level ≥ 1, we need **distance between clusters**, not points.

### Common Linkage Methods

Let clusters be $$C_p, C_q$$

#### 1️⃣ Single-Link (Closest Pair)
$$
d_{\text{single}}(C_p, C_q) = \min_{x \in C_p, y \in C_q} \|x - y\|
$$

- Can cause **chaining problem**
- Distant points may be grouped together indirectly

---

#### 2️⃣ Complete-Link (Farthest Pair)
$$
d_{\text{complete}}(C_p, C_q) = \max_{x \in C_p, y \in C_q} \|x - y\|
$$

- Produces compact clusters
- Sensitive to outliers

---

#### 3️⃣ Average-Link
$$
d_{\text{avg}}(C_p, C_q) =
\frac{1}{|C_p||C_q|}
\sum_{x \in C_p}\sum_{y \in C_q} \|x-y\|
$$

---

#### 4️⃣ Centroid Distance
$$
d(C_p, C_q) = \|\mu_p - \mu_q\|
$$

where
$$
\mu_p = \frac{1}{|C_p|}\sum_{x \in C_p} x
$$

---

### ⚠️ Important Note
Different linkage choices → **different clustering behavior**  
Clustering results are **not unique**.

---

## 📦 K-Means Clustering

### 🔄 Overview

- Iterative
- Partitional clustering algorithm
- Requires number of clusters **K**

---

## 🎯 Objective Function

Minimize within-cluster sum of squares:

$$
J = \sum_{i=1}^{N}\sum_{k=1}^{K} r_{ik} \|x_i - \mu_k\|^2
$$

Where:

- $x_i \in \mathbb{R}^d$ : data point
- $\mu_k \in \mathbb{R}^d$ : centroid of cluster k
- $r_{ik} \in \{0,1\}$ :
  $$
  r_{ik} =
  \begin{cases}
  1 & \text{if } x_i \text{ assigned to cluster } k \\
  0 & \text{otherwise}
  \end{cases}
  $$

---

## ⚙️ K-Means Algorithm

### Step 1️⃣ Initialization
Pick K random points as initial centroids:
$$
\mu_1, \mu_2, \dots, \mu_K
$$

---

### Step 2️⃣ Assignment Step (a)

For each data point:
$$
r_{ik} =
\begin{cases}
1 & k = \arg\min_j \|x_i - \mu_j\|^2 \\
0 & \text{otherwise}
\end{cases}
$$

---

### Step 3️⃣ Update Step (b)

Update centroids:
$$
\mu_k =
\frac{\sum_{i=1}^{N} r_{ik} x_i}
{\sum_{i=1}^{N} r_{ik}}
$$

---

### Step 4️⃣ Repeat
Repeat (a) and (b) until assignments do not change.

---

## ✅ Does K-Means Always Terminate?

✔️ **Yes**

Reasons:

- Assignment step does not increase $$J$$
- Update step minimizes $$J$$ w.r.t. $$\mu_k$$
- Finite number of possible assignments

➡️ Converges to a **local minimum**

---

## ❌ Does It Always Converge to Same Result?

❌ **No**

- Depends on random initialization
- Different runs → different local optima

➡️ Use **multiple restarts** or **K-means++**

---

## ⏱️ Time Complexity

Let:
- N = number of points
- K = number of clusters
- T = iterations

### Assignment Step:
$$
O(KN)
$$

### Update Step:
$$
O(N)
$$

### Total:
$$
O(TKN)
$$

Worst case is NP-hard, but in practice T is small.

---

## ❓ How to Choose K

### Problem
Objective $$J$$ always decreases as K increases.

---

### 🦴 Elbow Method

- Plot $$J$$ vs K
- Choose K where decrease slows (elbow point)

⚠️ Sometimes elbow is unclear.

---

## 🧠 Summary

- Hierarchical clustering builds a dendrogram
- K-means optimizes a clear objective
- Both depend on distance definitions
- No single “correct” clustering

✨ Clustering is exploratory, not definitive
