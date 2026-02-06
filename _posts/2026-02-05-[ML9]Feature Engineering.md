---
layout: post
title: "Feature Engineering"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Mathematics]
tags: [Machince Learning, Mathematics, Variable, Linear Regression, Statistics]
pin: false
math: true
mermaid: true
---

# 📊 Feature Engineering in Linear Models

---

# One-Hot Encoding

## Definition
One-hot encoding is a widely used method to represent **categorical variables** as numerical vectors.

A categorical variable with $$k$$ categories is transformed into $$k$$ binary variables.

---

## Example

Categorical variable: **Category = {A, B, C}**

| Category | A | B | C |
|---------|---|---|---|
| A | 1 | 0 | 0 |
| B | 0 | 1 | 0 |
| C | 0 | 0 | 1 |

---

## Regression Example

Suppose we model:

$$
y = \beta_0 + \beta_1 \cdot A + \beta_2 \cdot B + \beta_3 \cdot C + \varepsilon
$$

To avoid **multicollinearity (dummy variable trap)**, one category is usually dropped:

$$
y = \beta_0 + \beta_1 \cdot A + \beta_2 \cdot B + \varepsilon
$$

Here, **C is the reference category**.

---

## Example Interpretation

- difference between A and C: $$\beta_1$$
- difference between B and C: $$\beta_2$$

---

# Interaction Terms

## Model with Interaction

$$
y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 x_1 x_2 + \varepsilon
$$

Example:

$$
\text{Output} = \beta_0 + \beta_1 \cdot \text{Feature1} + \beta_2 \cdot \text{Feature2}
+ \beta_3 (\text{Feature1} \cdot \text{Feature2}) + \varepsilon
$$

- positive interaction: $$\beta_3 > 0$$
- negative interaction: $$\beta_3 < 0$$

---

## Hierarchy Principle

If an interaction is included, the corresponding main effects must also be included.

Example:
- If model includes $$x y$$, then both $$x$$ and $$y$$ must be included.
- If model includes $$x y z$$, then $$x y$$, $$y z$$, $$x z$$, and main effects should be included.

---

## Why Important

- Interaction without main effects is hard to interpret  
- Interaction implicitly contains main-effect information  
- Improves model correctness  

---

# Nonlinear Relationships in Linear Models

## Polynomial Regression

$$
y = \beta_0 + \beta_1 z + \beta_2 z^2 + \varepsilon
$$

Let:

$$
z_1 = z, \quad z_2 = z^2
$$

Then:

$$
y = \beta_0 + \beta_1 z_1 + \beta_2 z_2 + \varepsilon
$$

Still linear in coefficients → **Linear Regression**.

---

## Example

| Term | Coefficient |
|------|------------|
| Intercept | 48.321 |
| z | -0.372 |
| z² | 0.0009 |

---

## Takeaway

- Nonlinear relationship modeled via transformed predictors  
- Linear in parameters ⇒ still linear regression  
- Called **Polynomial Regression**  

---

# Feature Selection

## Motivation

- Sometimes $$X^T X$$ is not invertible  
- Sometimes coefficients become unstable  
- Highly correlated features cause numerical issues  

Example:

$$
x + y = 4
$$

$$
2x + 2y \approx 8.0000000001
$$

This causes near-singular matrix ⇒ unstable inverse.

---

## Idea in Machine Learning

Instead of manually selecting variables, ML models can automatically evaluate feature usefulness.

---

## Takeaway

- One-hot encoding converts categorical → numeric  
- Interaction captures joint effect of variables  
- Polynomial terms model nonlinear relationships  
- Feature selection improves stability and performance  
