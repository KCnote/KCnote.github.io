---
layout: post
title: "Least Squares Method (LSM) Explained Step by Step with Examples"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Mathematics]
tags: [Computer Vision, Mathematics, Least Squares, LSM, Linear Algebra, Optimization]
pin: false
math: true
mermaid: true
---

## 🎯 What Is Least Squares Method (LSM)?

The **Least Squares Method (LSM)** finds parameters that **best fit data** by minimizing the **sum of squared errors**.

It is used everywhere:
- Curve fitting
- Camera calibration
- Homography estimation
- Regression
- Sensor noise reduction

---

## 🔢 Step 1: Define the Model

Assume a **linear model**:

$$
y = ax + b
$$

Unknown parameters:
- slope
$$
a
$$
- intercept
$$
b
$$

---

## 📊 Step 2: Collect Measurements

Example data points:

| x | y |
|---|---|
| 1 | 2 |
| 2 | 2 |
| 3 | 4 |

These points do **not** lie perfectly on a line.

---

## ❌ Step 3: Define the Error (Residual)

For each data point:

$$
e_i = y_i - (a x_i + b)
$$

Residual vector:

$$
\mathbf{e} =
\begin{bmatrix}
e_1 \\
e_2 \\
e_3
\end{bmatrix}
$$

---

## 📉 Step 4: Define the Cost Function

Least squares minimizes:

$$
J(a,b) = \sum_i e_i^2
$$

Vector form:

$$
J = \| \mathbf{e} \|^2
$$

---

## 🧮 Step 5: Write in Matrix Form

Model for all points:

$$
\begin{bmatrix}
y_1 \\
y_2 \\
y_3
\end{bmatrix}
=
\begin{bmatrix}
x_1 & 1 \\
x_2 & 1 \\
x_3 & 1
\end{bmatrix}
\begin{bmatrix}
a \\
b
\end{bmatrix}
$$

Compactly:

$$
\mathbf{y} = \mathbf{A}\mathbf{\theta}
$$

Where:
- design matrix 
 $$
 \mathbf{A}
 $$ 
$$\mathbf{\theta} = [a, b]^T$$

---

## 🔁 Step 6: Overdetermined System

Here:
- Equations = 3
- Unknowns = 2

So no exact solution exists.

We solve:

$$
\min_{\mathbf{\theta}} \| \mathbf{A}\mathbf{\theta} - \mathbf{y} \|^2
$$

---

## ✏️ Step 7: Derive Normal Equation

Set gradient to zero:

$$
\mathbf{A}^T (\mathbf{A}\mathbf{\theta} - \mathbf{y}) = 0
$$

Which gives:

$$
\mathbf{A}^T \mathbf{A} \mathbf{\theta} = \mathbf{A}^T \mathbf{y}
$$

---

## 🧠 Step 8: Solve Parameters

If $$\mathbf{A}^T\mathbf{A}$$ is invertible:

$$
\mathbf{\theta} = (\mathbf{A}^T \mathbf{A})^{-1} \mathbf{A}^T \mathbf{y}
$$

This is the **Least Squares Solution**.

---

## 🔢 Step 9: Numerical Example

From data:

$$
\mathbf{A} =
\begin{bmatrix}
1 & 1 \\
2 & 1 \\
3 & 1
\end{bmatrix}
\quad
\mathbf{y} =
\begin{bmatrix}
2 \\
2 \\
4
\end{bmatrix}
$$

Compute:

$$
\mathbf{A}^T \mathbf{A} =
\begin{bmatrix}
14 & 6 \\
6 & 3
\end{bmatrix}
$$

$$
\mathbf{A}^T \mathbf{y} =
\begin{bmatrix}
18 \\
8
\end{bmatrix}
$$

Solve:

$$
\begin{bmatrix}
14 & 6 \\
6 & 3
\end{bmatrix}
\begin{bmatrix}
a \\
b
\end{bmatrix}
=
\begin{bmatrix}
18 \\
8
\end{bmatrix}
$$

Result:

$$
a = 1 \quad b = 0.33
$$

---

## 🔍 Geometric Interpretation

- Each equation = a line (or plane)
- LSM finds the **closest point** to intersection
- Error minimized in **orthogonal projection** sense

---

## ⚠️ Numerical Stability

Problems:
- It can be ill-conditioned
$$
\mathbf{A}^T\mathbf{A}
$$ 


Solutions:
- Use **SVD**
- Use **QR decomposition**
- Normalize data

---

## 🔗 Relation to Pseudo-Inverse

Using Moore–Penrose pseudo-inverse:

$$
\mathbf{\theta} = \mathbf{A}^+ \mathbf{y}
$$

Where:

$$
\mathbf{A}^+ = (\mathbf{A}^T\mathbf{A})^{-1}\mathbf{A}^T
$$

---

## 🎯 Takeaway

Least Squares Method:
- Handles noisy, redundant data
- Converts geometry into algebra
- Is the backbone of modern computer vision

If you understand LSM, you understand **why homography, calibration, and fitting work** 🚀
