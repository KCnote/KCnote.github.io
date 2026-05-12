---
layout: post
title: "03. Image as data"
date: 2026-05-12 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>Image as data</b>

---

### <b>Prerequisites</b>

    python
    RGB

---

## <b>1. What is Image</b>

On computer vision, image is a set of numeric data. As viusalized, the image is very intuitive, however when we process images, we should approach image as data.

Bascially, Image has features as follows:

#### (W, H, C)

```
width
height
channel
```

#### pixel

```
byte type per pixel
```

## <b>2. How to read image</b>

##### st_image.py

```python
import cv2 as cv
import os

def getDataPath():
    strRoot = os.getcwd()
    strDir = os.path.join(strRoot, "data")
    return strDir

def getImagePath(path):
    strImage = os.path.join(getDataPath(), path)
    return strImage

def readImage(path):
    img = cv.imread(path)
    return img

def showImage(title, img):
    cv.imshow(title, img)
    cv.waitKey(0)
    cv.destroyAllWindows()
```

##### main.py

```python
import cv2 as cv
import st_image

if __name__ == "__main__":
    img = st_image.readImage(st_image.getImagePath("cat.png"))
    st_image.showImage("Cat", img)
```