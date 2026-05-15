---
layout: post
title: "20. Pos Estimation"
date: 2026-05-15 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>Pos Estimation</b>

---

### <b>Prerequisites</b>

    python

---

## <b>1. Pos Estimation</b>

# Pose Estimation

Pose estimation is the process of finding the position and orientation of an object relative to a camera. In computer vision, this is commonly done using known 3D points and their corresponding 2D image points.

For example, when detecting a chessboard pattern, we already know the real-world 3D coordinates of each corner:

```text
(0,0,0)
(1,0,0)
(2,0,0)
...
```

At the same time, the image coordinates of those corners:

```text
(523,221)
(540,220)
...
```

Using these 3D–2D correspondences together with the camera intrinsic matrix, Estimating the camera pose by minimizing the reprojection error between known 3D object points and their corresponding 2D image points.

The algorithm first assumes an initial rotation and translation, then projects the 3D points onto the image plane using the camera intrinsic matrix. After projection, it compares the predicted 2D positions with the actual detected image points.

The difference between these points is called the reprojection error. Repeatedly updating the rotation and translation values to reduce this error through optimization. 

```
[R|T]
```

- Rotation vector: R
- Translation vector: T

This means the algorithm estimates how the object is rotated and where it is located relative to the camera.

After obtaining the pose, it can project 3D points back onto the image using projection.

For example, assume a 3D point exists at:

```text
(2,1,10)
```

This means:

- 2 units to the right
- 1 unit upward
- 10 units in front of the camera

To project this point onto the image plane:

$$
x = X / Z = 2 / 10 = 0.2, \:\:\:
y = Y / Z = 1 / 10 = 0.1
$$

These normalized coordinates are then converted into pixel coordinates using the intrinsic matrix:

$$
u = fx \times x + cx, \:\:\:
v = fy \times y + cy
$$

If:

```
fx = fy = 1000

cx = 640
cy = 360
```

$$
u = 1000 \times 0.2 + 640 = 840, \:\:\:
v = 1000 \times 0.1 + 360 = 460
$$

So the 3D point `(2,1,10)` appears at pixel position:

```text
(840,460)
```

This is the core idea behind pose estimation and 3D point projection in computer vision.

## <b>2. Pos Estimation Code</b>

```python
import cv2 as cv
import os
import ImageUtils
import VideoUtils
import numpy as np
import MultiImageViewer as view
import Viewers
import ImageProcessing as ip
import glob

def pose_estimation_webcam():
    # Inner corners count
    CHECKERBOARD = (7, 6)

    # Camera Matrix
    camera_matrix = np.array([
        [1.30458050e+03, 0.00000000e+00, 2.80160438e+02],
        [0.00000000e+00, 1.32052587e+03, 3.11461051e+02],
        [0.00000000e+00, 0.00000000e+00, 1.00000000e+00]
    ], dtype=np.float32)

    # Distortion Coefficients
    dist_coeffs = np.array([
        [-1.83011423e+00,
         1.28165170e+02,
         5.68135052e-02,
         3.31047683e-02,
         -2.27203441e+03]
    ], dtype=np.float32)

    criteria = (cv.TERM_CRITERIA_EPS + cv.TERM_CRITERIA_MAX_ITER,30,0.001)

    # 3D object points of chessboard corners
    objp = np.zeros((CHECKERBOARD[0] * CHECKERBOARD[1], 3), np.float32)
    objp[:, :2] = np.mgrid[0:CHECKERBOARD[0],0:CHECKERBOARD[1]].T.reshape(-1, 2)

    # Axis length
    axis = np.float32([[3, 0, 0], [0, 3, 0], [0, 0, -3]])

    cap = cv.VideoCapture(0)

    if not cap.isOpened():
        return

    while True:
        ret, frame = cap.read()

        if not ret:
            break

        gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
        found, corners = cv.findChessboardCorners(gray,CHECKERBOARD,None)

        if not found:
            print("Chessboard corners not found")

        if found:
            print("Chessboard corners found")

            corners_refined = cv.cornerSubPix(gray,corners,(11, 11),(-1, -1),criteria)
            success, rvec, tvec = cv.solvePnP(objp,corners_refined,camera_matrix,dist_coeffs)

            if success:
                imgpts, _ = cv.projectPoints(axis,rvec,tvec,camera_matrix,dist_coeffs)

                corner = tuple(corners_refined[0].ravel().astype(int))
                imgpts = imgpts.reshape(-1, 2).astype(int)

                cv.line(frame,corner,tuple(imgpts[0]),(255, 0, 0),4) # Draw X axis - blue
                cv.line(frame,corner,tuple(imgpts[1]),(0, 255, 0),4) # Draw Y axis - green
                cv.line(frame,corner,tuple(imgpts[2]),(0, 0, 255),4) # Draw Z axis - red
                cv.putText(frame,f"tvec: x={tvec[0][0]:.2f}, y={tvec[1][0]:.2f}, z={tvec[2][0]:.2f}",(20, 40),cv.FONT_HERSHEY_SIMPLEX,0.7,(0, 255, 255),2)

        cv.imshow("Pose Estimation", frame)

        key = cv.waitKey(1) & 0xFF

        if key == ord("q") or key == 27:
            break

    cap.release()
    cv.destroyAllWindows()


if __name__ == "__main__":
    pose_estimation_webcam()
```