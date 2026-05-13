---
layout: post
title: "07. Copy and Paste"
date: 2026-05-13 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>Copy and Paste</b>

---

### <b>Prerequisites</b>

    python

---

## <b>1. Copy and Paste</b>

Literally, copy and paste we use on vision engineering. Sometimes we wanna reduce the region of interest, manage the set of images and so on.

Bascially, source image and target image should be same format about byte type and channels. If it is not same, it will be making Logical error or silent failure(critical)

## <b>2. Copy and Paste Code</b>

```python
import cv2 as cv
import os
import ImageUtils
import MultiImageViewer as view

if __name__ == "__main__":
    img = ImageUtils.readImage(ImageUtils.getDataPathWithFile("cat.png"))
    img_gray = ImageUtils.convertColorSpace(img, cv.COLOR_BGR2GRAY)

    img[100:200, 100:200] = img_gray[100:200, 100:200] # Not Working and error
    img_copy = img.copy()
    img_copy[100:200, 100:200] = img[300:400, 300:400]

    viewer = view.MultiImageViewer.from_images(img, img_copy, sync_view=False)
    viewer.run()

    img_ref = img
    img_ref[100:200, 100:200] = img[300:400, 300:400]

    viewer = view.MultiImageViewer.from_images(img, img_ref, sync_view=False)
    viewer.run()
```

#### image + deepcopy (.copy())

!["opencv-python-06-01.png"](../assets/img/develop/opencv-python-06-01.png)

#### image + shallowcopy (=)

!["opencv-python-06-02.png"](../assets/img/develop/opencv-python-06-02.png)
