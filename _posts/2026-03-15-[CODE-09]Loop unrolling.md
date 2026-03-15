---
layout: post
title: "Loop Unrolling in 5x5 Convolution: Why It Matters"
date: 2026-03-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Loop Unrolling</b>
---
### <b>Prerequites</b>
    C++
    Convolution operation

---
## <b>What is Loop Unrolling</b>

In image processing, **convolution** is one of the most fundamental operations.

Whether it is smoothing, sharpening, edge detection, or feature extraction, many operations are built on top of convolution kernels.

Among them, a **5×5 convolution** is a common example because it is still small enough to optimize aggressively while being large enough to show meaningful performance differences.

One classic low-level optimization technique here is **loop unrolling**.

**Loop unrolling** is an optimization technique where loop iterations are expanded manually.

Instead of writing:

```cpp
for(int i = 0; i < 5; ++i)
{
    sum += a[i] * b[i];
}
```

we expand it as:

```cpp
sum += a[0] * b[0];
sum += a[1] * b[1];
sum += a[2] * b[2];
sum += a[3] * b[3];
sum += a[4] * b[4];
```

The goal is to reduce:

- loop counter operations
- branch instructions
- repeated index calculations
- address computations

### 1. How to deal with Loop unrolling on 5×5 Convolution Formula

For an image `src` and kernel `K`:

$$
dst(x,y) = \sum_{j=0}^{4} \sum_{i=0}^{4} src(x+i-2, y+j-2) \cdot K(j,i)
$$

Each pixel computation performs:

- **25 multiplications**
- **24 additions**

Even small overhead reductions can matter when applied to millions of pixels.

#### Basic 5×5 Convolution Implementation

```cpp
void Convolution5x5_Basic(
    const uint8_t* src,
    uint8_t* dst,
    int width,
    int height,
    int stride,
    const float kernel[5][5])
{
    for (int y = 2; y < height - 2; ++y)
    {
        for (int x = 2; x < width - 2; ++x)
        {
            float sum = 0.0f;

            for (int ky = 0; ky < 5; ++ky)
            {
                for (int kx = 0; kx < 5; ++kx)
                {
                    int ix = x + kx - 2;
                    int iy = y + ky - 2;

                    sum += src[iy * stride + ix] * kernel[ky][kx];
                }
            }

            if (sum < 0) sum = 0;
            if (sum > 255) sum = 255;

            dst[y * stride + x] = (uint8_t)sum;
        }
    }
}
```

#### Characteristics

Pros

- simple
- readable
- flexible

<span style="color:red"><b>Cons</b></span>

- loop overhead
- repeated indexing
- additional arithmetic operations

#### Fully Unrolled 5×5 Convolution

```cpp
void Convolution5x5_Unrolled(
    const uint8_t* src,
    uint8_t* dst,
    int width,
    int height,
    int stride,
    const float kernel[5][5])
{
    for (int y = 2; y < height - 2; ++y)
    {
        for (int x = 2; x < width - 2; ++x)
        {
            const uint8_t* r0 = src + (y-2)*stride + (x-2);
            const uint8_t* r1 = src + (y-1)*stride + (x-2);
            const uint8_t* r2 = src + (y)*stride   + (x-2);
            const uint8_t* r3 = src + (y+1)*stride + (x-2);
            const uint8_t* r4 = src + (y+2)*stride + (x-2);

            float sum = 0;

            sum += r0[0]*kernel[0][0]; 
            sum += r0[1]*kernel[0][1];
            sum += r0[2]*kernel[0][2]; 
            sum += r0[3]*kernel[0][3];
            sum += r0[4]*kernel[0][4];

            sum += r1[0]*kernel[1][0]; 
            sum += r1[1]*kernel[1][1];
            sum += r1[2]*kernel[1][2]; 
            sum += r1[3]*kernel[1][3];
            sum += r1[4]*kernel[1][4];

            sum += r2[0]*kernel[2][0]; 
            sum += r2[1]*kernel[2][1];
            sum += r2[2]*kernel[2][2]; 
            sum += r2[3]*kernel[2][3];
            sum += r2[4]*kernel[2][4];

            sum += r3[0]*kernel[3][0]; 
            sum += r3[1]*kernel[3][1];
            sum += r3[2]*kernel[3][2]; 
            sum += r3[3]*kernel[3][3];
            sum += r3[4]*kernel[3][4];

            sum += r4[0]*kernel[4][0]; 
            sum += r4[1]*kernel[4][1];
            sum += r4[2]*kernel[4][2]; 
            sum += r4[3]*kernel[4][3];
            sum += r4[4]*kernel[4][4];

            if (sum < 0) 
                sum = 0;
            
            if (sum > 255) 
                sum = 255;

            dst[y * stride + x] = (uint8_t)sum;
        }
    }
}
```

### 2. Why Loop Unrolling Can Be Faster

The improvement does **not** come from reducing the number of multiplications.

Both implementations still perform:

- 25 multiplications
- 24 additions

The benefit comes from reducing surrounding overhead.

#### 2-1. Reduced Loop Control

The nested loops remove:

- counter updates
- branch checks

#### 2-2. Reduced Index Arithmetic

Instead of repeatedly computing:

```
iy * stride + ix
```

row pointers are calculated once.

#### 2-3. Better Instruction Scheduling

Flat sequences of operations allow the compiler to:

- schedule instructions efficiently
- pipeline operations
- reduce dependency stalls

### 3. Performance Intuition

For a **1920×1080 image**

Pixels:

$$
1920 \times 1080 \approx 2.07M
$$

Operations per frame:

$$
2.07M \times 25 \approx 51.75M
$$

Even tiny savings per pixel can produce measurable gains.

#### Trade-offs

| Aspect | Nested Loop | Unrolled |
|---|---|---|
| Readability | High | Low |
| Maintainability | High | Low |
| Flexibility | High | Low |
| Loop overhead | Present | Removed |
| Index arithmetic | More | Less |
| Code size | Small | Large |

Manual unrolling is useful when:

- kernel size is fixed
- code is performance critical
- you control the full pipeline
- profiling shows convolution is a bottleneck

Otherwise, maintainable code is usually preferable.

Loop unrolling demonstrates an important principle in high-performance computing.
Sometimes the cost is not in the math itself.

The cost is in **how the computation is structured**.
For small fixed kernels such as **5×5 convolution**, removing loop overhead can provide meaningful speed improvements in performance-critical systems.
