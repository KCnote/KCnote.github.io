---
layout: post
title: "PART 5 — Advanced Linear Algebra"
date: 2026-02-18 00:00:00 +0900
author: kang
categories: [Linear Algebra, Temporary]
tags: [Linear Algebra]
pin: false
math: true
mermaid: true
---

# 📒 PART 5 — Advanced Linear Algebra 

This chapter covers:

1. Fundamental Theorem of Linear Algebra (FTLA) with numerical example  
2. Positive Definite matrices (multiple criteria)  
3. Fourier Transform as Linear Algebra (DFT matrix form)  
4. Non-negative Matrix Factorization (NMF) numerical example  

All derivations shown explicitly.

---

# 1️⃣ Fundamental Theorem of Linear Algebra (FTLA)

Consider matrix

$$
A =
\begin{bmatrix}
1 & 2 & 3 \\
2 & 4 & 6
\end{bmatrix}
$$

---

## Step 1: Row Reduce

Augmented (no RHS):

$$
\begin{bmatrix}
1 & 2 & 3 \\
2 & 4 & 6
\end{bmatrix}
$$

Row2 → Row2 − 2·Row1

$$
\begin{bmatrix}
1 & 2 & 3 \\
0 & 0 & 0
\end{bmatrix}
$$

Rank = 1

---

## Step 2: Column Space

Pivot column = first column

Basis for column space:

$$
\begin{bmatrix}
1 \\
2
\end{bmatrix}
$$

Dimension = 1

---

## Step 3: Null Space

Solve

$$
Ax = 0
$$

From reduced form:

$$
x_1 + 2x_2 + 3x_3 = 0
$$

Let

$$
x_2 = s, \quad x_3 = t
$$

Then

$$
x_1 = -2s -3t
$$

Thus null space basis:

$$
\begin{bmatrix}
-2 \\
1 \\
0
\end{bmatrix},
\quad
\begin{bmatrix}
-3 \\
0 \\
1
\end{bmatrix}
$$

Nullity = 2

---

## Rank-Nullity Theorem

$$
\text{rank}(A) + \text{nullity}(A) = n
$$

$$
1 + 2 = 3
$$

Verified.

---

# 2️⃣ Positive Definite Matrix

Let

$$
M =
\begin{bmatrix}
2 & 1 \\
1 & 2
\end{bmatrix}
$$

---

## Criterion 1: Eigenvalues

Characteristic polynomial:

$$
\det(M - \lambda I)
= (2-\lambda)^2 - 1
$$

$$
= \lambda^2 -4\lambda +3
$$

$$
(\lambda-3)(\lambda-1)=0
$$

Eigenvalues:

$$
3, 1 > 0
$$

Thus positive definite.

---

## Criterion 2: Leading Principal Minors

First minor:

$$
2 > 0
$$

Determinant:

$$
(2)(2)-1 = 3 > 0
$$

Positive definite confirmed.

---

# 3️⃣ Fourier Transform as Linear Algebra

Consider 2-point DFT.

Signal:

$$
x =
\begin{bmatrix}
x_0 \\
x_1
\end{bmatrix}
$$

DFT matrix:

$$
F =
\begin{bmatrix}
1 & 1 \\
1 & -1
\end{bmatrix}
$$

Transform:

$$
X = F x
$$

Compute:

$$
X_0 = x_0 + x_1
$$

$$
X_1 = x_0 - x_1
$$

Thus Fourier transform = basis change.

---

# 4️⃣ Non-Negative Matrix Factorization (NMF)

Let

$$
V =
\begin{bmatrix}
2 & 3 \\
4 & 6
\end{bmatrix}
$$

We approximate

$$
V \approx W H
$$

Choose rank 1.

Let

$$
W =
\begin{bmatrix}
a \\
b
\end{bmatrix},
\quad
H =
\begin{bmatrix}
c & d
\end{bmatrix}
$$

Thus

$$
WH =
\begin{bmatrix}
ac & ad \\
bc & bd
\end{bmatrix}
$$

Match entries:

$$
ac = 2, \quad ad = 3
$$

$$
bc = 4, \quad bd = 6
$$

Divide equations:

$$
\frac{ad}{ac} = \frac{3}{2}
$$

$$
\frac{bd}{bc} = \frac{6}{4} = \frac{3}{2}
$$

Consistent ratio.

Choose:

$$
a = 1, \quad c = 2
$$

Then:

$$
b = 2
$$

Choose:

$$
d = 3
$$

Thus:

$$
W =
\begin{bmatrix}
1 \\
2
\end{bmatrix},
\quad
H =
\begin{bmatrix}
2 & 3
\end{bmatrix}
$$

Exact rank-1 factorization achieved.

---

# 🔥 Key Takeaways

- FTLA links row space, column space, and null space  
- Rank-nullity verified numerically  
- Positive definite matrices have positive eigenvalues  
- Fourier transform is a linear basis transformation  
- NMF approximates matrices using non-negative factors  



행렬 곱 / 내적 계산, cos / 상관계수와 벡터내적 관계 / 고유값 고유벡터 계산방법 / PCA 계산방법, 공분산으로 분산최대화 방법, PCA로 차원축소방법 / LU분해 / 회전행렬의 고유값 고유벡터 / 고유값 분해 (EVD) / fundamental theorem of linear algebra / 대칭행렬의 고유벡터는 왜 직교인가 / SVD / SVD 이미지 압축 예시 / 가우스 조던행렬소거법 / 푸리에 변환(선형대수학이랑 연관해서) / 양의 정부호 Non-negative Matrix Factorization(NMF) 마할라노비스 거리 이거에 대한 (해석 중심보단 수치계산 예시 중심으로) 마크다운 만들껀데 적당히 항목을 어떻 게 나누면 좋을까