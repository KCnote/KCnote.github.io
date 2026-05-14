---
layout: post
title: "18. Optical Flow"
date: 2026-05-14 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>Optical Flow</b>

---

### <b>Prerequisites</b>

    python

---

## <b>1. Optical Flow</b>

Optical Flow is a computer vision technique used to estimate the motion of objects, surfaces, or pixels between consecutive image frames.

There are two main types of Optical Flow: sparse and dense. 
Sparse Optical Flow tracks only selected feature points such as corners or textured regions, making it faster and suitable for tracking applications. A common example is the Lucas-Kanade method. 
Dense Optical Flow, such as the Farneback algorithm, computes motion for every pixel in the image, providing a complete motion field but requiring more computation.

## <b>2. Optical Flow Code</b>

```python
if __name__ == "__main__":
    cap = cv.VideoCapture(0)

    if not cap.isOpened():
        return

    MIN_MOTION = 2.0
    MAX_MOTION = 100.0

    ret, old_frame = cap.read()

    if not ret:
        return

    old_gray = cv.cvtColor(old_frame, cv.COLOR_BGR2GRAY)
    feature_params = dict(maxCorners=80, qualityLevel=0.03, minDistance=20, blockSize=7, useHarrisDetector=True, k=0.04)
    p0 = cv.goodFeaturesToTrack(old_gray, mask=None, **feature_params)
    frame_count = 0

    viewer = view.MultiImageViewer([old_frame], True)

    while True:
        ret, frame = cap.read()

        if not ret:
            break

        frame_gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
        frame_count += 1
        display = frame.copy()
        next_points = []

        if p0 is not None and len(p0) > 0:
            lk_params = dict(winSize=(21, 21), maxLevel=3, criteria=(cv.TERM_CRITERIA_EPS | cv.TERM_CRITERIA_COUNT, 20, 0.03))
            p1, status, err = cv.calcOpticalFlowPyrLK(old_gray, frame_gray, p0, None, **lk_params)

            if p1 is not None and status is not None:
                good_new = p1[status == 1]
                good_old = p0[status == 1]

                for new, old in zip(good_new, good_old):
                    x_new, y_new = new.ravel()
                    x_old, y_old = old.ravel()

                    dx = x_new - x_old
                    dy = y_new - y_old
                    dist = np.sqrt(dx * dx + dy * dy)

                    # Remove static background points
                    if dist < MIN_MOTION:
                        continue

                    # Remove unstable/wrong tracking points
                    if dist > MAX_MOTION:
                        continue

                    cv.arrowedLine(display,(int(x_old), int(y_old)),(int(x_new), int(y_new)),(0, 255, 0),2,tipLength=0.4)
                    cv.circle(display,(int(x_new), int(y_new)),4,(0, 0, 255),-1)
                    next_points.append(new)

        if len(next_points) > 0:
            p0 = np.array(next_points, dtype=np.float32).reshape(-1, 1, 2)
        else:
            p0 = None

        # Re-detect points periodically or when points are too few
        if frame_count % 10 == 0 or p0 is None or len(p0) < 20:
            detect_mask = np.ones_like(frame_gray, dtype=np.uint8) * 255

            if p0 is not None:
                for point in p0:
                    x, y = point.ravel()
                    cv.circle(detect_mask,(int(x), int(y)),20,0,-1)

            new_points = cv.goodFeaturesToTrack(frame_gray,mask=detect_mask,**feature_params)

            if new_points is not None:
                if p0 is None:
                    p0 = new_points
                else:
                    p0 = np.vstack([p0, new_points])

        cv.putText(display,f"Tracked moving points: {0 if p0 is None else len(p0)}",(20, 30),cv.FONT_HERSHEY_SIMPLEX,0.75,(0, 255, 0),2)
        viewer.update_images(display)
        viewer.draw()

        old_gray = frame_gray.copy()

        key = cv.waitKey(1) & 0xFF

        if key == ord("q"):
            break

        if key == ord("r"):
            p0 = cv.goodFeaturesToTrack(frame_gray,mask=None,**feature_params)
            old_gray = frame_gray.copy()

    cap.release()
    cv.destroyAllWindows()
```  