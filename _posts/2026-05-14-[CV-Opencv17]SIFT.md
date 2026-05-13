---
layout: post
title: "17. SIFT"
date: 2026-05-14 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>SIFT</b>

---

### <b>Prerequisites</b>

    python

---

## <b>1. SIFT</b>

SIFT (Scale-Invariant Feature Transform) is a feature extraction algorithm used in computer vision to detect and describe local features in an image. It is designed to be robust against image scaling, rotation, illumination changes, and partial viewpoint variations.

The first step of SIFT is keypoint detection. SIFT searches for stable feature points across multiple image scales using a Difference of Gaussian (DoG) pyramid. The image is repeatedly blurred and downsampled to create scale-space representations. Then, adjacent Gaussian-blurred images are subtracted to detect strong local extrema.

After detecting candidate keypoints, SIFT computes gradient information around each point. The gradients are calculated using image derivatives:

$dx = I(x + 1, y) - I(x - 1, y)$
$dy = I(x, y + 1) - I(x, y - 1)$

Using these values, the gradient magnitude and orientation are calculated:

magnitude = $sqrt(dx² + dy²)$
orientation = $atan2(dy, dx)$

The dominant orientation of the local region is assigned to the keypoint. This makes SIFT rotation invariant because descriptors are generated relative to the keypoint orientation.

Next, SIFT creates a descriptor around the keypoint. A local image patch is divided into a 4 × 4 grid. For each cell, an orientation histogram with 8 bins is computed using gradient directions. Since there are:

4 × 4 cells × 8 orientation bins = 128 values

the final SIFT descriptor becomes a 128-dimensional feature vector.

These descriptors are highly distinctive and can be used for feature matching between images. Similar descriptors usually represent the same physical point in different images.

Unlike Harris Corner Detection, SIFT not only detects feature points but also provides scale and orientation information, making it much more robust for real-world applications such as object recognition, panorama stitching, SLAM, and image matching.

SIFT (Scale-Invariant Feature Transform) Important Logic Flow

1. Scale-Space Construction
2. Difference of Gaussian (DoG)
3. 3D Local Extrema Detection
4. Remove Unstable Keypoints
5. Orientation Assignment
6. Descriptor Generation
7. Feature Matching

Core Ideas of SIFT

- Scale Invariant:
Features are detected across multiple scales.

- Rotation Invariant:
Descriptors are aligned using dominant orientation.

- Robust Descriptor:
Gradient histograms represent local structure rather than raw pixel values.

- Matching:
Descriptors can match the same object even if:
    - resized
    - rotated
    - partially illuminated differently

## <b>2. SIFT Code</b>

```python
if __name__ == "__main__":
    cap = cv.VideoCapture(0)
    
    if not cap.isOpened():
        exit()
    
    ret, img = cap.read()
    
    viewer = view.MultiImageViewer.from_images(img, img, sync_view=False)
    
    while True:
        ret, img = cap.read()
        
        if not ret:
            break

        gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
        sift = cv.SIFT_create()
        keypoints, descriptors = sift.detectAndCompute(gray, None)
        imgSift = cv.drawKeypoints(img, keypoints, None, color=(0,255,0))

        if ret:
            viewer.update_images(img, imgSift)
            viewer.draw()
        
        key = cv.waitKey(1) & 0xFF
        
        if key == ord('q'):
            break
```