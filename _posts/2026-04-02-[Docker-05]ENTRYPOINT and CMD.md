---
layout: post
title: "05. Docker CMD vs ENTRYPOINT"
date: 2026-04-02 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - Docker]
tags: [Deploy, Deploy - Docker]
pin: false
math: true
mermaid: true
---

# <b>CMD vs ENTRYPOINT</b>

---

### <b>Prerequisites</b>


---

## <b>1. Docker CMD vs ENTRYPOINT </b>

This section explains the **roles of CMD and ENTRYPOINT**, how they work together, and what happens when you override them using `--entrypoint`.

> **ENTRYPOINT = fixed executable (program)**
> **CMD = default arguments**


```text
ENTRYPOINT(Not Replaceable) + CMD(Fully Replaceable) = final command
```

```text
ENTRYPOINT = program
CMD        = default parameter
```

#### <b>1-1. CMD Only</b>
   
##### CMD is **fully replaceable**

```dockerfile
FROM ubuntu:22.04
CMD ["echo", "hello"]
```

##### ✔️ Run

```bash
docker run my-image
```

same as:

```bash
docker run my-image echo hello
```

```text
hello
```

##### ✔️ Override CMD

```bash
docker run my-image ls
```

same as:

```bash
docker run my-image ls
```

👉 Actual execution:

```bash
ls
```

#### <b>1-2. ENTRYPOINT Only</b>

##### ENTRYPOINT is **not replaced**, arguments are appended

```dockerfile
FROM ubuntu:22.04
ENTRYPOINT ["echo", "hello"]
```

---

##### ✔️ Run

```bash
docker run my-image world
```

same as:

```bash
docker run my-image echo hello world
```

👉 Actual execution:

```bash
echo hello world
```

##### ✔️ Override ENTRYPOINT → X

```bash
docker run my-image ls
```

same as:

```bash
docker run my-image echo hello ls
```

👉 Actual execution:

```bash
echo hello ls
```

#### <b>1-3. ENTRYPOINT + CMD</b>

##### CMD is **fully replaceable**
##### ENTRYPOINT is **not replaced**, arguments are appended

```dockerfile
FROM ubuntu:22.04
ENTRYPOINT ["echo"]
CMD ["hello"]
```

##### ✔️ Run (default)

```bash
docker run my-image
```

same as:

```bash
docker run my-image echo hello
```

👉 Actual execution:

```bash
echo hello
```

##### ✔️ Run (override argument)

```bash
docker run my-image world
```

same as:

```bash
docker run my-image echo world
```

👉 Actual execution:

```bash
echo world
```

## <b>2. Practical Example (Python)</b>

```dockerfile
FROM python:3.10
ENTRYPOINT ["python"]
CMD ["app.py"]
```

##### ✔️ Default

```bash
docker run my-image
```

```bash
python app.py
```

##### ✔️ Change argument

```bash
docker run my-image test.py
```

```bash
python test.py
```

##### ✔️ Invalid argument case

```bash
docker run my-image ls
```

👉 Actual execution:

```bash
python ls
```

👉 Result:

```text
python: can't open file 'ls': No such file or directory
```

## <b>3. Overriding ENTRYPOINT with `--entrypoint`</b>

> CMD provides default arguments, ENTRYPOINT defines the executable, and `--entrypoint` allows you to override it at runtime.

```bash
docker run -it --entrypoint bash my-image
```

##### ✔️ What happens?

👉 ENTRYPOINT is replaced:

```bash
bash
```

* ENTRYPOINT → replaced
* CMD → ignored

| Case                               | Result           |
| ---------------------------------- | ---------------- |
| docker run image                   | ENTRYPOINT + CMD |
| docker run image arg               | ENTRYPOINT + arg |
| docker run --entrypoint bash image | bash only        |

#### <b>3-1. Why Use `--entrypoint`?</b>

##### ✔️ Debugging

```bash
docker run -it --entrypoint bash my-image
```

* Access container manually
* Inspect files, logs, environment

##### ✔️ Bypass default execution

If container always runs an app:

```dockerfile
ENTRYPOINT ["python", "app.py"]
```

👉 You cannot stop it normally

👉 But:

```bash
docker run -it --entrypoint bash my-image
```

👉 You can enter container without running the app
