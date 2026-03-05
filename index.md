---
layout: page
title: Home
permalink: /
---

<div style="
padding:120px 40px;
text-align:center;
background:linear-gradient(135deg,#1f4037,#99f2c8);
color:white;
border-radius:12px;
">

<h1 style="font-size:48px;">Cho Kang</h1>

<p style="font-size:22px;">
Computer Vision Engineer
</p>

<p>
C++ • Machine Learning • Industrial Vision
</p>

</div>

<div class="hero">

# Cho Kang

### Computer Vision Engineer

C++ | Machine Learning | Industrial Vision | Algorithm Design

</div>

---

## 👋 About Me

Computer vision engineer specializing in high-performance C++ vision libraries and industrial inspection systems.

- Pattern Matching
- Geometric Vision
- 3D Vision Systems
- Algorithm Optimization

[View Resume](/about)

---

## 📝 Recent Posts

<ul>
{% for post in site.posts limit:5 %}
<li>
<a href="{{ post.url }}">{{ post.title }}</a>
<span style="color:gray"> {{ post.date | date: "%Y-%m-%d" }}</span>
</li>
{% endfor %}
</ul>

---

## 📂 Categories

- Linear Algebra
- Machine Learning
- Computer Vision
- C++ Engineering


