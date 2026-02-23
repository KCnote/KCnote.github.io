---
layout: post
title: "Video Transformers"
date: 2026-02-23 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligenc, Transformer]
pin: false
math: true
mermaid: true

---

# Video Transformers

This note covers three important families of transformer-based video models:

1) ViViT (Video Vision Transformer) — Model 1 to Model 4  
2) TimeSFormer (Time-Space Transformer)  
3) Multiscale Vision Transformers (MViT / MViTv2 style ideas)

Throughout, I will **avoid fragile inline LaTeX** and use **block equations** only.

---------------------------------------------------------------------

# 0) Problem Setup and Notation

A video clip is a tensor:

$$
X \in \mathbb{R}^{T \times H \times W \times C}
$$

Commonly:

- C = 3 for RGB
- T = number of frames
- H, W = spatial resolution

Patch size:

$$
P \times P
$$

Spatial patches per frame:

$$
N = \frac{H W}{P^2}
$$

Tokens per clip (if we tokenize each frame into patches):

$$
L = T \cdot N
$$

Transformer hidden dimension:

$$
D
$$

We represent tokens as a sequence:

$$
Z \in \mathbb{R}^{L \times D}
$$

Standard attention (single head form):

$$
Q = Z W_Q
$$

$$
K = Z W_K
$$

$$
V = Z W_V
$$

$$
\mathrm{Attn}(Z) =
\mathrm{softmax}
\left(
\frac{Q K^T}{\sqrt{D}}
\right)
V
$$

Compute cost is dominated by:

$$
\mathcal{O}(L^2)
$$

So **how we define L** and **how we factor attention** matters a lot.

---------------------------------------------------------------------

# 1) ViViT (Video Vision Transformer): Models 1–4

ViViT is a design space for adapting ViT to video.

The core challenge:

- Video tokens explode: L = T*N can be huge
- Full attention over L tokens is expensive
- Need to model both appearance and motion / temporal dynamics

Below, “Model 1–4” are best understood as **different factorization and aggregation strategies**.

---------------------------------------------------------------------

## 1.1 ViViT Tokenization (shared start)

Per-frame patch embedding:

Each frame at time t is:

$$
X_t \in \mathbb{R}^{H \times W \times C}
$$

Split into N patches, flatten each patch:

$$
x_{t,i} \in \mathbb{R}^{P^2 \cdot C}
$$

Linear embedding:

$$
z_{t,i} = E x_{t,i}
$$

Collect all tokens:

$$
Z = \{ z_{t,i} \}
$$

Video positional encoding usually has *two components*:

- Spatial position within a frame
- Temporal position across frames

A common additive form:

$$
z_{t,i} \leftarrow z_{t,i} + p^{space}_i + p^{time}_t
$$

---------------------------------------------------------------------

## 1.2 ViViT Model 1: Spatio-Temporal Joint Attention (Full Attention)

### Idea

Treat all tokens across space and time as one long sequence and apply standard transformer blocks.

Sequence length:

$$
L = T \cdot N
$$

Attention complexity:

$$
\mathcal{O}\left( (T N)^2 \right)
$$

### Why it works

- Directly models arbitrary interactions: any patch can attend to any other patch in any frame
- Rich temporal-spatial reasoning capacity

### Why it is expensive

If H=W=224, P=16:

$$
N = \frac{224 \cdot 224}{16^2} = 196
$$

If T=32:

$$
L = 32 \cdot 196 = 6272
$$

Then attention matrix is:

$$
6272 \times 6272
$$

That is huge in memory and compute.

### When Model 1 makes sense

- Short clips
- Smaller resolution
- Large compute budget
- You want maximum flexibility and can afford it

---------------------------------------------------------------------

## 1.3 ViViT Model 2: Factorized Space-Time Attention (Two-Stage Attention)

### Idea

Replace joint attention with a factorization:

1) Spatial attention within each frame (independent across t)  
2) Temporal attention across frames (for corresponding spatial tokens or for aggregated per-frame tokens)

This reduces cost by avoiding a full (TN) x (TN) matrix.

### Step A: Spatial attention per frame

For each t, run attention over N tokens:

$$
Z_t \in \mathbb{R}^{N \times D}
$$

Cost per frame:

$$
\mathcal{O}(N^2)
$$

For T frames:

$$
\mathcal{O}(T \cdot N^2)
$$

### Step B: Temporal attention

There are two common flavors.

#### Flavor B1: Token-wise temporal attention

For each spatial index i, attend across time:

$$
Z_{:,i} \in \mathbb{R}^{T \times D}
$$

Cost per spatial location:

$$
\mathcal{O}(T^2)
$$

For all N spatial positions:

$$
\mathcal{O}(N \cdot T^2)
$$

Total Model 2 cost:

$$
\mathcal{O}(T N^2 + N T^2)
$$

Compare to Model 1:

$$
\mathcal{O}(T^2 N^2)
$$

Model 2 is dramatically cheaper when T and N are not tiny.

#### Flavor B2: Temporal attention on per-frame pooled tokens

Instead of per-location temporal attention, summarize each frame with a token and attend over T summary tokens.

Per-frame summary token (class token or pooling):

$$
s_t = \mathrm{Pool}(Z_t)
$$

Sequence:

$$
S \in \mathbb{R}^{T \times D}
$$

Temporal attention cost:

$$
\mathcal{O}(T^2)
$$

Total is then mostly spatial:

$$
\mathcal{O}(T N^2 + T^2)
$$

### Pros

- Much cheaper than Model 1
- Still captures temporal dynamics

### Cons

- Factorization imposes structural constraints
- Some interactions (like patch i at time t attending to patch j at time t') are only indirectly modeled

---------------------------------------------------------------------

## 1.4 ViViT Model 3: Factorized Encoder with Temporal Tokenization / Pooling (Aggressive Temporal Compression)

### Idea

Reduce temporal length early.

Common strategies:

1) Temporal pooling or striding on frame embeddings
2) Tubelet embedding (3D patch) to reduce tokens
3) Use temporal attention only on a small set of tokens

### Tubelet embedding (common in video ViT designs)

Instead of 2D patches, use a 3D “tubelet” spanning t frames:

Tubelet size:

$$
\tau \times P \times P
$$

Now temporal axis is downsampled by factor \tau:

New temporal length:

$$
T' = \frac{T}{\tau}
$$

Tokens per clip:

$$
L' = T' \cdot N
$$

Attention cost becomes:

$$
\mathcal{O}\left( (T' N)^2 \right)
$$

which is smaller than Model 1 if T' << T.

### Pros

- Significant compute reduction
- Strong for longer clips

### Cons

- If \tau is large, you may lose fine motion details
- Temporal aliasing can occur if content changes quickly between frames

---------------------------------------------------------------------

## 1.5 ViViT Model 4: Factorized Attention + Token Pooling / Hierarchical Video Transformer

### Idea

Build a **hierarchical** representation across layers (like CNN pyramids) so the model processes:

- High resolution, many tokens early
- Lower resolution, fewer tokens later

This is aligned with the “multiscale” philosophy (also used in MViT).

Common mechanisms:

1) Patch merging or pooling in space  
2) Temporal pooling / merging across frames  
3) Attention operating on progressively shorter sequences

### Patch merging (concept)

Merge 2x2 spatial neighboring tokens into one:

Spatial resolution reduces by 2, channels increase (or projection changes).

If spatial tokens N correspond to H/P by W/P grid:

After merging:

$$
N' = \frac{N}{4}
$$

Similarly, temporal merging could reduce T:

$$
T' = \frac{T}{2}
$$

Total tokens reduce:

$$
L' = T' \cdot N'
$$

### Why this matters

Attention cost scales with L^2, so token reduction is extremely powerful.

### Pros

- Scales to longer clips and higher resolution
- Learns coarse-to-fine temporal-spatial representations

### Cons

- Requires careful design of merging rules
- Some fine details may be lost at late stages

---------------------------------------------------------------------

# 2) TimeSFormer (Time-Space Transformer)

TimeSFormer is closely related to ViViT-style factorization, but often explained with a simple, clean idea:

- Separate attention into **temporal attention** and **spatial attention**
- Alternate or compose them in each layer

---------------------------------------------------------------------

## 2.1 TimeSFormer Factorized Attention

Let tokens be indexed by (t, i) where:

- t: time index
- i: spatial patch index

### Temporal attention (per spatial position)

For each i, attend over t:

$$
Z_{:,i} \in \mathbb{R}^{T \times D}
$$

Temporal attention output:

$$
\hat{Z}_{:,i} = \mathrm{Attn}(Z_{:,i})
$$

### Spatial attention (per time step)

For each t, attend over i:

$$
Z_{t,:} \in \mathbb{R}^{N \times D}
$$

Spatial attention output:

$$
\hat{Z}_{t,:} = \mathrm{Attn}(Z_{t,:})
$$

### Layer composition

A common pattern is:

1) Temporal attention
2) Spatial attention
3) MLP

Each with residual connections and LayerNorm.

---------------------------------------------------------------------

## 2.2 Complexity Comparison (Intuition)

Full joint attention:

$$
\mathcal{O}(T^2 N^2)
$$

Factorized temporal + spatial:

Temporal part:

$$
\mathcal{O}(N T^2)
$$

Spatial part:

$$
\mathcal{O}(T N^2)
$$

Total:

$$
\mathcal{O}(N T^2 + T N^2)
$$

This is the same big picture as ViViT Model 2, explained in a time-first manner.

---------------------------------------------------------------------

## 2.3 Practical Strengths and Weaknesses

Strengths:

- Much more scalable than full attention
- Natural separation of motion modeling (temporal) and appearance (spatial)
- Strong performance on action recognition benchmarks

Weaknesses:

- Still can be heavy if T and N are both large
- Temporal attention per location can overfit or be noisy if motion is subtle
- Camera motion can complicate temporal attention patterns (background changes dominate)

---------------------------------------------------------------------

# 3) Multiscale Vision Transformers (MViT-style)

Key idea:

- Use hierarchical, multiscale token representations
- Reduce tokens progressively (space and time)
- Use attention with pooling / striding so the model scales like CNN pyramids

This is one of the most important directions for scalable video transformers.

---------------------------------------------------------------------

## 3.1 Why Multiscale for Video?

Video has two scaling dimensions:

- Spatial resolution H x W
- Temporal resolution T

If you keep all tokens everywhere, attention explodes.

Multiscale strategy:

- Early layers: high resolution, short receptive field
- Later layers: lower resolution, larger receptive field (long-range context)

This mimics CNN inductive bias while staying transformer-based.

---------------------------------------------------------------------

## 3.2 Pooling Attention (Concept)

Instead of attending with Q,K,V all at full length, we can downsample K and V:

Let original tokens length be L.

We compute:

$$
Q \in \mathbb{R}^{L \times D}
$$

but pool K and V to length L_p:

$$
K_p \in \mathbb{R}^{L_p \times D}
$$

$$
V_p \in \mathbb{R}^{L_p \times D}
$$

Now attention uses:

$$
\mathrm{softmax}
\left(
\frac{Q K_p^T}{\sqrt{D}}
\right)
V_p
$$

This reduces cost from:

$$
\mathcal{O}(L^2)
$$

to:

$$
\mathcal{O}(L \cdot L_p)
$$

If L_p << L, you save a lot.

Pooling can be applied:

- spatial pooling
- temporal pooling
- both (tubelet pooling)

---------------------------------------------------------------------

## 3.3 Hierarchical Stages (Video Pyramid)

A typical multiscale transformer uses stages like:

Stage 1:
- high spatial tokens
- high temporal tokens

Stage 2:
- reduce spatial and/or temporal

Stage 3:
- reduce more

Stage 4:
- very coarse tokens, global reasoning

At each stage you can increase channel dimension D while reducing L.

This resembles ResNet-style stage scaling.

---------------------------------------------------------------------

## 3.4 Inductive Bias vs Flexibility

Multiscale introduces inductive bias:

- locality (early)
- hierarchy (progressive abstraction)

This is often beneficial for data efficiency and generalization, especially on moderate-scale datasets.

The tradeoff:

- you restrict the model relative to full joint attention
- but you gain scalability and often real-world robustness

---------------------------------------------------------------------

# 4) Practical Design Choices Cheat Sheet

## 4.1 When to prefer which

- Full spatio-temporal (ViViT Model 1):
  - best when T and N are small or compute is huge
  - most flexible interactions

- Factorized attention (TimeSFormer / ViViT Model 2):
  - strong baseline
  - scales much better than full attention
  - good when you want a clean, interpretable design

- Tubelet / temporal compression (ViViT Model 3):
  - best when clip length is long
  - watch out for motion detail loss

- Hierarchical / multiscale (ViViT Model 4 / MViT):
  - best for scalability (high-res, long clips)
  - tends to generalize well
  - more engineering complexity

---------------------------------------------------------------------

# 5) Summary Table

| Family | Core Mechanism | Token Length Control | Main Benefit | Main Risk |
| --- | --- | --- | --- | --- |
| ViViT Model 1 | Joint space-time attention | None | Max flexibility | Very expensive |
| ViViT Model 2 | Factorized space then time | Attention factorization | Much cheaper | Restricted interactions |
| ViViT Model 3 | Tubelets / temporal compression | Reduce T early | Scales to long clips | Motion detail loss |
| ViViT Model 4 | Hierarchical pooling/merging | Reduce T and N over stages | Best scalability | Design complexity |
| TimeSFormer | Separate temporal + spatial attention | Factorization | Clean + scalable | Still heavy for big T,N |
| MViT | Multiscale + pooled attention | Hierarchy + pooling | CNN-like scaling | Needs careful pooling design |
