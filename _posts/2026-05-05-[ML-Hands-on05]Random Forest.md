---
layout: post
title: "Hands-on 05. Random Forest"
date: 2026-05-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - Hands on]
tags: [Machince Learning]
pin: false
math: true
mermaid: true
---

# <b>Random Forest</b>

---

### <b>Prerequisites</b>

    Random Forest

---

## <b>1. How to implement the real</b>

Random Forest is ensembling model with various random decision tree. Each trees has different features from being choosed randomly and structure. So when it comes to inference with each trees, combined the each decision and build the result like average, majority vote

```python
import pandas as pd
import numpy as np

from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_absolute_error, r2_score

RANDOM_STATE = 42

def train_random_forest(df):

    # Drop rows involved NA
    df = df.copy()

    rf_features = [
        "bedrooms",
        "bathrooms",
        "garage",
        "land_area",
        "floor_area",
        "latitude",
        "longitude",
        "cbd_dist",
        "nearest_stn_dist",
        "nearest_sch_dist",
    ]

    available_features = [c for c in rf_features if c in df.columns]
    model_df = df.dropna(subset=available_features + ["price"]).copy()

    if len(model_df) < 100:
        df["predicted_price"] = np.nan
        df["prediction_gap"] = np.nan
        df["prediction_gap_pct"] = np.nan
        return df, {"mae": None, "r2": None, "features": []}

    X = model_df[available_features]
    y = model_df["price"]

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=RANDOM_STATE
    )

    # set model
    model = RandomForestRegressor(
        n_estimators=120,
        max_depth=14,
        min_samples_leaf=4,
        random_state=RANDOM_STATE,
        n_jobs=-1,
    )

    # train
    model.fit(X_train, y_train)

    # inference
    y_pred = model.predict(X_test)
    all_pred = model.predict(X)

    # result
    model_df["predicted_price"] = all_pred
    model_df["prediction_gap"] = model_df["predicted_price"] - model_df["price"]
    model_df["prediction_gap_pct"] = model_df["prediction_gap"] / model_df["predicted_price"]

    df["predicted_price"] = np.nan
    df["prediction_gap"] = np.nan
    df["prediction_gap_pct"] = np.nan

    df.loc[model_df.index, ["predicted_price", "prediction_gap", "prediction_gap_pct"]] = model_df[["predicted_price", "prediction_gap", "prediction_gap_pct"]]

    importance = sorted(
        zip(available_features, model.feature_importances_),
        key=lambda x: x[1], reverse=True,
    )

    metrics = {
        "mae": float(mean_absolute_error(y_test, y_pred)),
        "r2": float(r2_score(y_test, y_pred)),
        "features": importance[:8],
    }

    return df, metrics


df = pd.read_csv("data.csv")
df, metrics = train_random_forest(df)
```

#### <b>1-1. Data</b>

1. Select features
2. Split train and test data

#### <b>1-2. Random Forest</b>

1. Set model parameters

   * Number of estimators (tree count)
   * Maximum depth
   * Minimum samples per leaf
   * Number of features to consider at each split

2. Process as follows:

   1. Create a Decision Tree

      1. Generate a bootstrap sample (random sampling with replacement)
      2. At each node:

         * Randomly select a subset of features
         * For each selected feature, test all possible split thresholds
         * Choose the split that minimizes error (e.g., MSE)
         * 
      3. Split the data based on the best condition
      4. Repeat recursively until stopping criteria are met
         (e.g., max depth, minimum samples)

   2. Repeat tree creation

      * Build multiple trees independently using different random samples
      * Continue until the total number of estimators is reached

   3. Make predictions

      * Pass input data through all trees
      * Aggregate results:

        * Regression → average
        * Classification → majority vote



#### <b>1.3 In real</b>

!["ml-ho-rf01"](../assets/img/develop/ml-ho-rf01.png)

## <b>2. How to work Random Forest</b>

```
Random Forest =
Data randomness +
Feature randomness +
Optimal split selection +
Aggregation (average / vote)
```

Random Forest is an ensemble learning method used to improve prediction accuracy by combining multiple decision trees.
Instead of relying on a single tree, it builds many trees using randomness and aggregates their results.

Complexity Comparison:

```
Single Decision Tree:   High variance (overfitting risk)
Random Forest:          Reduced variance (stable prediction)
```

#### <b>2-1. Build Process (Tree Construction)</b>

Random Forest builds multiple decision trees using:

```
1. Bootstrap Sampling (random data)
2. Random Feature Selection
3. Best Split Selection (not random split)
```

#### Example Data

```
House : (bedrooms, area, cbd_dist) → price

A : (2, 300, 15) → 500k
B : (3, 500, 10) → 800k
C : (4, 700, 5)  → 1200k
D : (3, 400, 12) → 700k
E : (5, 900, 3)  → 1500k
```

##### > Tree 1

##### Step 1: Bootstrap Sampling

```
[A, B, C, D]
```

##### Step 2: Random Feature Selection

```
Selected features:
area, cbd_dist
```

##### Step 3: Try multiple split candidates

```
area < 400
area < 600
cbd_dist < 10
```

##### Step 4: Evaluate each split (MSE)

```
area < 400 → MSE = 120k
area < 600 → MSE = 80k   ✅ best
cbd_dist < 10 → MSE = 150k
```

##### Step 5: Choose best split

```
if area < 600
```

##### > Tree 2

##### Step 1: Bootstrap Sampling

```
[B, C, D, E]
```

##### Step 2: Random Feature Selection

```
Selected features:
bedrooms, cbd_dist
```

##### Step 3: Split candidates

```
bedrooms < 4
cbd_dist < 8
cbd_dist < 15
```

##### Step 4: Evaluation

```
bedrooms < 4 → MSE = 90k
cbd_dist < 8 → MSE = 70k   ✅ best
cbd_dist < 15 → MSE = 100k
```

##### Step 5: Best split

```
if cbd_dist < 8
```

##### > Tree 3

##### Step 1: Bootstrap Sampling

```
[A, C, E]
```

##### Step 2: Random Feature Selection

```
Selected features:
area, bedrooms
```

##### Step 3: Split candidates

```
area < 500
bedrooms < 4
```

##### Step 4: Evaluation

```
area < 500 → MSE = 60k   ✅ best
bedrooms < 4 → MSE = 110k
```

##### Step 5: Best split

```
if area < 500
```

#### > Final Tree Concept

Each tree is:

```
- trained on different data
- uses different features
- finds different decision rules
```

#### <b>2-2. Prediction</b>

```
Predict price for:
(3 bedrooms, 450 area, cbd_dist = 10)
```

##### Step 1: Run all trees

```
Tree 1 → 1000k
Tree 2 → 750k
Tree 3 → 650k
```

##### Step 2: Aggregate results

###### For regression:

```
Final = Average

(1000 + 750 + 650) / 3 = 800k
```

###### For classification:

```
Tree1 → Premium
Tree2 → Mid
Tree3 → Mid

Final = Majority Vote → Mid
```
