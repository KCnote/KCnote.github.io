---
layout: post
title: "04. Image Viewer"
date: 2026-05-12 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>Image Viewer</b>

---

### <b>Prerequisites</b>

    python
    UI

---

## <b>1. Image Viewer</b>

The vision is related with visualization. When I was developing vision alogithms, the image as tensor is just numeric set. So it is not easy to understand what is wrong or how to solve the relationship between images. So more efficiently developing key is using UI Viewer

Implemented Features as follows:

#### 1. Multi-image viewer
   - Dual panel image comparison viewer
   - Side-by-side synchronized visualization

#### 2. Smooth panning
   - Left mouse drag panning
   - Real-time viewport movement

#### 3. Zoom system
   - Mouse wheel zoom in/out
   - Cursor-centered zoom behavior
   - Adjustable zoom limits

#### 4. Pixel inspection
   - Pixel grid visualization at high zoom levels
   - Per-pixel BGR value display
   - Vertical text layout inside each pixel

#### 5. Dynamic pixel rendering optimization
   - Pixel text rendering enabled only above a zoom threshold
   - Automatic clipping to visible viewport region only
   - Visible pixel count limitation for performance optimization

#### 6. Text readability improvements
   - Center-aligned pixel values
   - White text with black outline for visibility on all backgrounds

#### 7. Runtime synchronization system
   - Independent panel navigation mode
   - Synchronized navigation mode
   - Ctrl+Y runtime sync toggle
   - Active panel based synchronization

#### 8. Status bar UI
   - Bottom-right coordinate display
   - Current pixel BGR value display
   - Zoom level display
   - Synchronization state display

#### 9. Rendering optimization
   - Viewport-based rendering
   - Cropped region processing only
   - INTER_NEAREST scaling for pixel-accurate visualization
   - Reduced unnecessary redraw overhead

#### 10. User interaction controls
    - q / ESC : quit
    - r       : reset view
    - Ctrl+Y  : synchronization toggle
    - Mouse drag : panning
    - Mouse wheel : zoom

## <b>2. Image Viewer Code</b>

```python
import cv2 as cv
import numpy as np

class MultiImageViewer:
    def __init__(self, imgs, sync_view=True):
        if len(imgs) != 2:
            raise ValueError("imgs must contain exactly two images")

        self.imgs = [imgs[0], imgs[1]]

        if self.imgs[0] is None:
            raise ValueError("First image is None")
        if self.imgs[1] is None:
            raise ValueError("Second image is None")

        self.win_name = "Multi Pixel Viewer"

        self.panel_w = 600
        self.panel_h = 760
        self.top_h = 32
        self.status_h = 40

        self.win_w = self.panel_w * 2
        self.win_h = self.top_h + self.panel_h + self.status_h

        self.sync_view = sync_view

        self.zoom = [1.0, 1.0]
        self.min_zoom = 0.1
        self.max_zoom = 80.0

        self.center_x = [
            self.imgs[0].shape[1] / 2,
            self.imgs[1].shape[1] / 2
        ]
        self.center_y = [
            self.imgs[0].shape[0] / 2,
            self.imgs[1].shape[0] / 2
        ]

        self.is_dragging = False
        self.drag_panel = 0
        self.last_mouse_x = 0
        self.last_mouse_y = 0
        self.mouse_x = 0
        self.mouse_y = 0

        self.grid_zoom_threshold = 12.0
        self.pixel_value_zoom_threshold = 35.0
        self.max_text_pixels = 600

        cv.namedWindow(self.win_name, cv.WINDOW_NORMAL)
        cv.resizeWindow(self.win_name, self.win_w, self.win_h)
        cv.setMouseCallback(self.win_name, self.on_mouse)

    @classmethod
    def from_paths(cls, path1, path2, sync_view=True):
        img1 = cv.imread(path1, cv.IMREAD_UNCHANGED)
        img2 = cv.imread(path2, cv.IMREAD_UNCHANGED)

        if img1 is None:
            raise FileNotFoundError(path1)
        if img2 is None:
            raise FileNotFoundError(path2)

        return cls([img1, img2], sync_view=sync_view)

    @classmethod
    def from_images(cls, img1, img2, sync_view=True):
        return cls([img1, img2], sync_view=sync_view)

    @staticmethod
    def to_display_bgr(img):
        """
        원본 이미지는 바꾸지 않고, OpenCV canvas에 붙이기 위한 표시용 이미지만 만든다.

        지원:
        - Gray:  (H, W)       -> BGR
        - BGR:   (H, W, 3)    -> 그대로
        - BGRA:  (H, W, 4)    -> BGR
        """
        if img is None:
            return None

        if len(img.shape) == 2:
            return cv.cvtColor(img, cv.COLOR_GRAY2BGR)

        if len(img.shape) == 3:
            if img.shape[2] == 1:
                return cv.cvtColor(img, cv.COLOR_GRAY2BGR)
            if img.shape[2] == 3:
                return img
            if img.shape[2] == 4:
                return cv.cvtColor(img, cv.COLOR_BGRA2BGR)

        raise ValueError(f"Unsupported image shape: {img.shape}")

    @staticmethod
    def pixel_to_text(pixel):
        """
        pixel 값을 이미지 포맷에 맞게 문자열로 변환한다.
        """
        if np.isscalar(pixel):
            return f"Gray={int(pixel)}"

        values = pixel.tolist()

        if len(values) == 1:
            return f"Gray={int(values[0])}"
        if len(values) == 3:
            b, g, r = values
            return f"BGR=({int(b)},{int(g)},{int(r)})"
        if len(values) == 4:
            b, g, r, a = values
            return f"BGRA=({int(b)},{int(g)},{int(r)},{int(a)})"

        return f"Pixel={values}"

    @staticmethod
    def pixel_to_value_list(pixel):
        """
        확대했을 때 픽셀 안에 표시할 값 목록.
        Gray면 1줄, BGR이면 3줄, BGRA면 4줄.
        """
        if np.isscalar(pixel):
            return [str(int(pixel))]

        values = pixel.tolist()
        return [str(int(v)) for v in values]

    def get_panel_index(self, sx, sy):
        if sy < self.top_h or sy >= self.top_h + self.panel_h:
            return None

        if 0 <= sx < self.panel_w:
            return 0

        if self.panel_w <= sx < self.panel_w * 2:
            return 1

        return None

    def put_pixel_text(self, img, text, org, scale=0.32):
        cv.putText(
            img,
            text,
            org,
            cv.FONT_HERSHEY_SIMPLEX,
            scale,
            (0, 0, 0),
            2,
            cv.LINE_AA
        )
        cv.putText(
            img,
            text,
            org,
            cv.FONT_HERSHEY_SIMPLEX,
            scale,
            (255, 255, 255),
            1,
            cv.LINE_AA
        )

    def screen_to_image(self, sx, sy, panel_idx):
        local_x = sx - panel_idx * self.panel_w
        local_y = sy - self.top_h

        img_x = self.center_x[panel_idx] + (local_x - self.panel_w / 2) / self.zoom[panel_idx]
        img_y = self.center_y[panel_idx] + (local_y - self.panel_h / 2) / self.zoom[panel_idx]

        return img_x, img_y

    def clamp_panel(self, panel_idx):
        h, w = self.imgs[panel_idx].shape[:2]

        self.center_x[panel_idx] = max(0, min(self.center_x[panel_idx], w - 1))
        self.center_y[panel_idx] = max(0, min(self.center_y[panel_idx], h - 1))

    def clamp_all(self):
        self.clamp_panel(0)
        self.clamp_panel(1)

    def copy_view_to_other(self, source_idx):
        target_idx = 1 - source_idx

        self.zoom[target_idx] = self.zoom[source_idx]
        self.center_x[target_idx] = self.center_x[source_idx]
        self.center_y[target_idx] = self.center_y[source_idx]

        self.clamp_all()

    def on_mouse(self, event, x, y, flags, param):
        self.mouse_x = x
        self.mouse_y = y

        panel_idx = self.get_panel_index(x, y)
        if panel_idx is None:
            return

        active_panels = [0, 1] if self.sync_view else [panel_idx]

        if event == cv.EVENT_LBUTTONDOWN:
            self.is_dragging = True
            self.drag_panel = panel_idx
            self.last_mouse_x = x
            self.last_mouse_y = y

        elif event == cv.EVENT_LBUTTONUP:
            self.is_dragging = False

        elif event == cv.EVENT_MOUSEMOVE and self.is_dragging:
            dx = x - self.last_mouse_x
            dy = y - self.last_mouse_y

            move_panel = self.drag_panel
            active_panels = [0, 1] if self.sync_view else [move_panel]

            for idx in active_panels:
                self.center_x[idx] -= dx / self.zoom[idx]
                self.center_y[idx] -= dy / self.zoom[idx]
                self.clamp_panel(idx)

            self.last_mouse_x = x
            self.last_mouse_y = y

        elif event == cv.EVENT_MOUSEWHEEL:
            before = self.screen_to_image(x, y, panel_idx)

            old_zoom = self.zoom[panel_idx]

            if flags > 0:
                new_zoom = old_zoom * 1.25
            else:
                new_zoom = old_zoom / 1.25

            new_zoom = max(self.min_zoom, min(new_zoom, self.max_zoom))

            if self.sync_view:
                for idx in [0, 1]:
                    self.zoom[idx] = new_zoom
            else:
                self.zoom[panel_idx] = new_zoom

            after = self.screen_to_image(x, y, panel_idx)

            dx_img = before[0] - after[0]
            dy_img = before[1] - after[1]

            for idx in active_panels:
                self.center_x[idx] += dx_img
                self.center_y[idx] += dy_img
                self.clamp_panel(idx)

    def get_visible_region(self, panel_idx):
        img_h, img_w = self.imgs[panel_idx].shape[:2]
        zoom = self.zoom[panel_idx]

        visible_w = self.panel_w / zoom
        visible_h = self.panel_h / zoom

        x0 = int(np.floor(self.center_x[panel_idx] - visible_w / 2))
        y0 = int(np.floor(self.center_y[panel_idx] - visible_h / 2))
        x1 = int(np.ceil(self.center_x[panel_idx] + visible_w / 2))
        y1 = int(np.ceil(self.center_y[panel_idx] + visible_h / 2))

        src_x0 = max(0, x0)
        src_y0 = max(0, y0)
        src_x1 = min(img_w, x1)
        src_y1 = min(img_h, y1)

        return x0, y0, src_x0, src_y0, src_x1, src_y1

    def draw_grid_and_values(self, panel, img, panel_idx, x0, y0, src_x0, src_y0, src_x1, src_y1):
        zoom = self.zoom[panel_idx]

        if zoom < self.grid_zoom_threshold:
            return

        for ix in range(src_x0, src_x1 + 1):
            sx = int((ix - x0) * zoom)
            if 0 <= sx < self.panel_w:
                cv.line(panel, (sx, 0), (sx, self.panel_h), (80, 80, 80), 1)

        for iy in range(src_y0, src_y1 + 1):
            sy = int((iy - y0) * zoom)
            if 0 <= sy < self.panel_h:
                cv.line(panel, (0, sy), (self.panel_w, sy), (80, 80, 80), 1)

        if zoom < self.pixel_value_zoom_threshold:
            return

        visible_pixel_count = (src_x1 - src_x0) * (src_y1 - src_y0)
        if visible_pixel_count > self.max_text_pixels:
            return

        for iy in range(src_y0, src_y1):
            sy = int((iy - y0) * zoom)

            if sy < 0 or sy >= self.panel_h:
                continue

            for ix in range(src_x0, src_x1):
                sx = int((ix - x0) * zoom)

                if sx < 0 or sx >= self.panel_w:
                    continue

                pixel = img[iy, ix]
                values = self.pixel_to_value_list(pixel)

                scale = 0.32
                line_gap = max(10, int(zoom * 0.22))
                total_height = line_gap * (len(values) - 1)
                start_y = sy + int((zoom - total_height) / 2)

                for i, value in enumerate(values):
                    text_size, _ = cv.getTextSize(
                        value,
                        cv.FONT_HERSHEY_SIMPLEX,
                        scale,
                        1
                    )

                    text_w = text_size[0]
                    text_h = text_size[1]

                    tx = sx + int((zoom - text_w) / 2)
                    ty = start_y + i * line_gap + int(text_h / 2)

                    self.put_pixel_text(panel, value, (tx, ty), scale)

    def draw_one_panel(self, canvas, panel_idx):
        img = self.imgs[panel_idx]
        zoom = self.zoom[panel_idx]

        panel_offset_x = panel_idx * self.panel_w
        panel_y0 = self.top_h

        panel = canvas[
            panel_y0:panel_y0 + self.panel_h,
            panel_offset_x:panel_offset_x + self.panel_w
        ]

        panel[:] = 40

        x0, y0, src_x0, src_y0, src_x1, src_y1 = self.get_visible_region(panel_idx)

        if src_x0 >= src_x1 or src_y0 >= src_y1:
            return

        patch = img[src_y0:src_y1, src_x0:src_x1]

        dst_x0 = int((src_x0 - x0) * zoom)
        dst_y0 = int((src_y0 - y0) * zoom)

        dst_w = max(1, int(patch.shape[1] * zoom))
        dst_h = max(1, int(patch.shape[0] * zoom))

        interp = cv.INTER_NEAREST if zoom >= 1 else cv.INTER_AREA
        resized = cv.resize(patch, (dst_w, dst_h), interpolation=interp)

        # canvas는 3채널 BGR이므로, 표시할 때만 변환한다.
        # self.imgs 안의 원본 이미지는 Gray/BGR/BGRA 그대로 유지된다.
        resized_display = self.to_display_bgr(resized)

        dst_x1 = dst_x0 + dst_w
        dst_y1 = dst_y0 + dst_h

        cx0 = max(0, dst_x0)
        cy0 = max(0, dst_y0)
        cx1 = min(self.panel_w, dst_x1)
        cy1 = min(self.panel_h, dst_y1)

        if cx1 > cx0 and cy1 > cy0:
            rx0 = cx0 - dst_x0
            ry0 = cy0 - dst_y0
            rx1 = rx0 + (cx1 - cx0)
            ry1 = ry0 + (cy1 - cy0)

            panel[cy0:cy1, cx0:cx1] = resized_display[ry0:ry1, rx0:rx1]

        self.draw_grid_and_values(
            panel,
            img,
            panel_idx,
            x0,
            y0,
            src_x0,
            src_y0,
            src_x1,
            src_y1
        )

    def draw_top_bar(self, canvas):
        canvas[0:self.top_h, :] = 30

        sync_text = "ON" if self.sync_view else "OFF"
        text = f"drag: pan | wheel: zoom | t: sync {sync_text} | r: reset | q/esc: quit"

        cv.putText(
            canvas,
            text,
            (10, 23),
            cv.FONT_HERSHEY_SIMPLEX,
            0.55,
            (255, 255, 255),
            1,
            cv.LINE_AA
        )

        cv.line(
            canvas,
            (self.panel_w, self.top_h),
            (self.panel_w, self.top_h + self.panel_h),
            (180, 180, 180),
            1
        )

    def draw_status_bar(self, canvas):
        y0 = self.top_h + self.panel_h
        canvas[y0:y0 + self.status_h, :] = 30

        panel_idx = self.get_panel_index(self.mouse_x, self.mouse_y)

        if panel_idx is None:
            text = "outside panel"
        else:
            img_x, img_y = self.screen_to_image(self.mouse_x, self.mouse_y, panel_idx)
            img = self.imgs[panel_idx]
            h, w = img.shape[:2]

            if 0 <= img_x < w and 0 <= img_y < h:
                ix = int(img_x)
                iy = int(img_y)
                pixel = img[iy, ix]
                pixel_text = self.pixel_to_text(pixel)

                text = (
                    f"panel={'A' if panel_idx == 0 else 'B'} | "
                    f"x={ix}, y={iy} | "
                    f"{pixel_text} | "
                    f"zoom={self.zoom[panel_idx]:.2f}x | "
                    f"sync={'ON' if self.sync_view else 'OFF'}"
                )
            else:
                text = (
                    f"panel={'A' if panel_idx == 0 else 'B'} | "
                    f"outside image | "
                    f"zoom={self.zoom[panel_idx]:.2f}x | "
                    f"sync={'ON' if self.sync_view else 'OFF'}"
                )

        text_size, _ = cv.getTextSize(text, cv.FONT_HERSHEY_SIMPLEX, 0.52, 1)
        x = max(10, self.win_w - text_size[0] - 15)
        y = y0 + 27

        cv.putText(
            canvas,
            text,
            (x, y),
            cv.FONT_HERSHEY_SIMPLEX,
            0.52,
            (255, 255, 255),
            1,
            cv.LINE_AA
        )

    def draw_panel_labels(self, canvas):
        y = self.top_h + 25

        cv.putText(
            canvas,
            "Image A",
            (10, y),
            cv.FONT_HERSHEY_SIMPLEX,
            0.7,
            (255, 255, 255),
            1,
            cv.LINE_AA
        )

        cv.putText(
            canvas,
            "Image B",
            (self.panel_w + 10, y),
            cv.FONT_HERSHEY_SIMPLEX,
            0.7,
            (255, 255, 255),
            1,
            cv.LINE_AA
        )

    def draw(self):
        # 창 전체 canvas는 OpenCV imshow용으로 3채널 BGR 고정.
        # 각 이미지 원본 포맷은 self.imgs에 그대로 유지된다.
        canvas = np.full((self.win_h, self.win_w, 3), 40, dtype=np.uint8)

        self.draw_one_panel(canvas, 0)
        self.draw_one_panel(canvas, 1)

        self.draw_top_bar(canvas)
        self.draw_panel_labels(canvas)
        self.draw_status_bar(canvas)

        cv.imshow(self.win_name, canvas)

    def reset(self):
        for i in [0, 1]:
            h, w = self.imgs[i].shape[:2]
            self.zoom[i] = 1.0
            self.center_x[i] = w / 2
            self.center_y[i] = h / 2
            self.clamp_panel(i)

    def run(self):
        while True:
            self.draw()
            key = cv.waitKey(16) & 0xFF

            if key == ord("q") or key == 27:
                break

            if key == ord("r"):
                self.reset()

            if key == ord("t"):
                self.sync_view = not self.sync_view

            if key == 25:  # Ctrl + Y
                if self.sync_view:
                    self.sync_view = False
                else:
                    panel_idx = self.get_panel_index(self.mouse_x, self.mouse_y)

                    if panel_idx is None:
                        panel_idx = 0

                    self.copy_view_to_other(panel_idx)
                    self.sync_view = True

        cv.destroyAllWindows()
```

```python
import cv2 as cv
import os
import ImageUtils
import MultiImageViewer as view

if __name__ == "__main__":
    img = ImageUtils.readImage(ImageUtils.getDataPathWithFile("cat.png"))
    viewer = view.MultiImageViewer("data/cat.png", "data/cat.png", sync_view=False)
    viewer.run()
```

!["opencv-python-04-01.png"](../assets/img/develop/opencv-python-04-01.png)
