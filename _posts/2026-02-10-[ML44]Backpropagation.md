---
layout: post
title: "Backpropagation"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, MLP]
tags: [Artificial Intelligence, MLP]
pin: false
math: true
mermaid: true
---

# 🔁 Computational Graphs & Backpropagation (with Vectors & Matrices)

> **Goal:** Understand backpropagation as **systematic application of the chain rule** on a **computational graph** — from scalars → vectors → matrices.  
> ✅ Includes **full derivations**, **numeric examples**, and **matrix-shape sanity checks**.  
> 🧠 Tip: Always track **(1) the value**, **(2) the local derivative**, **(3) the upstream gradient**.

---


## 0) 🧩 What is a Computational Graph?

A **computational graph** expresses a function as a composition of simple operations (nodes):

- Inputs: $x, W, b, \dots$
- Ops: $+, \times, \exp, \log, \max, \dots$
- Output: $f$ (or loss $\mathcal{L}$)

Example (affine / linear layer):

$$
f(x,W,b) = Wx + b
$$

We compute:

- **Forward pass:** compute values from inputs → output
- **Backward pass (backprop):** compute gradients from output → inputs

---

## 1) 🏃 Forward vs Backward (Core Idea)

### ✅ Forward pass
Compute intermediate values:

$$
u = Wx,\quad f = u + b
$$

### ✅ Backward pass
We want derivatives like:

$$
\frac{\partial f}{\partial W},\quad \frac{\partial f}{\partial x},\quad \frac{\partial f}{\partial b}
$$

Backprop works because of the chain rule:

> **Upstream gradient** × **Local gradient** = **Downstream gradient**

---

## 2) 🧪 Scalar Backprop Example: $f(x,y,z) = (x+y)z$

### 2.1 Forward pass (numeric)

Given:

$$
x=-2,\quad y=5,\quad z=-4
$$

Compute:

1) Add node:

$$
q = x+y = -2+5 = 3
$$

2) Multiply node:

$$
f = qz = 3\cdot(-4) = -12
$$

So:

- $q=3$
- $f=-12$

---

### 2.2 Backward pass (full derivation)

We compute partial derivatives of $f$ w.r.t. each input.

#### Step A) Derivatives wrt $q$ and $z$

From:

$$
f = qz
$$

Treating $q$ and $z$ as independent variables:

$$
\frac{\partial f}{\partial q} = z
$$

Because:

$$
f = qz \Rightarrow \frac{\partial}{\partial q}(qz)=z
$$

Similarly:

$$
\frac{\partial f}{\partial z} = q
$$

Because:

$$
f=qz \Rightarrow \frac{\partial}{\partial z}(qz)=q
$$

Now plug values $q=3, z=-4$:

$$
\frac{\partial f}{\partial q} = -4,\quad \frac{\partial f}{\partial z} = 3
$$

#### Step B) Derivatives wrt $x$ and $y$

We also have:

$$
q = x+y
$$

So:

$$
\frac{\partial q}{\partial x}=1,\quad \frac{\partial q}{\partial y}=1
$$

Now chain rule:

$$
\frac{\partial f}{\partial x} = \frac{\partial f}{\partial q}\cdot\frac{\partial q}{\partial x}
$$

Substitute:

$$
\frac{\partial f}{\partial x} = (-4)\cdot 1 = -4
$$

Similarly:

$$
\frac{\partial f}{\partial y} = \frac{\partial f}{\partial q}\cdot\frac{\partial q}{\partial y} = (-4)\cdot 1 = -4
$$

And $z$ was direct from Step A:

$$
\frac{\partial f}{\partial z} = 3
$$

✅ Final gradients:

$$
\boxed{\frac{\partial f}{\partial x}=-4,\ \frac{\partial f}{\partial y}=-4,\ \frac{\partial f}{\partial z}=3}
$$

---

## 3) 🔗 Chain Rule on Graphs: Upstream / Local / Downstream

### 3.1 The chain rule (scalar form)

If:

$$
z=f(u),\quad u=g(x)
$$

Then:

$$
\frac{dz}{dx}=\frac{dz}{du}\cdot\frac{du}{dx}
$$

In backprop language:

- Upstream gradient: $\frac{dz}{du}$
- Local gradient: $\frac{du}{dx}$
- Downstream gradient: $\frac{dz}{dx}$

---

## 4) 🧠 “Gate” Patterns in Gradient Flow


Let upstream gradient be $g = \frac{\partial \mathcal{L}}{\partial \text{out}}$.

### 4.1 Add gate: $s=a+b$

$$
s=a+b
$$

Local gradients:

$$
\frac{\partial s}{\partial a}=1,\quad \frac{\partial s}{\partial b}=1
$$

So:

$$
\frac{\partial \mathcal{L}}{\partial a}=g\cdot 1=g,\quad
\frac{\partial \mathcal{L}}{\partial b}=g\cdot 1=g
$$

✅ **Add gate copies gradient** (distributes).

---

### 4.2 Multiply gate: $p=ab$

$$
p=ab
$$

Local gradients:

$$
\frac{\partial p}{\partial a}=b,\quad \frac{\partial p}{\partial b}=a
$$

So:

$$
\frac{\partial \mathcal{L}}{\partial a}=g\cdot b,\quad
\frac{\partial \mathcal{L}}{\partial b}=g\cdot a
$$

✅ **Multiply gate swaps multiplier** (each gets the other value).

---

### 4.3 Copy / split gate

If a value $v$ feeds into two branches and later contributes to loss, gradients **sum**:

If:

$$
\mathcal{L}=\mathcal{L}_1(v)+\mathcal{L}_2(v)
$$

Then:

$$
\frac{\partial \mathcal{L}}{\partial v}
=
\frac{\partial \mathcal{L}_1}{\partial v}
+
\frac{\partial \mathcal{L}_2}{\partial v}
$$

✅ **Branch merge ⇒ gradient add**.

---

### 4.4 Max gate: $m=\max(a,b)$

Piecewise:

- If $a>b$, then $m=a$ so gradient goes to $a$
- If $b>a$, then $m=b$ so gradient goes to $b$

Formally:

If $a>b$:

$$
\frac{\partial m}{\partial a}=1,\quad \frac{\partial m}{\partial b}=0
$$

If $b>a$:

$$
\frac{\partial m}{\partial a}=0,\quad \frac{\partial m}{\partial b}=1
$$

✅ **Max gate routes gradient**.

---

## 5) 📈 Logistic Regression Backprop (Graph View)

### 5.1 Model definition

Binary logistic regression with input vector $x\in\mathbb{R}^d$:

$$
s = w^Tx + b
$$

Sigmoid:

$$
\hat{y} = \sigma(s) = \frac{1}{1+e^{-s}}
$$

---

### 5.2 Derivative of sigmoid (full derivation)

Start:

$$
\sigma(s)=\frac{1}{1+e^{-s}} = (1+e^{-s})^{-1}
$$

Differentiate:

$$
\frac{d\sigma}{ds} = -1\cdot(1+e^{-s})^{-2}\cdot \frac{d}{ds}(1+e^{-s})
$$

Compute inner derivative:

$$
\frac{d}{ds}(1+e^{-s}) = 0 + \frac{d}{ds}(e^{-s})
$$

Since:

$$
\frac{d}{ds}(e^{-s}) = e^{-s}\cdot\frac{d}{ds}(-s) = e^{-s}\cdot(-1) = -e^{-s}
$$

So:

$$
\frac{d\sigma}{ds}
= -(1+e^{-s})^{-2}\cdot(-e^{-s})
= \frac{e^{-s}}{(1+e^{-s})^2}
$$

Now express in terms of $\sigma(s)$. Note:

$$
\sigma(s)=\frac{1}{1+e^{-s}}
\Rightarrow 1-\sigma(s)=1-\frac{1}{1+e^{-s}}=\frac{e^{-s}}{1+e^{-s}}
$$

Multiply:

$$
\sigma(s)(1-\sigma(s))
=
\frac{1}{1+e^{-s}}\cdot\frac{e^{-s}}{1+e^{-s}}
=
\frac{e^{-s}}{(1+e^{-s})^2}
$$

Thus:

$$
\boxed{\frac{d\sigma}{ds}=\sigma(s)\bigl(1-\sigma(s)\bigr)}
$$

---

### 5.3 Add a loss and backprop

Common binary cross-entropy loss (single sample):

$$
\mathcal{L} = -\Bigl(y\log(\hat{y}) + (1-y)\log(1-\hat{y})\Bigr)
$$

We will derive $\frac{\partial \mathcal{L}}{\partial s}$ step-by-step.

First compute:

$$
\frac{\partial \mathcal{L}}{\partial \hat{y}}
=
-\left(
y\cdot\frac{1}{\hat{y}} + (1-y)\cdot\frac{1}{1-\hat{y}}\cdot(-1)
\right)
$$

Because:

- $\frac{d}{d\hat{y}}\log(\hat{y}) = \frac{1}{\hat{y}}$
- $\frac{d}{d\hat{y}}\log(1-\hat{y}) = \frac{1}{1-\hat{y}}\cdot(-1)$

So:

$$
\frac{\partial \mathcal{L}}{\partial \hat{y}}
=
-\left(\frac{y}{\hat{y}} - \frac{1-y}{1-\hat{y}}\right)
=
-\frac{y}{\hat{y}} + \frac{1-y}{1-\hat{y}}
$$

Now chain with $\hat{y}=\sigma(s)$:

$$
\frac{\partial \mathcal{L}}{\partial s}
=
\frac{\partial \mathcal{L}}{\partial \hat{y}}\cdot
\frac{\partial \hat{y}}{\partial s}
$$

Substitute $\frac{\partial \hat{y}}{\partial s}=\hat{y}(1-\hat{y})$:

$$
\frac{\partial \mathcal{L}}{\partial s}
=
\left(-\frac{y}{\hat{y}} + \frac{1-y}{1-\hat{y}}\right)\cdot \hat{y}(1-\hat{y})
$$

Distribute:

$$
\frac{\partial \mathcal{L}}{\partial s}
=
-\frac{y}{\hat{y}}\hat{y}(1-\hat{y})
+
\frac{1-y}{1-\hat{y}}\hat{y}(1-\hat{y})
$$

Cancel terms:

$$
-\frac{y}{\hat{y}}\hat{y}(1-\hat{y}) = -y(1-\hat{y})
$$

$$
\frac{1-y}{1-\hat{y}}\hat{y}(1-\hat{y}) = (1-y)\hat{y}
$$

So:

$$
\frac{\partial \mathcal{L}}{\partial s}
=
-y(1-\hat{y}) + (1-y)\hat{y}
=
-y + y\hat{y} + \hat{y} - y\hat{y}
=
\hat{y} - y
$$

✅ Key result:

$$
\boxed{\frac{\partial \mathcal{L}}{\partial s} = \hat{y}-y}
$$

Then:

$$
s=w^Tx+b
$$

So:

$$
\frac{\partial s}{\partial w} = x,\quad
\frac{\partial s}{\partial b} = 1,\quad
\frac{\partial s}{\partial x} = w
$$

Therefore:

$$
\boxed{\frac{\partial \mathcal{L}}{\partial w} = (\hat{y}-y)x}
$$

$$
\boxed{\frac{\partial \mathcal{L}}{\partial b} = \hat{y}-y}
$$

$$
\boxed{\frac{\partial \mathcal{L}}{\partial x} = (\hat{y}-y)w}
$$

---

### 5.4 Numeric mini-example (one sample)

Let:

$$
x=\begin{bmatrix}2\\-1\end{bmatrix},\quad
w=\begin{bmatrix}0.3\\-0.7\end{bmatrix},\quad
b=0.1,\quad
y=1
$$

Compute score:

$$
s=w^Tx+b = (0.3)(2)+(-0.7)(-1)+0.1 = 0.6+0.7+0.1 = 1.4
$$

Sigmoid:

$$
\hat{y}=\frac{1}{1+e^{-1.4}}
$$

Compute $e^{-1.4}\approx 0.2466$:

$$
\hat{y}\approx \frac{1}{1+0.2466}\approx 0.8022
$$

Gradient core:

$$
\hat{y}-y \approx 0.8022-1 = -0.1978
$$

So:

$$
\frac{\partial \mathcal{L}}{\partial w}=(\hat{y}-y)x
\approx (-0.1978)\begin{bmatrix}2\\-1\end{bmatrix}
=
\begin{bmatrix}-0.3956\\0.1978\end{bmatrix}
$$

$$
\frac{\partial \mathcal{L}}{\partial b}=\hat{y}-y\approx -0.1978
$$

$$
\frac{\partial \mathcal{L}}{\partial x}=(\hat{y}-y)w
\approx (-0.1978)\begin{bmatrix}0.3\\-0.7\end{bmatrix}
=
\begin{bmatrix}-0.0593\\0.1385\end{bmatrix}
$$

---

## 6) 🧱 From Scalars to Vectors: What changes?

### 6.1 Scalar → Scalar

If:

$$
x\in\mathbb{R},\ y\in\mathbb{R}
$$

Then derivative:

$$
\frac{dy}{dx}\in\mathbb{R}
$$

---

### 6.2 Vector → Scalar (gradient)

If:

$$
x\in\mathbb{R}^n,\ y\in\mathbb{R}
$$

Then gradient:

$$
\frac{\partial y}{\partial x}\in\mathbb{R}^n
$$

Concretely:

$$
\frac{\partial y}{\partial x}
=
\begin{bmatrix}
\frac{\partial y}{\partial x_1}\\
\vdots\\
\frac{\partial y}{\partial x_n}
\end{bmatrix}
$$

---

### 6.3 Vector → Vector (Jacobian)

If:

$$
x\in\mathbb{R}^n,\ y\in\mathbb{R}^m
$$

Then:

$$
\frac{\partial y}{\partial x}\in\mathbb{R}^{m\times n}
$$

This is the **Jacobian**:

$$
J
=
\frac{\partial y}{\partial x}
=
\begin{bmatrix}
\frac{\partial y_1}{\partial x_1} & \cdots & \frac{\partial y_1}{\partial x_n}\\
\vdots & \ddots & \vdots\\
\frac{\partial y_m}{\partial x_1} & \cdots & \frac{\partial y_m}{\partial x_n}
\end{bmatrix}
$$

---

## 7) 🧭 Backprop with Vectors = Jacobianᵀ × Upstream Gradient

Suppose a vector function:

$$
z=f(x),\quad x\in\mathbb{R}^n,\ z\in\mathbb{R}^m
$$

And the loss is scalar:

$$
\mathcal{L}=\mathcal{L}(z)
$$

Backprop gives upstream gradient:

$$
\frac{\partial \mathcal{L}}{\partial z}\in\mathbb{R}^{m}
$$

We want:

$$
\frac{\partial \mathcal{L}}{\partial x}\in\mathbb{R}^{n}
$$

Chain rule (vector form):

$$
\boxed{
\frac{\partial \mathcal{L}}{\partial x}
=
\left(\frac{\partial z}{\partial x}\right)^T
\frac{\partial \mathcal{L}}{\partial z}
}
$$

✅ **Shape check**:

$$
(n\times m)(m\times 1)=(n\times 1)
$$

---

## 8) ✅ Vector Example: ReLU / max(x,0)

Define elementwise ReLU:

$$
z = \max(x,0)
$$

Where $x,z\in\mathbb{R}^n$, and:

$$
z_i=\max(x_i,0)
$$

### 8.1 Local derivative (elementwise)

For each component:

If $x_i>0$:

$$
\frac{\partial z_i}{\partial x_i}=1
$$

If $x_i\le 0$:

$$
\frac{\partial z_i}{\partial x_i}=0
$$

Also, for $i\ne j$, $z_i$ depends only on $x_i$:

$$
\frac{\partial z_i}{\partial x_j}=0\quad(i\ne j)
$$

So Jacobian is diagonal:

$$
\frac{\partial z}{\partial x}
=
\operatorname{diag}(m_1,\dots,m_n)
$$

Where mask:

$$
m_i=
\begin{cases}
1 & x_i>0\\
0 & x_i\le 0
\end{cases}
$$

### 8.2 Numeric example (explicit)

Let:

$$
x=
\begin{bmatrix}
1\\
-2\\
3\\
-1
\end{bmatrix}
\Rightarrow
z=\max(x,0)=
\begin{bmatrix}
1\\
0\\
3\\
0
\end{bmatrix}
$$

Mask:

$$
m=
\begin{bmatrix}
1\\
0\\
1\\
0
\end{bmatrix}
$$

So Jacobian:

$$
J=
\frac{\partial z}{\partial x}
=
\begin{bmatrix}
1&0&0&0\\
0&0&0&0\\
0&0&1&0\\
0&0&0&0
\end{bmatrix}
$$

If upstream gradient is:

$$
\frac{\partial \mathcal{L}}{\partial z}
=
\begin{bmatrix}
4\\
-1\\
5\\
9
\end{bmatrix}
$$

Then:

$$
\frac{\partial \mathcal{L}}{\partial x}
=
J^T
\frac{\partial \mathcal{L}}{\partial z}
=
J
\frac{\partial \mathcal{L}}{\partial z}
=
\begin{bmatrix}
4\\
0\\
5\\
0
\end{bmatrix}
$$

✅ Interpretation: gradient passes only through active ReLU units.

---

## 9) 🧮 Backprop with Matrices

We focus on the standard affine layer.

### 9.1 Single sample: $z = Wx + b$

Let:

- $x\in\mathbb{R}^{d_x}$
- $W\in\mathbb{R}^{d_z\times d_x}$
- $b\in\mathbb{R}^{d_z}$
- $z\in\mathbb{R}^{d_z}$

Forward:

$$
z = Wx + b
$$

Assume we already have upstream gradient:

$$
g=\frac{\partial \mathcal{L}}{\partial z}\in\mathbb{R}^{d_z}
$$

We want:

$$
\frac{\partial \mathcal{L}}{\partial x},\ 
\frac{\partial \mathcal{L}}{\partial W},\
\frac{\partial \mathcal{L}}{\partial b}
$$

---

### 9.2 Derive $\frac{\partial \mathcal{L}}{\partial b}$

For each component:

$$
z_i = \sum_{j=1}^{d_x} W_{ij}x_j + b_i
$$

So:

$$
\frac{\partial z_i}{\partial b_k}
=
\begin{cases}
1 & i=k\\
0 & i\ne k
\end{cases}
$$

Thus:

$$
\boxed{\frac{\partial \mathcal{L}}{\partial b}=g}
$$

---

### 9.3 Derive $\frac{\partial \mathcal{L}}{\partial x}$

From:

$$
\frac{\partial z_i}{\partial x_k} = W_{ik}
$$

So $\frac{\partial z}{\partial x}=W$. Hence:

$$
\boxed{\frac{\partial \mathcal{L}}{\partial x}=W^Tg}
$$

---

### 9.4 Derive $\frac{\partial \mathcal{L}}{\partial W}$

Elementwise:

$$
\frac{\partial \mathcal{L}}{\partial W_{ij}}
=
\sum_{k=1}^{d_z}
\frac{\partial \mathcal{L}}{\partial z_k}
\frac{\partial z_k}{\partial W_{ij}}
$$

But only $k=i$ survives, giving:

$$
\frac{\partial \mathcal{L}}{\partial W_{ij}} = g_i x_j
$$

So:

$$
\boxed{
\frac{\partial \mathcal{L}}{\partial W}
=
g x^T
}
$$

---

### 9.5 Numeric matrix example (small and explicit)

Let:

$$
W=
\begin{bmatrix}
1 & 2\\
3 & 4
\end{bmatrix},\quad
x=
\begin{bmatrix}
5\\
6
\end{bmatrix},\quad
b=
\begin{bmatrix}
7\\
8
\end{bmatrix}
$$

Forward:

$$
z=Wx+b=
\begin{bmatrix}
1\cdot5+2\cdot6\\
3\cdot5+4\cdot6
\end{bmatrix}
+
\begin{bmatrix}7\\8\end{bmatrix}
=
\begin{bmatrix}
24\\
47
\end{bmatrix}
$$

Assume upstream gradient:

$$
g=\frac{\partial \mathcal{L}}{\partial z}=
\begin{bmatrix}
10\\
-1
\end{bmatrix}
$$

Then:

$$
\frac{\partial \mathcal{L}}{\partial b}=g=
\begin{bmatrix}
10\\
-1
\end{bmatrix}
$$

$$
\frac{\partial \mathcal{L}}{\partial x}=W^Tg=
\begin{bmatrix}
1 & 3\\
2 & 4
\end{bmatrix}
\begin{bmatrix}
10\\
-1
\end{bmatrix}
=
\begin{bmatrix}
7\\
16
\end{bmatrix}
$$

$$
\frac{\partial \mathcal{L}}{\partial W}=g x^T
=
\begin{bmatrix}
10\\
-1
\end{bmatrix}
\begin{bmatrix}
5 & 6
\end{bmatrix}
=
\begin{bmatrix}
50 & 60\\
-5 & -6
\end{bmatrix}
$$

---

## 10) 🧺 Mini-batch Version

Let:

- $X\in\mathbb{R}^{N\times d_x}$ (rows are samples)
- $W\in\mathbb{R}^{d_x\times d_z}$
- $b\in\mathbb{R}^{d_z}$ (broadcast)
- $Z=XW + \mathbf{1}b^T\in\mathbb{R}^{N\times d_z}$

Upstream gradient:

$$
G=\frac{\partial \mathcal{L}}{\partial Z}\in\mathbb{R}^{N\times d_z}
$$

Then:

$$
\boxed{
\frac{\partial \mathcal{L}}{\partial X}=GW^T
}
$$

$$
\boxed{
\frac{\partial \mathcal{L}}{\partial W}=X^T G
}
$$

$$
\boxed{
\frac{\partial \mathcal{L}}{\partial b}=\sum_{n=1}^{N} G_{n,:}
}
$$

---

## 11) ✅ Backprop Checklist

### 🧾 Step-by-step
1) **Forward**: store intermediate tensors needed for gradients  
2) Start from **upstream gradient** at the output  
3) For each node:
   - compute **local gradient**
   - multiply with upstream
   - if fan-out: **sum** gradients from branches
4) **Shape check** at every step ✅

### 🧨 Common bugs
- missing transpose $W^T$ in $\frac{\partial \mathcal{L}}{\partial x}=W^Tg$
- row/column convention mismatch between notes and code
- bias broadcasting but forgetting batch-sum for $\partial \mathcal{L}/\partial b$
- ReLU mask wrong (use pre-activation $x>0$ as standard)

---

## 12) 🔥 Why this matters: Automatic Differentiation

Autodiff frameworks do exactly this:

- decompose into primitive ops
- store forward values
- propagate gradients backward with chain rule
