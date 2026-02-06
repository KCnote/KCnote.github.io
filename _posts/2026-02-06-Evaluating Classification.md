---
layout: post
title: "Evaluating Classification"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Classification]
tags: [Machince Learning, Mathematics, ML, Supervised, Regression, Classification, Discriminant, LDA]
pin: false
math: true
mermaid: true
---

# Evaluating Classification

> Full mathematical notes with extended metrics and intuition  
> Covers confusion matrix, threshold trade-off, ROC/AUC, and practical metrics.

---

# 1. 📊 Confusion Matrix (Credit Card Default)

|               | True No | True Yes | Total |
|--------------|---------|----------|-------|
| **Pred No**  | 9644    | 252      | 9896  |
| **Pred Yes** | 23      | 81       | 104   |
| **Total**    | 9667    | 333      | 10000 |

**Definitions**

$$
TN = 9644,\quad FP = 23,\quad FN = 252,\quad TP = 81
$$

---

# 2. ❌ Misclassification Rate

$$
\text{Error} = \frac{FP + FN}{N}
= \frac{23 + 252}{10000}
= 0.0275 = 2.75\%
$$

### Baseline (Always predict No)

$$
\frac{333}{10000} = 3.33\%
$$

✔ Model improves over naive baseline.

---

# 3. ⚠️ Error Types

## False Positive Rate (FPR)

$$
\text{FPR} = \frac{FP}{FP + TN}
= \frac{23}{9667}
\approx 0.0024
$$

---

## False Negative Rate (FNR)

$$
\text{FNR} = \frac{FN}{FN + TP}
= \frac{252}{333}
\approx 0.757
$$

Interpretation → many defaulters missed.

---

## True Positive Rate (Recall)

$$
\text{TPR} = \frac{TP}{TP + FN} = 1 - \text{FNR}
$$

---

## True Negative Rate (Specificity)

$$
\text{TNR} = \frac{TN}{TN + FP} = 1 - \text{FPR}
$$

---

# 4. 🎯 Decision Rule via Probability Threshold

$$
\hat y =
\begin{cases}
1 & \text{if } P(Default=Yes \mid x) \ge t \\
0 & \text{otherwise}
\end{cases}
$$

Default:

$$
t = 0.5
$$

Changing threshold controls **FN vs FP trade-off**.

---

# 5. 🔄 Threshold Trade-off

Lower threshold:

- ↑ TP
- ↓ FN
- ↑ FP

Higher threshold:

- ↓ FP
- ↑ FN

$$
\text{Cannot minimize FP and FN simultaneously}
$$

---

# 6. 📉 Overall Error vs Threshold

$$
\text{Error}(t) = \frac{FP(t) + FN(t)}{N}
$$

Behavior:

- Small $t$ → High FP  
- Large $t$ → High FN  
- Middle $t$ minimizes total error  

---

# 7. 📈 ROC Curve

ROC plots:

$$
(\text{FPR}(t),\ \text{TPR}(t))
$$

Properties:

- Top-left corner → Perfect classifier  
- Diagonal → Random guessing  

---

## AUC (Area Under Curve)

$$
\text{AUC} = \int_0^1 \text{TPR}(u)\, d(\text{FPR}(u))
$$

Interpretation:

$$
\text{AUC} = P(\text{model ranks positive higher than negative})
$$

Higher AUC → Better ranking performance.

---

# 8. 🧠 Additional Practical Metrics

## Precision

$$
\text{Precision} = \frac{TP}{TP + FP}
$$

---

## Recall

$$
\text{Recall} = \text{TPR}
$$

---

## F1 Score

$$
F1 =
\frac{2 \cdot \text{Precision} \cdot \text{Recall}}
{\text{Precision} + \text{Recall}}
$$

---

## Balanced Error Rate (BER)

$$
\text{BER} = \frac{\text{FPR} + \text{FNR}}{2}
$$

Useful for **imbalanced datasets**.

---

# 9. 🎯 Choosing Optimal Threshold

### Minimize total error

$$
t^* = \arg\min_t \text{Error}(t)
$$

---

### Maximize F1

$$
t^* = \arg\max_t F1(t)
$$

---

### Cost-Sensitive Decision

If FN is more expensive → choose **lower threshold**.

---

# 10. 🚀 Key Insights

- Accuracy alone is misleading for imbalanced data
- ROC/AUC evaluates ranking ability independent of threshold
- Threshold depends on **business cost & risk**
- In credit default:
  - FN = missing defaulter (very costly)
  - FP = false alarm (less costly)

👉 Always tune threshold based on **application objective**

---
