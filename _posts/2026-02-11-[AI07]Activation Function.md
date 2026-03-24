---
layout: post
title: "Activation Functions"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Optimization]
tags: [Artificial Intelligence, Optimization]
pin: false
math: true
mermaid: true
---

# 🧠 1. Why Activation Functions Matter

Without activation:

$$
f(x) = W_2(W_1 x)
$$

This simplifies to:

$$
f(x) = (W_2 W_1)x
$$

❌ Still linear.\
❌ Cannot model complex patterns.

With activation:

$$
f(x) = W_2 a(W_1 x)
$$

✅ Introduces non-linearity\
✅ Enables deep learning power

------------------------------------------------------------------------

# 🔵 2. Sigmoid Function

## 📌 Definition

$$
\sigma(x) = \frac{1}{1 + e^{-x}}
$$

Range:

$$
0 < \sigma(x) < 1
$$

------------------------------------------------------------------------

## 🧮 Full Derivative Derivation

Start:

$$
\sigma(x) = (1 + e^{-x})^{-1}
$$

Differentiate:

$$
\frac{d}{dx}\sigma(x)
= -1(1+e^{-x})^{-2}(-e^{-x})
$$

$$
= \frac{e^{-x}}{(1+e^{-x})^2}
$$

Rewrite:

$$
= \frac{1}{1+e^{-x}}\left(1 - \frac{1}{1+e^{-x}}\right)
$$

Therefore:

$$
\sigma'(x) = \sigma(x)(1-\sigma(x))
$$

------------------------------------------------------------------------

## ⚠️ Vanishing Gradient

If $x \to +\infty$:

$$
\sigma'(x) \to 0
$$

If $x \to -\infty$:

$$
\sigma'(x) \to 0
$$

🚨 Gradients vanish in deep networks.

------------------------------------------------------------------------

## ⚠️ Not Zero-Centered

$$
\sigma(x) > 0
$$

All outputs positive → inefficient zig-zag gradient updates.

------------------------------------------------------------------------

# 🟢 3. Tanh Function

## 📌 Definition

$$
\tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}
$$

Relation:

$$
\tanh(x) = 2\sigma(2x) - 1
$$

Range:

$$
-1 < \tanh(x) < 1
$$

------------------------------------------------------------------------

## 🧮 Derivative

$$
\frac{d}{dx}\tanh(x) = 1 - \tanh^2(x)
$$

✔ Zero-centered\
❌ Still saturates for large $|x|$

------------------------------------------------------------------------

# 🔴 4. ReLU (Rectified Linear Unit)

## 📌 Definition

$$
\text{ReLU}(x) = \max(0,x)
$$

Piecewise:

$$
\text{ReLU}(x) =
\begin{cases}
x & x > 0 \\
0 & x \le 0
\end{cases}
$$

------------------------------------------------------------------------

## 🧮 Derivative

$$
\text{ReLU}'(x) =
\begin{cases}
1 & x > 0 \\
0 & x < 0
\end{cases}
$$

✔ No saturation for $x>0$\
✔ Fast computation\
✔ Sparse activation

------------------------------------------------------------------------

## ⚠️ Dead ReLU

If neuron always outputs 0:

$$
\nabla = 0
$$

Weights stop updating ❌

------------------------------------------------------------------------

# 🟡 5. Leaky ReLU

## 📌 Definition

$$
f(x) =
\begin{cases}
x & x > 0 \\
\alpha x & x \le 0
\end{cases}
$$

------------------------------------------------------------------------

## 🧮 Derivative

$$
f'(x) =
\begin{cases}
1 & x > 0 \\
\alpha & x \le 0
\end{cases}
$$

✔ Prevents dead neurons\
✔ Keeps small gradient for negative region

------------------------------------------------------------------------

# 🟣 6. ELU (Exponential Linear Unit)

## 📌 Definition

$$
\text{ELU}(x) =
\begin{cases}
x & x \ge 0 \\
\alpha(e^x - 1) & x < 0
\end{cases}
$$

------------------------------------------------------------------------

## 🧮 Derivative

For $x \ge 0$:

$$
\frac{d}{dx} = 1
$$

For $x < 0$:

$$
\frac{d}{dx} = \alpha e^x
$$

✔ Closer to zero-centered\
✔ Smooth negative region

------------------------------------------------------------------------

# 📊 7. Comparison Table

  Activation   Saturation   Zero-Centered   Vanishing Gradient
  ------------ ------------ --------------- --------------------
  Sigmoid      Yes          No              Severe
  Tanh         Yes          Yes             Moderate
  ReLU         No (+side)   No              Partial
  Leaky ReLU   No           No              Minimal
  ELU          Mild         Closer          Minimal

------------------------------------------------------------------------

# 🎯 8. Final Practical Advice

✅ Use **ReLU** by default\
✅ Try **Leaky ReLU / ELU** for improvements\
❌ Avoid Sigmoid/Tanh in deep hidden layers

------------------------------------------------------------------------

# 🚀 Core Insight

Activation functions control:

$$
\text{Non-linearity}
$$

$$
\text{Gradient flow}
$$

$$
\text{Optimization stability}
$$

They are fundamental to deep learning success.
