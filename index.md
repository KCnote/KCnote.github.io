---
layout: page
title: Home
permalink: /
---

<div style="
background: linear-gradient(135deg,#177e89,#1f2937);
color:white;
padding:40px 30px;
border-radius:14px;
margin-bottom:30px;
">

<h1 style="margin:0;font-size:38px;color:white;">Carrey Kang</h1>

<p style="margin:8px 0 0 0;font-size:18px;opacity:0.95;">
Computer Vision Engineer
</p>

<p style="margin:14px 0 0 0;font-size:14px;opacity:0.85;">
C++ • Computer Vision • Vision Algorithms • Machine Learning
</p>

<p style="margin-top:16px;font-size:14px;">
📍 Perth, Australia &nbsp;&nbsp;
🔗 <a style="color:white;" href="https://www.linkedin.com/in/carreykangcho/">LinkedIn</a> &nbsp;&nbsp;
💻 <a style="color:white;" href="dev.kangcho@gmail.com">Email</a>
</p>

</div>

## 👋 About Me

I am a computer vision engineer focused on building **high-performance vision algorithms and scalable C++ libraries** for industrial systems.

My main interest is designing algorithms that are:

- **robust in real-world environments**
- **efficient enough for production systems**
- **simple for engineers to use**

I spent several years developing computer vision algorithms used in **industrial inspection systems**, including pattern matching, metrology, and image analysis pipelines.

My work has been deployed in **large-scale production environments**, including semiconductor and manufacturing lines.

### **[Click more about me](/about)**

---

## 🔬 What I Work On

My main areas of interest are:

- **Computer Vision Algorithms**
- **Pattern Matching / Geometric Detection**
- **Industrial Vision Systems**
- **Performance Optimization**
- **C++ Library Design**

I enjoy solving problems where **mathematics, software engineering, and real-world systems meet**.

---

## 🧠 What You'll Find on This Blog

This blog is my personal knowledge base where I document topics such as:

### Computer Vision
- Pattern matching
- geometric vision
- inspection algorithms
- image processing

### Machine Learning
- transformers
- deep learning fundamentals
- optimization

### Mathematics
- linear algebra
- matrix decomposition
- probability & statistics

### Engineering
- C++ performance optimization
- multithreading
- SIMD
- algorithm design

---

## 🚀 Selected Work

Some of the work I have done includes:

- Designing **geometric pattern matching algorithms** robust to occlusion and illumination changes
- Developing **high-performance C++ vision libraries**
- Optimizing algorithms using **SIMD (AVX/SSE), multithreading, and CUDA**
- Deploying inspection algorithms in **large-scale industrial production lines**

Some of this work resulted in **patents related to vision algorithms**.

---

## ✍️ Recent Posts

{% for post in site.posts limit:6 %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}

➡️ [View all posts]({{ '/archives/' | relative_url }})

---

## 📂 Topics

You can browse posts by topic:

- Linear Algebra
- Computer Vision
- Machine Learning
- C++
- System Design
