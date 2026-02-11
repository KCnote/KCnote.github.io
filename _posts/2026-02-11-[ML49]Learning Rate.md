---
layout: post
title: "Learning Rate"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Optimization]
tags: [Artificial Intelligenc, Optimization, Learning Rate]
pin: false
math: true
mermaid: true
---
# 🚀 Learning Rate & Scheduling --- Complete Notes

------------------------------------------------------------------------

# 1️⃣ How to Choose a Learning Rate

All optimizers (SGD, Momentum, AdaGrad, RMSProp, Adam) require a
**learning rate** $\alpha$ as a hyperparameter.

Update rule:

$$
\theta_{t+1} = \theta_t - \alpha_t \nabla_\theta \mathcal{L}(\theta_t)
$$

### Too High Learning Rate

If $\alpha$ is too large:

$$
\theta_{t+1} \text{ overshoots the minimum}
$$

Loss may **explode**.

------------------------------------------------------------------------

### Too Low Learning Rate

If $\alpha$ is too small:

$$
\theta_{t+1} \approx \theta_t
$$

Training becomes extremely slow.

------------------------------------------------------------------------

# 2️⃣ Why Use Learning Rate Decay?

Initially:

-   Large $\alpha$ helps capture global structure.

Near optimum:

-   Smaller $\alpha$ needed for fine convergence.

------------------------------------------------------------------------

# 3️⃣ Step Decay

Reduce learning rate at fixed milestones.

Example:

$$
\alpha \leftarrow 0.1 \alpha
$$

at 50% and 75% of training.

------------------------------------------------------------------------

# 4️⃣ Cosine Decay

$$
\alpha_t = \frac{1}{2} \alpha_0 \left(1 + \cos\left(\frac{\pi t}{T}\right)\right)
$$

Where:

-   $\alpha_0$ = initial learning rate\
-   $t$ = current epoch\
-   $T$ = total epochs

------------------------------------------------------------------------

# 5️⃣ Linear Decay

$$
\alpha_t = \alpha_0 \left(1 - \frac{t}{T}\right)
$$

------------------------------------------------------------------------

# 6️⃣ Inverse Square Root Decay

$$
\alpha_t = \frac{\alpha_0}{\sqrt{t}}
$$

Common in Transformer training.

------------------------------------------------------------------------

# 7️⃣ Warmup Strategy

Large initial $\alpha$ can explode training.

Warmup gradually increases learning rate:

$$
\alpha_t = \alpha_0 \frac{t}{T_{warmup}}
$$

for $t \le T_{warmup}$

Then normal decay begins.

------------------------------------------------------------------------

# 8️⃣ Summary

|  Schedule      | Formula
|  --------------| ----------------------------------------
|  Step          | $\alpha \leftarrow 0.1 \alpha$
|  Cosine        | $\frac{1}{2}\alpha_0(1+\cos(\pi t/T))$
|  Linear        | $\alpha_0(1 - t/T)$
|  Inverse Sqrt  | $\alpha_0 / \sqrt{t}$
|  Warmup        | $\alpha_0 t/T_{warmup}$

------------------------------------------------------------------------

✅ Practical Rule:

-   Start large
-   Decay gradually
-   Use warmup for deep networks
