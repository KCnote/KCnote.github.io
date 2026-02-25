---
layout: post
title: "07. Attention"
date: 2026-02-25 00:00:00 +0900
author: kang
categories: [Deep Learning, Foundations]
tags: [Artificial Intelligence, RNN, LSTM, GRU, Seq2Seq, Attention]
pin: false
math: true
mermaid: true
---

---
# <b>Attention</b>
---
### <b>Prerequisites</b>
    1. Recurrent Neural Network
    2. Seq2Seq

<b>Recurrent Neural Network</b> have a problem that have <span style="color:#FFD5D5">rough 1:1 aligment</span>. But machine translation breaks these assumptions. input/output lengths differ, different order, a target word may depend on a far-away source word.

<b>Seq2Seq</b> have a problem that have <span style="color:#FFD5D5">bottleneck of encoder</span>. If encoder have long sequences, it is not sufficient. It still suffer from treating extremly long sequences.

---
## <b>What is Attention</b>

### 1. What is Attention?
- A Model that use Seq2Seq model but additional method that <b>take into account hidden states at all input steps with closer attention on more relevant input tokens</b></span>.
    
- <b>Structure</b>

    $$
    Input \rightarrow  ENCODER \rightarrow  DECODER \rightarrow  Softmax \rightarrow  Output
    $$

- Can be free bottleneck of decoder hidden state
- Instead of compressing everything into one vector, Decoder attends to <b>all encoder hidden states</b>.


### 2. Why use Attention?
    All Encoder Hidden States

Each output token focuses on different input.

Attention map visualizes alignment.

### 3. How use Attention?
    1. Query(context)
    2. Value(references)
    3. Key(references)
    4. Attention value

>#### <b>$ Query $</b>: the decoder hidden state at the t
>#### <b>$ Keys $</b>: the encoder hidden state at all times
>#### <b>$ Values $</b>: the encoder hidden state at all times
>#### <b>$ Attention Value $</b>: the result of Attention(Q,K,V)
$$
\text{Attention}(Q, K, V) = \text{weighted sum of } V
$$

![Attention-Cal](/assets/img/develop/Attention-Cal.png)

>## Step 1: Define Hidden States

<b>Encoder hidden states</b>: $ h_1, h_2, ..., h_T \in \mathbb{R}^d $,  
<b>Decoder state at time t</b>: $ s_t \in \mathbb{R}^d $

### "Decoder state at time t ($s_t$)" is <span style="color:cyan">Query</span>

>## Step 2: Compute Attention Scores

Score between decoder state and each encoder state: $ e_{t,i} = s_t^T h_i $

Vector form:

$$
e_t = 
\begin{bmatrix}
s_t^T h_1 \\
s_t^T h_2 \\
\vdots \\
s_t^T h_T
\end{bmatrix}
\in \mathbb{R}^T
$$

### "Each encoder state ($h_i$)" is <span style="color:cyan">Key</span>

>## Step 3: Softmax → Attention Coefficients

$$
\alpha_{t,i} = \frac{\exp(e_{t,i})}{\sum_{j=1}^{T} \exp(e_{t,j})}
$$

where: $ \sum_{i=1}^{T} \alpha_{t,i} = 1 $

>## Step 4: Compute Attention Vector

Weighted sum:

$$
a_t = \sum_{i=1}^{T} \alpha_{t,i} h_i
$$

This is convex combination of encoder states.

### "Each encoder state ($h_i$)" is <span style="color:cyan">Value</span>
### "Each encoder state ($a_t$)" is <span style="color:cyan">Attention Value</span>

>## Step 5: Combine with Decoder State

Concatenate:

$$
\tilde{s}_t = \tanh(W_c [a_t ; s_t])
$$

where: 
$[a_t ; s_t]$ is concatenated vector

Output probability:

$$
P(y_t | y_{<t}, x) = \text{softmax}(W_o \tilde{s}_t)
$$

#### <b> Attention Process </b>
![Attention-Structure](/assets/img/develop/Attention-Structure.png)

#### <b> Relation Token from attention score </b>
![Attention-Relation](/assets/img/develop/Attention-Relation.png)

|  Component          |Meaning
|  ------------------ |--------------------
|  Query              |Decoder state
|  Key                |Encoder states
|  Value              |Encoder states
|  Attention weight   |Similarity measure
|  Output             |Weighted average

