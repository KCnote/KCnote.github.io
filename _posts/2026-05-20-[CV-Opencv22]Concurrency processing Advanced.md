---
layout: post
title: "22. Concurrency Processing - Advanced"
date: 2026-05-15 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>Concurrency Processing</b>

---

### <b>Prerequisites</b>

    python

---

## <b>1. Concurrency Processing</b>

We already know how to build concurrency processing. In this post, I will create a practice that detects face.
The face detection model is YUNet. The main point of this post is how to design correct architecture.

There're processing sections:

1. Capture Image from webcam
2. Processing for detecting face
3. Save log when processing failed
4. Save image when processing failed

If there's no concurrency, the processing is serialized and some processing is stopped temporary because of another process such as save image processing.

However, with concurrency, there's no stopping and continuously the processes move on each process


## <b>2. Concurrency Processing Code</b>

```python
import cv2 as cv
import threading
import queue
import time
import os
import cv2 as cv
import ImageUtils
import numpy as np
import MultiImageViewer as view
import Viewers
import ImageProcessing as ip
import glob
import numpy as np
import matplotlib.pyplot as plt

MODEL_PATH = "face_detection_yunet_2023mar.onnx"
FRAME_WIDTH = 640
FRAME_HEIGHT = 480

frame_queue = queue.Queue(maxsize=1)
save_queue = queue.Queue(maxsize=100)
stop_event = threading.Event()

latest_frame = None
latest_result_frame = None
latest_status = "No frame"

frame_lock = threading.Lock()

def put_latest(q, item):
    try:
        q.put_nowait(item)
    except queue.Full:
        try:
            q.get_nowait()
        except queue.Empty:
            pass

        try:
            q.put_nowait(item)
        except queue.Full:
            pass

def capture_thread():
    global latest_frame

    cap = cv.VideoCapture(0)
    cap.set(cv.CAP_PROP_FRAME_WIDTH, FRAME_WIDTH)
    cap.set(cv.CAP_PROP_FRAME_HEIGHT, FRAME_HEIGHT)

    if not cap.isOpened():
        print("Cannot open camera")
        stop_event.set()
        return

    frame_id = 0

    while not stop_event.is_set():
        ret, frame = cap.read()

        if not ret:
            continue

        with frame_lock:
            latest_frame = frame.copy()

        put_latest(frame_queue, (frame_id, frame))
        frame_id += 1

    cap.release()

def processing_thread():
    global latest_result_frame
    global latest_status

    if not os.path.exists(MODEL_PATH):
        print(f"Model file not found: {MODEL_PATH}")
        stop_event.set()
        return

    detector = cv.FaceDetectorYN.create(
        MODEL_PATH,
        "",
        (FRAME_WIDTH, FRAME_HEIGHT),
        score_threshold=0.6,
        nms_threshold=0.3,
        top_k=5000
    )

    fail_count = 0
    FAIL_THRESHOLD = 10

    while not stop_event.is_set():
        try:
            frame_id, frame = frame_queue.get(timeout=0.1)
        except queue.Empty:
            continue

        result, face_count = detect_faces_yunet(detector, frame)

        if face_count > 0:
            fail_count = 0
            status = f"FACE FOUND: {face_count}"

        else:
            fail_count += 1
            status = f"NO FACE count={fail_count}"

            if fail_count >= FAIL_THRESHOLD:
                try:
                    save_queue.put_nowait((frame_id, result.copy()))
                    status = "NO FACE - SAVED"
                    fail_count = 0
                except queue.Full:
                    status = "SAVE QUEUE FULL"

        with frame_lock:
            latest_result_frame = result.copy()
            latest_status = status

        frame_queue.task_done()

def detect_faces_yunet(detector, frame):
    result = frame.copy()
    h, w = result.shape[:2]

    detector.setInputSize((w, h))

    _, faces = detector.detect(frame)

    face_count = 0

    if faces is not None and len(faces) > 0:
        face_count = len(faces)

        for face in faces:
            x, y, bw, bh = face[:4].astype(int)
            score = face[-1]

            x = max(0, x)
            y = max(0, y)

            cv.rectangle(
                result,
                (x, y),
                (x + bw, y + bh),
                (0, 255, 0),
                2
            )

            cv.putText(
                result,
                f"Face {score:.2f}",
                (x, max(20, y - 10)),
                cv.FONT_HERSHEY_SIMPLEX,
                0.7,
                (0, 255, 0),
                2
            )

            landmarks = face[4:14].astype(int).reshape((5, 2))

            for px, py in landmarks:
                cv.circle(result, (px, py), 3, (0, 255, 255), -1)

            right_eye = tuple(landmarks[0])
            left_eye = tuple(landmarks[1])
            nose = tuple(landmarks[2])
            right_mouth = tuple(landmarks[3])
            left_mouth = tuple(landmarks[4])

            mouth_center = (
                (right_mouth[0] + left_mouth[0]) // 2,
                (right_mouth[1] + left_mouth[1]) // 2
            )

            cv.line(result, left_eye, right_eye, (255, 255, 0), 2)
            cv.line(result, left_eye, nose, (255, 255, 0), 2)
            cv.line(result, right_eye, nose, (255, 255, 0), 2)
            cv.line(result, nose, mouth_center, (255, 255, 0), 2)

    return result, face_count

def save_thread():
    os.makedirs("fail", exist_ok=True)

    log_path = "fail/fail_log.txt"

    while not stop_event.is_set() or not save_queue.empty():
        try:
            frame_id, frame = save_queue.get(timeout=0.5)
        except queue.Empty:
            continue

        timestamp = time.strftime("%Y-%m-%d %H:%M:%S")
        file_timestamp = time.strftime("%Y%m%d_%H%M%S")

        filename = f"fail/fail_to_find_{file_timestamp}_{frame_id}.jpg"

        success = cv.imwrite(filename, frame)

        if success:
            log_message = f"[{timestamp}] DETECTING FAILED | frame_id={frame_id} | file={filename}"
            print(log_message)
        else:
            log_message = f"[{timestamp}] SAVE FAILED | frame_id={frame_id} | file={filename}"
            print(log_message)

        with open(log_path, "a", encoding="utf-8") as f:
            f.write(log_message + "\n")

        save_queue.task_done()


def main():
    t1 = threading.Thread(target=capture_thread)
    t2 = threading.Thread(target=processing_thread)
    t3 = threading.Thread(target=save_thread)

    t1.start()
    t2.start()
    t3.start()

    imgEmpty = np.zeros((FRAME_HEIGHT, FRAME_WIDTH, 3), dtype=np.uint8)
    viewer1 = view.MultiImageViewer([imgEmpty,imgEmpty])

    while not stop_event.is_set():
        with frame_lock:
            frame = None if latest_frame is None else latest_frame.copy()
            result = None if latest_result_frame is None else latest_result_frame.copy()
            status = latest_status

        if frame is not None:
            cv.putText(frame,status,(20, 40),cv.FONT_HERSHEY_SIMPLEX,1,(0, 255, 0) if "FACE FOUND" in status else (0, 0, 255),2)

        if result is not None and frame is not None:
            viewer1.update_images(frame, result)
            viewer1.draw()

        if cv.waitKey(1) & 0xFF == ord("q"):
            stop_event.set()
            break

    t1.join()
    t2.join()
    t3.join()

    cv.destroyAllWindows()


if __name__ == "__main__":
    main()
```

### Result: Concureency processing

!["opencv-python-22-01.png"](../assets/img/develop/opencv-python-22-01.gif)
