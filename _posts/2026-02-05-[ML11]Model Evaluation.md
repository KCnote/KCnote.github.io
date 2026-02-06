---
layout: post
title: "Model Evaluation"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Overview]
tags: [Machince Learning, Overview, Cross Validation, K-Fold, Model]
pin: false
math: true
mermaid: true
---

# 📊 How to check Model Evaluation

---

# Choosing the Optimal Model

## How to Choose a Model

### Question
Which model is the best one?

Common answers:
- Smallest RSS
- Largest $$R^2$$

---

## Observation

- The model containing **all predictors** will always have:
  - The **smallest RSS**
  - The **largest $$R^2$$**
- Adding more variables seems to have nothing to lose.
- One might think useless variables will simply get coefficients close to **zero**, so there is no harm.

However, this reasoning is **not correct for generalization**.

---

## Training vs Test Error

- Our goal is **low test error**, not low training error.
- Training error is usually a **poor estimate of test error**.
- A model with many variables may fit training data well but perform poorly on unseen data.

---

## Overfitting Risk

- Using all variables can lead to **overfitting**.
- The model may capture noise instead of true signal.
- This increases **variance** and harms prediction on new data.

---

## Key Principle

- Model selection should be based on **test error (generalization)**.
- RSS and $$R^2$$ from training data alone are **not reliable** for comparing models with different complexity.

---

# Evaluation Strategy

## Goal

We want to verify that the model learns **general patterns**, not memorization.

---

## Hold-out Test Data

- Set aside a portion of data for **testing / evaluation**.
- Test data **must NOT be used during training**.
- Example: In Kaggle, test labels are hidden.

---

## How to Split Data

### Common Approach
- Random uniform split
- Typical ratio: **10%–20% test**

### Problem-Dependent Approach
- Time-series split:
  - Train on past
  - Evaluate on future
- Tests ability to predict future from past.

---

## Key Takeaway

- Evaluate **generalization**, not memorization.
- Keep a **strictly separate test set**.
- Proper splitting is critical.

---

# Choosing Hyperparameters

## Why Hyperparameters Matter

- Many ML models require **hyperparameters**.
- Examples:
  - Model complexity (number of predictors $$p$$)
  - Learning rate
  - Regularization strength

---

## How to Choose Hyperparameters

- Use **data-driven selection**:
  - Train multiple models
  - Compare performance
  - Choose best

---

## Validation Set

- Needed for hyperparameter tuning.
- **Do NOT use test set for tuning**.
- Split training data → **training + validation**.

---

## Key Takeaway

- Hyperparameters control **model complexity and behavior**.
- Use validation set for tuning.
- Keep test set only for **final evaluation**.

---

# Cross Validation

## Procedure

### Step 1: Train using training set
Fit model using training data (e.g., 70%).

---

### Step 2: Evaluate using validation set
Measure performance on validation data (e.g., 20%).

---

### Step 3: Hyperparameter tuning
Repeat with different hyperparameters, choose best.

---

### Step 4: Retrain with best hyperparameters
Train using **training + validation data**.

---

### Step 5: Final evaluation
Evaluate on **test set** (e.g., 10%).

---

## Important Rule

- The **test set must NOT be used** until final evaluation.

---

## Typical Split

- Training: 70%
- Validation: 20%
- Test: 10%

---

## Key Takeaway

- Validation → model selection & tuning
- Test → final unbiased evaluation
- Ensures proper measurement of generalization

---

# K-Fold Cross Validation

## Idea

- Split dataset into **K folds**.
- Train **K times**:
  - K-1 folds → training
  - 1 fold → validation
- Each fold used **once as validation**.
- Final performance = average validation score.

---

## Procedure

1. Split data into $$K$$ folds.
2. For each $$i = 1,\dots,K$$:
   - Train on all folds except $$i$$
   - Evaluate on fold $$i$$
3. Average results:

$$
\text{CV Error} = \frac{1}{K} \sum_{i=1}^{K} \text{Error}_i
$$

---

## Example (K = 5)

- Fold 1 → validation, others → training  
- Fold 2 → validation, others → training  
- Fold 3 → validation, others → training  
- Fold 4 → validation, others → training  
- Fold 5 → validation, others → training  

Each fold is used **exactly once** as validation.

---

## Key Takeaway

- Uses **all data** for training and validation (in different rounds).
- Produces **stable and reliable estimate** of test error.
- Common choices:
  - $$K = 5$$
  - $$K = 10$$
