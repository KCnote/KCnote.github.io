---
layout: post
title: "11. Convolution"
date: 2026-05-13 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>Convolution</b>

---

### <b>Prerequisites</b>

    python

---

## <b>1. Convolution</b>

Convolution is very popular algorithm as you know. It makes features smoothing/blurring, enhancing image. Furthermore, Edge detection and Feature extraction algorithms are also using this method.

This is based on kernel. How to you build the kernel, the result is different. No matter how much i emphasize it, it's never enough.

## <b>2. Convolution Code</b>

```python
import cv2 as cv
import os
from enum import Enum


class KernelType(Enum):
    GAUSSIAN_BLUR = 0
    UNIFORM_BLUR = 1
    SHARPEN = 2
    LAPLACIAN = 3
    SOBEL_X = 4
    SOBEL_Y = 5
    EMBOSS = 6

def convolution3x3(img, kernel_type):
    if kernel_type == KernelType.GAUSSIAN_BLUR:
        kernel_1d = cv.getGaussianKernel(3, 0)
        kernel = kernel_1d @ kernel_1d.T
    elif kernel_type == KernelType.UNIFORM_BLUR:
        kernel = np.ones((3, 3), dtype=np.float32)
        kernel /= 9
    elif kernel_type == KernelType.SHARPEN:
        kernel = np.array([
            [0, -1, 0],
            [-1, 5, -1],
            [0, -1, 0]
        ], dtype=np.float32)
    elif kernel_type == KernelType.LAPLACIAN:
        kernel = np.array([
            [0,  1, 0],
            [1, -4, 1],
            [0,  1, 0]
        ], dtype=np.float32)
    elif kernel_type == KernelType.SOBEL_X:
        kernel = np.array([
            [-1, 0, 1],
            [-2, 0, 2],
            [-1, 0, 1]
        ], dtype=np.float32)
    elif kernel_type == KernelType.SOBEL_Y:
        kernel = np.array([
            [-1, -2, -1],
            [ 0,  0,  0],
            [ 1,  2,  1]
        ], dtype=np.float32)
    elif kernel_type == KernelType.EMBOSS:
        kernel = np.array([
            [-2, -1, 0],
            [-1,  1, 1],
            [ 0,  1, 2]
        ], dtype=np.float32)

    else:
        raise ValueError("Invalid kernel type")

    return cv.filter2D(img, -1, kernel)
```

```python
if __name__ == "__main__":
    img = ImageUtils.readImage(ImageUtils.getDataPathWithFile("cat.png"))
    imgGaussian = ip.convolution3x3(img, ip.KernelType.GAUSSIAN_BLUR)
    imgUniform = ip.convolution3x3(img, ip.KernelType.UNIFORM_BLUR)
    imgSharpen = ip.convolution3x3(img, ip.KernelType.SHARPEN)
    imgLaplacian = ip.convolution3x3(img, ip.KernelType.LAPLACIAN)
    imgSobelX = ip.convolution3x3(img, ip.KernelType.SOBEL_X)
    imgSobelY = ip.convolution3x3(img, ip.KernelType.SOBEL_Y)   
    imgEmboss = ip.convolution3x3(img, ip.KernelType.EMBOSS)
        
    viewer = view.MultiImageViewer.from_images(img, imgGaussian, imgUniform, imgSharpen, imgLaplacian, imgSobelX, imgSobelY, imgEmboss, sync_view=False)
    viewer.run()
```

!["opencv-python-11-01.png"](../assets/img/develop/opencv-python-11-01.png)
