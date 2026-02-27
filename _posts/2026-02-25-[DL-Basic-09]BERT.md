---
layout: post
title: "09. BERT"
date: 2026-02-25 00:00:00 +0900
author: kang
categories: [Deep Learning, Foundations]
tags: [Artificial Intelligence, RNN, LSTM, GRU, Seq2Seq, Attention, Transformer, BERT]
pin: false
math: true
mermaid: true
---

---
# <b>BERT</b>
---
### <b>Prerequisites</b>
    1. Transformer

<b>Transformer</b> is very important turning point of model with good performance. But at the moment, the focus of task is just translation.

---
## <b>What is BERT</b>

### 1. What is BERT?
- A Model that <b>use encoder-only of transformer model</b></span>. The main idea is bidirectional method can adapt the new task like self-supervised learning.
    
- <b>Structure</b>

    $$
    Input \rightarrow  ENCODER \rightarrow  Softmax \rightarrow  Output
    $$

- From tasks of Masked Language Model(MLM), Next Sentence Prediction(NSP), it works for training.
- With bidirectional, word embedding is not fixed embedding and to be contextualizal embedding data.


### 2. Why use BERT?
    1. Transformer Encoder
    2. Token + Segment + Position Embedding
    3. Self-Supervised Learning

<b> Encoder: Bidirectional contextualization </b>

Thanks to Token, OOV issue is almost solved and even though there're typo issue, it will <b>be tokenalization of almost word</b> or use UNK token that can guess contextual what kind of a word. 

### 3. How use Transformer?
    1. Token + Segment + Position Embedding 
    2. Transformer Encoder

>### $Input$
- $Token\; embedding$: a pre-trained word embedding (WordPiece)
    - With special token: `[CLS]`, `[SEP]`
        - `[CLS]`: Classificaiton toekn, put always at the beginning. Final hidden state for this token is used as the aggregate sequence representation for classification tasks
        - `[SEP]`: Seperator token, used to mark the end of a sentence.
- $Segment\; embedding$: A learned embedding indicating which sentence each token belongs to
- $Position\; embedding$: a learned embedding for each position

![BERT-Input](/assets/img/develop/BERT-Input.png)

>### $Transformer\; Encoder$

BERT uses the Transformer encoder to learn bidirectional language representations that function as contextual embeddings.
And the main idea of how to make contextualizaion functions. With the MLM task, the BERT learn embedding and contextualization weights. It can use not only by itself but also fine tunning with another models. 

![BERT-pretraining](/assets/img/develop/BERT-pretraining.png)

The model is not difference via transformer. But the contribution of BERT is that this perspective change the NLP paradigm. Especially MLM task is very efficient and easily collect the data from anywhere without human annotation.
In other word, <b><span style="color:cyan">Not a new architecture BUT a new training framwork with transformer</span></b>.

