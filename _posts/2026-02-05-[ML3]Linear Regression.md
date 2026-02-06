---
layout: post
title: "Supervised Learning Process"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Linear Regression]
tags: [Machince Learning, ML, Supervised, Linear Regression, RSS, Correlation]
pin: false
math: true
mermaid: true
---

# 📊 Linear Regression, Bias–Variance, and Model Interpretation

---

# 🎯 Bias–Variance Decomposition

$$
\underbrace{\mathbb{E}\big[\, y_0 - \hat{f}(x_0) \,\big]^2}_{\text{Expected squared prediction error}}
=
\underbrace{\mathrm{Var}\big(\hat{f}(x_0)\big)}_{\text{Variance}}
+
\underbrace{\big[\mathrm{Bias}\big(\hat{f}(x_0)\big)\big]^2}_{\text{Squared Bias}}
+
\underbrace{\mathrm{Var}(\varepsilon)}_{\text{Irreducible Noise}}
$$

$$
\mathrm{Bias}\big(\hat{f}(x_0)\big)
= \mathbb{E}\big[\hat{f}(x_0)\big] - f(x_0)
$$

---

# 📈 Linear Regression Model

Assume linear dependence:

$$
y = \beta_0 + \beta_1 x + \varepsilon
$$

- $\beta_0, \beta_1$ : intercept and slope  
- $\varepsilon$ : random noise  

---

# 📉 Residual Sum of Squares (RSS)

$$
\mathrm{RSS} = \sum_{i=1}^{n}(y_i-\hat{y}_i)^2
$$

$$
\mathrm{RSS}=\sum_{i=1}^{n}\big(y_i-(\beta_0+\beta_1 x_i)\big)^2
$$

---

# 🧮 Estimation of Coefficients

## Derivative w.r.t $\beta_0$

$$
\frac{\partial}{\partial \beta_0}
\sum_{i=1}^{n}\big(y_i-(\beta_0+\beta_1 x_i)\big)^2
= -2\sum_{i=1}^{n}\big(y_i-(\beta_0+\beta_1 x_i)\big)=0
$$

$$
\beta_0=\bar{y}-\beta_1\bar{x}
$$

---

## Derivative w.r.t $\beta_1$

$$
\hat{\beta}_1=
\frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}
{\sum_{i=1}^{n}(x_i-\bar{x})^2},
\qquad
\hat{\beta}_0=\bar{y}-\hat{\beta}_1\bar{x}
$$

---

# 📊 Error Metrics

## Residual Standard Error (RSE)

$$
\mathrm{RSE}
= \sqrt{\frac{\mathrm{RSS}}{n-2}}
$$

## Root Mean Squared Error (RMSE)

$$
\mathrm{RMSE}
= \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}
$$

## Coefficient of Determination ($R^2$)

$$
R^2 = 1-\frac{\mathrm{RSS}}{\mathrm{TSS}}
$$

$$
\mathrm{TSS}=\sum_{i=1}^{n}(y_i-\bar{y})^2
$$

---

## Correlation and $R^2$

$$
r = \frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}
{\sqrt{\sum_{i=1}^{n}(x_i-\bar{x})^2}\;
 \sqrt{\sum_{i=1}^{n}(y_i-\bar{y})^2}},
\qquad
R^2=r^2
$$

(Simple linear regression)

---

# ⚠️ Predictor Relationships

## 1) Uncorrelated Predictors (Ideal)

- Coefficients estimated independently  
- Interpretation clear  

---

## 2) Correlated Predictors (Multicollinearity)

- Variance of coefficients increases  
- Estimates become unstable  
- Interpretation difficult  
- Coefficient sign/magnitude unreliable  

---

## 3) Correlation ≠ Causation

- Observational data cannot prove causality  
- Requires experiments or strong assumptions  

---

# 📈 Multiple Linear Regression Example

$$
\text{Revenue}
= \alpha_0 + \alpha_1 \cdot \text{OnlineAds}
+ \alpha_2 \cdot \text{SocialMedia}
+ \alpha_3 \cdot \text{EmailCampaign}
+ \varepsilon
$$

$$
\alpha_0 = 3.412,\quad
\alpha_1 = 0.052,\quad
\alpha_2 = 0.137,\quad
\alpha_3 = 0.009
$$

---

## Correlation Matrix

$$
\begin{array}{c|cccc}
 & \text{OnlineAds} & \text{SocialMedia} & \text{EmailCampaign} & \text{Revenue} \\
\hline
\text{OnlineAds} & 1.0000 & 0.0721 & 0.0413 & 0.8015 \\
\text{SocialMedia} & 0.0721 & 1.0000 & 0.2987 & 0.6234 \\
\text{EmailCampaign} & 0.0413 & 0.2987 & 1.0000 & 0.1916 \\
\text{Revenue} & 0.8015 & 0.6234 & 0.1916 & 1.0000
\end{array}
$$

---

# 💡 Key Insight

$$
\boxed{
\text{Larger regression coefficient does NOT imply higher correlation}
}
$$

- Regression coefficients depend on **scale / units** of predictors  
- Correlation is **scale-invariant**  
- Newspaper: weak correlation + near-zero coefficient → little predictive value  

---

# 🚀 Final Takeaway

$$
\boxed{
\text{Linear Regression = Estimation + Error Analysis + Interpretation}
}
$$

---
---
# Prove Bias–Variance Decomposition
$$
\mathbb{E}\big[ (y_0 - \hat{f}(x_0))^2 \big]
= \mathbb{E}\big[(f(x_0) + \varepsilon - \hat{f}(x_0))^2\big]
$$

$$
= \mathbb{E}\big[(f(x_0) - \hat{f}(x_0) + \varepsilon)^2\big]
$$

$$
= \mathbb{E}\big[(f(x_0) - \hat{f}(x_0))^2
+ 2(f(x_0) - \hat{f}(x_0))\varepsilon
+ \varepsilon^2\big]
$$

$$
= \mathbb{E}\big[(f(x_0) - \hat{f}(x_0))^2\big]
+ 2\mathbb{E}\big[(f(x_0) - \hat{f}(x_0))\varepsilon\big]
+ \mathbb{E}[\varepsilon^2]
$$

$$
= \mathbb{E}\big[(f(x_0) - \hat{f}(x_0))^2\big]
+ \mathrm{Var}(\varepsilon)
$$
Now decompose the first term:
$$
\mathbb{E}\big[(f(x_0) - \hat{f}(x_0))^2\big]
= \mathbb{E}\big[(f(x_0) - \mathbb{E}[\hat{f}(x_0)] + \mathbb{E}[\hat{f}(x_0)] - \hat{f}(x_0))^2\big]
$$

$$
= \mathbb{E}\big[(f(x_0) - \mathbb{E}[\hat{f}(x_0)])^2
+ (\mathbb{E}[\hat{f}(x_0)] - \hat{f}(x_0))^2
+ 2(f(x_0) - \mathbb{E}[\hat{f}(x_0)])(\mathbb{E}[\hat{f}(x_0)] - \hat{f}(x_0))\big]
$$

$$
= (f(x_0) - \mathbb{E}[\hat{f}(x_0)])^2
+ \mathbb{E}\big[(\hat{f}(x_0) - \mathbb{E}[\hat{f}(x_0)])^2\big]
$$

$$
= \mathrm{Bias}(\hat{f}(x_0))^2 + \mathrm{Var}(\hat{f}(x_0))
$$

$$
\mathbb{E}\big[ (y_0 - \hat{f}(x_0))^2 \big]
= \mathrm{Var}(\hat{f}(x_0))
+ \mathrm{Bias}(\hat{f}(x_0))^2
+ \mathrm{Var}(\varepsilon)
$$
