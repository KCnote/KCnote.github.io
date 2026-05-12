---
layout: post
title: "02. Set Environment with virtual machine"
date: 2026-05-12 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Opencv]
tags: [Computer Vision, Opencv, Pythons]
pin: false
math: true
mermaid: true
---

# <b>Set Environment with virtual machine</b>

---

### <b>Prerequisites</b>

    python

---

## <b>1. Why we set Environment with virtual machine</b>

When we use library, sometimes the version is not support some functions or another issue. So we will use virtual machine with python.

```bash
python -m venv {name}
```

If you remove virtual machine

```bash
Remove-Item -Recurse -Force .\{name}
```

##### Activate virtual machine

```bash
{name}\Scripts\activate
```

#### <b>1-1. on virtal machine</b>

If you activate venv, we're already in venv path like `(venv-opencv) PS C:\Users\imjg0\Desktop\opencv-project>`

First python version checking

```bash
python --version
python -m pip install --upgrade pip
```

Now you should install package you want on venv path.

```bash
pip install {package}
```

Other way (recommended)

Current install list regist on `requirements.txt`

```bash
pip freeze > requirements.txt
```

On requirements.txt, regist the list of package name

```text
numpy==2.1.1
opencv-python==4.12.0.88
open3d==0.19.0
scipy
matplotlib
```

Install package listed

```bash
pip install -r requirements.txt
```

Check current packages installed

```bash
pip freeze
```

If you want to know where located the packages

```bash
pip show numpy
```

#### <b>1-2. folder hierarchy</b>

```
opencv-project/
│
├─ src/
│   ├─ main.py
│   └─ image-processing.py
│
├─ data/
│   └─ image.png
│
├─ outputs/
│
├─ requirements.txt
├─ README.md
└─ venv-opencv/   # virtual environments
```

#### main.py

```python
import cv2 as cv

if __name__ == "__main__":
    print(cv.__version__)
```

I will focus on the logical structure and how to use opencv with python.

##### Let's start!