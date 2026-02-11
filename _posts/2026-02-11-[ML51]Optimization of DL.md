---
layout: post
title: "Optimization"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Optimization]
tags: [Artificial Intelligenc, Optimization, Momentum, AdaGrad, RMSProp, Adam]
pin: false
math: true
mermaid: true
---
# 🚀 Optimization in Deep Learning  
### From SGD → Momentum → AdaGrad → RMSProp → Adam → Second-Order

---

## 🎯 Why Optimization Matters

We want to minimize:

$$
J(\theta)
$$

by iteratively updating parameters:

$$
\theta_{t+1} = \theta_t - \alpha \nabla J(\theta_t)
$$

where:

- $\alpha$ = learning rate  
- $\nabla J(\theta)$ = gradient  

---

# 1️⃣ Problems with SGD

## 📌 SGD Update

$$
\theta_{t+1} = \theta_t - \alpha \nabla J(\theta_t)
$$

---

## ⚠️ Problem 1: Jittering (Slow Progress)

When curvature differs across directions:

- Oscillation in steep direction  
- Slow movement in flat direction  

This relates to the **condition number** of the Hessian:

$$
\kappa(H) = \frac{\lambda_{\max}}{\lambda_{\min}}
$$

Large $\kappa$ ⇒ slow convergence.

---

## ⚠️ Problem 2: Local Minima & Saddle Points

At stationary points:

$$
\nabla J(\theta) = 0
$$

Then:

$$
\theta_{t+1} = \theta_t
$$

No update occurs 😵

In high-dimensional space, saddle points are common.

---

## ⚠️ Problem 3: Noisy Gradient (Mini-batch)

SGD uses approximation:

$$
\nabla J(\theta) \approx \frac{1}{m} \sum_{i=1}^{m} \nabla L_i(\theta)
$$

Small batch → high variance  
Large batch → expensive  

Tradeoff between noise & computation ⚖️

---

# 2️⃣ SGD + Momentum 🏎️

## 💡 Idea

Like physics momentum:

$$
\text{momentum} = \text{mass} \times \text{velocity}
$$

We accumulate gradient history.

---

## 🔄 Update Rule

Velocity:

$$
v_{t+1} = \rho v_t - \alpha \nabla J(\theta_t)
$$

Parameter:

$$
\theta_{t+1} = \theta_t + v_{t+1}
$$

where:

- $\rho \approx 0.9$

---

## ✅ Benefits

- Reduces oscillation  
- Accelerates convergence  
- Escapes shallow saddle regions  

---

# 3️⃣ AdaGrad 📊

## 💡 Idea

Adapt learning rate per parameter.

Accumulate squared gradients:

$$
G_t = \sum_{i=1}^{t} g_i^2
$$

Update:

$$
\theta_{t+1} = \theta_t - \frac{\alpha}{\sqrt{G_t + \epsilon}} g_t
$$

---

## ❌ Weakness

$G_t$ keeps growing:

$$
\frac{\alpha}{\sqrt{G_t}} \to 0
$$

Learning rate shrinks too much 📉

---

# 4️⃣ RMSProp 🔄

Fixes AdaGrad by exponential moving average.

Running average:

$$
s_t = \beta s_{t-1} + (1 - \beta) g_t^2
$$

Update:

$$
\theta_{t+1} = \theta_t - \frac{\alpha}{\sqrt{s_t + \epsilon}} g_t
$$

Typical:

- $\beta = 0.9$

---

# 5️⃣ Adam 🔥

Adaptive Moment Estimation  
Combines Momentum + RMSProp.

---

## 📌 First Moment (Mean)

$$
m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t
$$

## 📌 Second Moment (Variance)

$$
v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2
$$

Bias correction:

$$
\hat{m}_t = \frac{m_t}{1 - \beta_1^t}
$$

$$
\hat{v}_t = \frac{v_t}{1 - \beta_2^t}
$$

Final update:

$$
\theta_{t+1} = \theta_t - \frac{\alpha}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t
$$

---

## 🔢 Typical Hyperparameters

| Parameter | Value |
|-----------|--------|
| $\beta_1$ | 0.9 |
| $\beta_2$ | 0.999 |
| $\alpha$ | 1e-3 |

---

# 6️⃣ First vs Second Order Optimization

---

## 🟢 First-Order Methods

Use gradient only:

$$
\theta_{t+1} = \theta_t - \alpha \nabla J(\theta_t)
$$

Examples:

- SGD
- Momentum
- RMSProp
- Adam

✅ Cheap  
✅ Scalable  

---

## 🔵 Second-Order Methods

Use Hessian:

$$
H = \nabla^2 J(\theta)
$$

Newton update:

$$
\theta^* = \theta_0 - H^{-1} \nabla J(\theta_0)
$$

---

## 📐 Taylor Expansion

$$
J(\theta) \approx J(\theta_0)
+ (\theta - \theta_0)^T \nabla J(\theta_0)
+ \frac{1}{2} (\theta - \theta_0)^T H (\theta - \theta_0)
$$

More accurate curvature modeling 🎯

---

## ❌ Why Rare in Deep Learning?

If dimension = $d$

- Hessian size: $O(d^2)$  
- Inversion cost: $O(d^3)$  

Modern networks:

$$
d \sim 10^7 \text{ to } 10^9
$$

Too expensive 💸

---

# 🧠 Summary Table

| Method | Key Idea | Strength | Weakness |
|--------|----------|----------|----------|
| SGD | Raw gradient | Simple | Slow, oscillation |
| Momentum | Velocity | Faster | Needs tuning |
| AdaGrad | Per-dim LR | Good for sparse | LR shrinks too much |
| RMSProp | EMA scaling | Stable | Extra hyperparameter |
| Adam | Momentum + RMSProp | Default choice | Sometimes overfits |

---

# 🎓 Practical Tips

- 🔥 Start with Adam  
- 🏎️ Try SGD + Momentum for better generalization  
- 📉 Always use LR scheduling  
- 📊 Monitor validation metric, not just loss  

---

# 🎉 Final Insight

Optimization is not just about minimizing loss.

It determines:

- Convergence speed 🚀  
- Stability ⚖️  
- Generalization 📈  

Choosing optimizer = choosing training dynamics.
