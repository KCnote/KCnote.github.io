---
layout: post
title: "Entry of Machine Learning"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Overview]
tags: [Machince Learning, Overview, ML, AL, Supervised, Unsupervised, Parametric, Non-Parametric]
pin: false
math: true
mermaid: true
---

# 🤖 Machine Learning & AI Overview

---

## 🧠 Intelligence

> **Ability to perceive or infer information and apply it to adaptive behavior.**

---

# 📊 Statistical Machine Learning

Machine Learning is a field of Artificial Intelligence focused on building systems that **learn from data** and **generalize to unseen data** without explicit programming.

$$
\boxed{\text{Learning from data → Generalization → Adaptive behavior}}
$$

### ✨ Core Idea
- 📈 Data-driven approach  
- ⚙️ Minimize manual rules / instructions  
- 🔍 Automatically discover patterns from data  

---

# 🤖 Machine Learning (ML)

> A field of AI that develops **statistical algorithms** capable of learning from data and generalizing to unseen data **without explicit instruction**.

### 🔑 Key Characteristics
- 📊 Statistical learning
- 🧩 Pattern discovery from data
- 🌍 Generalization to unseen data
- 🔧 Reduce manual feature engineering

---

# 🧠 Deep Learning (DL)

Deep Learning is a subset of ML that attempts to **mimic how human intelligence works** — especially perception.

$$
\boxed{\text{Representation Learning via Neural Networks}}
$$

### ⚡ Characteristics
- 🧠 Neural network based
- 🏗️ Hierarchical feature learning
- 🎯 Strong performance in:
  - Computer Vision
  - Speech Recognition
  - Natural Language Processing
- 💾 Requires large-scale data & compute

---

# 🎓 Learning Paradigms

## 🟢 Supervised Learning

**Given:** (input, label) pairs  
**Goal:** Learn mapping → predict label for unseen data  

$$
\boxed{\text{Learn } f(x) \rightarrow y}
$$

**Examples**
- Classification
- Regression

---

## 🔵 Unsupervised Learning

**Given:** data without labels  
**Goal:** Discover hidden structure purely from data  

$$
\boxed{\text{Discover structure in data}}
$$

**Examples**
- Clustering
- Dimensionality Reduction
- Density Estimation

---

## ⚠️ Typical Difficulty

$$
\boxed{\text{Unsupervised} > \text{Supervised}}
$$

**Why harder?**
- ❌ No ground truth
- ❌ Harder to evaluate
- ❌ Structure must be inferred

---

# ⚙️ Parametric vs Non‑Parametric

## 📌 Parametric Methods

Assume a **functional form** and learn parameters.

$$
\boxed{\text{Model = Fixed form + Learn parameters}}
$$

### Characteristics
- Fixed number of parameters
- Faster training
- Works with smaller datasets
- Strong distribution assumptions

**Examples**
- Linear Regression
- Logistic Regression
- Naive Bayes

---

## 📌 Non‑Parametric Methods

Do **not assume functional form** — fit closely to data without overfitting.

$$
\boxed{\text{Flexible model capacity → Needs more data}}
$$

### Characteristics
- Flexible / adaptive complexity
- Can model complex patterns
- Requires large-scale dataset
- Higher computational cost

**Examples**
- k‑Nearest Neighbors (kNN)
- Decision Trees
- Kernel Methods
- Gaussian Processes

---

# 📌 Summary Table

| Concept | Key Idea |
|--------|----------|
| 🤖 ML | Learn patterns from data |
| 🧠 DL | Neural networks mimicking cognition |
| 🟢 Supervised | Learn from labeled data |
| 🔵 Unsupervised | Discover hidden structure |
| 📌 Parametric | Fixed model, learn parameters |
| 📌 Non‑Parametric | Flexible, data‑driven, needs more data |

---

# 💡 Key Insight

$$
\boxed{
\text{ML = Learning from data} \quad
\text{DL = Representation learning}
}
$$

- Supervised → easier, label‑driven  
- Unsupervised → harder, structure discovery  
- Parametric → simple & efficient  
- Non‑parametric → flexible but data‑hungry  

---

# 🚀 Big Picture

$$
\boxed{
\text{AI} \supset \text{Machine Learning} \supset \text{Deep Learning}
}
$$


---

# 🆚 Machine Learning vs Deep Learning

## Core Difference

$$
\boxed{
\text{Machine Learning = Learn patterns from features} \quad
\text{Deep Learning = Learn features + patterns automatically}
}
$$

---

## 🔍 Key Conceptual Difference

| Aspect | 🤖 Machine Learning | 🧠 Deep Learning |
|--------|--------------------|-----------------|
| Feature Engineering | Manual / Human-designed | Automatic (Representation Learning) |
| Model Type | Statistical models | Neural Networks |
| Data Requirement | Works with small–medium data | Requires large-scale data |
| Compute | Low–Moderate | High (GPU/TPU) |
| Interpretability | Higher | Lower (Black-box) |
| Performance | Good for structured data | Superior for perception tasks |
| Pipeline | Feature → Model → Prediction | Raw Data → Neural Network → Prediction |

---

## ⚙️ Workflow Difference

### Machine Learning

$$
\boxed{
\text{Data} \rightarrow 
\text{Feature Engineering} \rightarrow 
\text{Model} \rightarrow 
\text{Prediction}}
$$

Human expertise is required to design meaningful features.

---

### Deep Learning

$$
\boxed{
\text{Raw Data} \rightarrow 
\text{Neural Network} \rightarrow 
\text{Automatic Feature Learning} \rightarrow 
\text{Prediction}}
$$

The model **learns features automatically** from raw data.

---

## 📊 When to Use Which?

### Use Machine Learning when:
- 📊 Data is structured (tabular, signals, measurements)
- 📉 Dataset is small / medium
- ⚡ Fast training & interpretability needed
- 🏭 Industrial / classical statistical problems

---

### Use Deep Learning when:
- 🖼️ Data is unstructured (image, video, speech, text)
- 📦 Large-scale dataset available
- 🎯 Highest accuracy required
- 🧠 Complex pattern recognition needed

---

## 💡 Insight

$$
\boxed{
\text{DL is a subset of ML, but with automatic representation learning}
}
$$

- ML focuses on **learning relationships**
- DL focuses on **learning representations + relationships**

