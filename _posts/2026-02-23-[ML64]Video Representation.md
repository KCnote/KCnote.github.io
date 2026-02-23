---
layout: post
title: "Video Representation & Fusion"
date: 2026-02-23 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligenc, CNN]
pin: false
math: true
mermaid: true
---

# Video Representation & Fusion (Detailed Technical Notes)

This document explains in detail:

- Multi-frame modeling
- Early fusion
- Late fusion
- Temporal pooling
- Optical flow (with full derivation)
- ConvLSTM (full equations)
- Two-stream networks
- Convolutional two-stream fusion
- Hidden two-stream variants

We use the following notation:

Let a video clip contain:

- T frames
- Each frame: $I_t \in \mathbb{R}^{H 	imes W 	imes 3}$
- 2D CNN backbone: $f_\theta$
- Classifier: $g_\phi$
- Number of classes: C

Final prediction:

$$
\hat{p} = \mathrm{softmax}(z)
$$

Loss:

$$
\mathcal{L} = - \sum_{c=1}^{C} y_c \log \hat{p}_c
$$

---------------------------------------------------------------------

# 1. Multi-Frame Representation

Single-frame models capture appearance only.

To incorporate temporal information, we use multiple frames:

$$
x_t = f_\theta(I_t), \quad x_t \in \mathbb{R}^d
$$

Clip-level representation:

$$
x_{clip} = \mathrm{Agg}(x_1, x_2, \dots, x_T)
$$

Aggregation can be pooling, RNN, ConvLSTM, etc.

---------------------------------------------------------------------

# 2. Early Fusion

Frames are fused at the input level.

Channel stacking:

$$
X = \mathrm{concat}_{channel}(I_1, I_2, \dots, I_T)
$$

Then:

$$
h = f_\theta(X)
$$

Input dimension becomes:

$$
H \times W \times (3T)
$$

First convolution parameter count:

If kernel size is k x k and output channels = C_out,

$$
\#W = k^2 \cdot (3T) \cdot C_{out}
$$

Compared to single-frame:

$$
k^2 \cdot 3 \cdot C_{out}
$$

Thus parameters increase by factor T.

Advantage:
- Early interaction between frames

Disadvantage:
- Parameter explosion
- Weak temporal order modeling

---------------------------------------------------------------------

# 3. Late Fusion

Each frame processed independently:

$$
x_t = f_\theta(I_t)
$$

Feature-level fusion:

$$
x_{clip} = \frac{1}{T} \sum_{t=1}^{T} x_t
$$

Logit-level fusion:

$$
z_t = g_\phi(x_t)
$$

$$
z = \frac{1}{T} \sum_{t=1}^{T} z_t
$$

Advantage:
- Reuse pretrained 2D CNN
- Stable training

Disadvantage:
- Weak temporal interaction

---------------------------------------------------------------------

# 4. Temporal Pooling

Average pooling:

$$
x = \frac{1}{T} \sum_{t=1}^{T} x_t
$$

Max pooling:

$$
x_i = \max_t (x_t)_i
$$

Weighted pooling (attention-like):

Score:

$$
s_t = w^T x_t
$$

Softmax normalization:

$$
\alpha_t = \frac{e^{s_t}}{\sum_{j=1}^{T} e^{s_j}}
$$

Aggregation:

$$
x = \sum_{t=1}^{T} \alpha_t x_t
$$

---------------------------------------------------------------------

# 5. Optical Flow (Full Derivation)

Brightness constancy assumption:

$$
I(x, y, t) = I(x + u, y + v, t + 1)
$$

Using first-order Taylor expansion:

$$
I(x + u, y + v, t + 1)
\approx I(x, y, t)
+ I_x u + I_y v + I_t
$$

Substitute into brightness constancy:

$$
I(x, y, t) =
I(x, y, t) + I_x u + I_y v + I_t
$$

Cancel terms:

$$
I_x u + I_y v + I_t = 0
$$

This is the Optical Flow Constraint Equation.

Since we have 1 equation and 2 unknowns (u, v),
regularization is required.

Example energy formulation:

$$
E(u,v) =
\iint (I_x u + I_y v + I_t)^2 dx dy
+ \lambda \iint (||\nabla u||^2 + ||\nabla v||^2) dx dy
$$

---------------------------------------------------------------------

# 6. ConvLSTM (Full Equations)

Standard LSTM:

$$
i_t = \sigma(W_{xi} x_t + W_{hi} h_{t-1} + b_i)
$$

$$
f_t = \sigma(W_{xf} x_t + W_{hf} h_{t-1} + b_f)
$$

$$
o_t = \sigma(W_{xo} x_t + W_{ho} h_{t-1} + b_o)
$$

$$
\tilde{c}_t = \tanh(W_{xc} x_t + W_{hc} h_{t-1} + b_c)
$$

$$
c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t
$$

$$
h_t = o_t \odot \tanh(c_t)
$$

ConvLSTM replaces matrix multiplication with convolution.

Inputs:

$$
X_t \in \mathbb{R}^{H \times W \times C_x}
$$

Hidden state:

$$
H_t \in \mathbb{R}^{H \times W \times C_h}
$$

ConvLSTM equations:

$$
I_t = \sigma(W_{xI} * X_t + W_{hI} * H_{t-1} + b_I)
$$

$$
F_t = \sigma(W_{xF} * X_t + W_{hF} * H_{t-1} + b_F)
$$

$$
O_t = \sigma(W_{xO} * X_t + W_{hO} * H_{t-1} + b_O)
$$

$$
\tilde{C}_t = \tanh(W_{xC} * X_t + W_{hC} * H_{t-1} + b_C)
$$

$$
C_t = F_t \odot C_{t-1} + I_t \odot \tilde{C}_t
$$

$$
H_t = O_t \odot \tanh(C_t)
$$

---------------------------------------------------------------------

# 7. Two-Stream Network

Two separate streams:

Spatial stream (RGB):
$$
x^{(s)} = f_{\theta_s}(RGB)
$$

Temporal stream (Flow):
$$
x^{(t)} = f_{\theta_t}(Flow)
$$

Fusion:

$$
z = \mathrm{Fuse}(x^{(s)}, x^{(t)})
$$

Limitations:

1. Flow computation cost
2. Flow sensitivity to noise
3. Harder end-to-end optimization
4. Camera motion interference

---------------------------------------------------------------------

# 8. Convolutional Two-Stream Fusion

Feature maps:

$$
F^{(s)} \in \mathbb{R}^{h \times w \times c_s}
$$

$$
F^{(t)} \in \mathbb{R}^{h \times w \times c_t}
$$

Concatenation:

$$
F = \mathrm{concat}(F^{(s)}, F^{(t)})
$$

1x1 convolution mixing:

$$
F_{mix} = W_{1x1} * F
$$

Gated fusion:

$$
G = \sigma(\mathrm{Conv}([F^{(s)}, F^{(t)}]))
$$

$$
F = G \odot F^{(s)} + (1-G) \odot F^{(t)}
$$

---------------------------------------------------------------------

# 9. Hidden Two-Stream

Instead of explicit optical flow, motion is learned implicitly.

Temporal difference feature:

$$
\Delta x_t = x_t - x_{t-1}
$$

Two representations:

Appearance:
$$
x_t
$$

Motion-like latent:
$$
\Delta x_t
$$

Advantages:
- No explicit flow computation
- Fully end-to-end trainable

Disadvantages:
- Motion signal may be weaker than true flow
