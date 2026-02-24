---
layout: post
title: "04. Long Short Term Memory (LSTM)"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Deep Learning, Foundations]
tags: [Artificial Intelligence, RNN, LSTM]
pin: false
math: true
mermaid: true
---

---
# <b>Long Short Term Memory (LSTM)</b>
---
### <b>Prerequisites</b>
    1. Recurrent Neural Network
<b>Recurrent Neural Network</b> have a problem when training owing to update gradient without insecure like <span style="color:#FFD5D5">vanishing or exploding gradient</span>. This leads to exponential growth or decay of gradients.

---
## <b>What is Long Short Term Memory (LSTM)</b>

### 1. What is Long Short Term Memory (LSTM)?
- A <b>Model that is similar structure but having additional cell state parameters</b></span> composed of preventing gradient update issue while long term sequence.
    
- <b>Structure</b>

    $$
    Input \rightarrow  LSTM \rightarrow  LSTM \rightarrow  LSTM ... \rightarrow  Softmax \rightarrow  Output
    $$

- Can have more secure model with cell state
- it is also having vanishing or exploding gradient problem if sequence is too long. 


### 2. Why use Long Short Term Memory (LSTM)?
     Keep Memory with additional state

Create a <b>cell state highway</b> allowing gradient to flow with minimal
decay.

### 3. How use Recurrent Neural Network(RNN)?
    Hidden State + cell state

![LSTM-inner](/assets/img/develop/LSTM-inner.png)

- $ Forget Gate: f_t = \sigma(W_f x_t + U_f h_{t-1} + b_f) $
- $ Input Gate: i_t = \sigma(W_i x_t + U_i h_{t-1} + b_i) $
- $ Candidate Memory: \tilde{c}_t = \tanh(W_c x_t + U_c h_{t-1} + b_c) $
- $ Cell Update: c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t $
- $ Output Gate: o_t = \sigma(W_o x_t + U_o h_{t-1} + b_o) $
- $ Hidden State: h_t = o_t \odot \tanh(c_t) $

|  Component     |Role
|  ------------- |----------------------------------
|  Forget gate   |Decides what old memory to erase
|  Input gate    |Decides what new info to store
|  Output gate   |Controls exposure of memory
|  Cell state    |Gradient highway

If:

-   $f_t \approx 1$
-   $i_t \approx 0$

Then memory is preserved almost perfectly.

![RNN-process](/assets/img/develop/RNN-process.png)


### 4. What is <span style="color:red">FEW</span> PROBLEM of Recurrent Neural Network(RNN)?
    1. Vanishing Gradient
    2. Exploding Gradient

It is also remain the gradient problem. but it is more better than Vanilla RNN