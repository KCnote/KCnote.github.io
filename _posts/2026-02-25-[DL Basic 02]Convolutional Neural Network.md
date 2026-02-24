---
layout: post
title: "02. Convolutional Neural Network"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Deep Learning, Foundations]
tags: [Artificial Intelligence, CNN]
pin: false
math: true
mermaid: true
---

---
# <b>Convolutional Neural Network</b>
---
### <b>Prerequisites</b>
    1. Multi-layer Perceptron
<b>Multi-layer Perceptron</b> $ f(x) = \sigma(Wx) $ have a simple model that that <span style="color:#FFD5D5">have a parameters explosion and just using flatten data</span>. 

---
## <b>What is Convolutional Neural Network(CNN)</b>

### 1. What is Convolutional Neural Network(CNN)?
- A <span style="color:blue"><b>Model that use learnable filters</b></span> composed of multiple channel with user-defined kernel size.
    
- <b>Structure</b>

    $$
    Input \rightarrow  Filter \rightarrow  Activation \rightarrow  Filter ... \rightarrow  Softmax \rightarrow  Output
    $$

- Can have good performance with smaller parameters than FC parameters
- Sometimes it is interpretable model why this is work. 

### 2. Why use Convolutional Neural Network(CNN)?
     Spatial Locality

#### <b>Each filter looks at NEARBY PIXELS ONLY</b>
instead of connecting everything like FC, looking at local patches

![CNN-Spatial-Locality](/assets/img/develop/CNN-Spatial-Locality.png)


    Positional Invariance
#### <b>Same filters are applied to ALL LOCATION in the image</b>
Reuse the same filter across space.

![CNN-Positional-Invariance](/assets/img/develop/CNN-Positional-Invariance.png)


| Aspect | FC | Conv |
|------|----|------|
| Connectivity | Global | Local |
| Parameters | Huge | Small |
| Spatial info | ❌ | ✅ |
| Weight sharing | ❌ | ✅ |

### 3. How use Convolutional Neural Network(CNN)?
    Convolution
    
#### 1. 2D Discrete Convolution
$$
y(i,j) = \sum_{m=0}^{K_h-1} \sum_{n=0}^{K_w-1} x(i+m,; j+n), w(m,n)
$$

- $x$ : Input
- $w$ : Kernel
- $y$ : Feature map
- $K_h, K_w$ : Height, Width
- $(i,j)$ : coordinate

#### 2. Multi-Channel CNN Convolution

$$
y(i,j) = \sum_{c=1}^{C} \sum_{m=0}^{K_h-1} \sum_{n=0}^{K_w-1}
x_c(i+m,; j+n), w_c(m,n)
$$

- $C$ : Number of channels (RGB=3, Feature map=64 and so on)

![CNN-Cal](/assets/img/develop/CNN-Cal.gif)

![CNN-process](/assets/img/develop/CNN-process.png)



### 4. What kind of hyperparameters Convolutional Neural Network(CNN) does have?
    1. Stride
    2. Padding
    3. Filter Size
    4. Number of Filters

- $Stride(S)$: How far the filter moves
- $Padding(P)$: Additional extra space
    - Preserve spatial size
    - Allow deeper networks

- $Filter Size(F)$: How many size convolution calculate on spatial locality
- $Number of Filters(K)$: How many various information it made
$$
Parameters:
K(F^2C + 1)
$$

### 5. How to work Convolutional Neural Network(CNN) on Features?

Assumed to have mulit-level layers we have, the CNN can be distinguished with low, mid, high level features.

![CNN-level](/assets/img/develop/CNN-level.png)

Hierarchical Features with Convolutional Layer Level:

| Level | Learns |
|------|-------|
| Low | Edges, colors |
| Mid | Corners, textures |
| High | Objects, faces |


### 6. Another component of Convolutional Neural Network(CNN)?
    Pooling

1. MaxPooling
2. AveragePooling

![CNN-Pooling](/assets/img/develop/CNN-Pooling.png)

<b>It is <span style="color:red">NOT learnable</span> parameters, it is just for downsampling/reduce computation or sometimes improving robustness</b>




