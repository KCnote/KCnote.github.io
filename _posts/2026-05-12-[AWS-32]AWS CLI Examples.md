---
layout: post
title: "32. AWS CLI Examples"
date: 2026-05-12 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>AWS CLI Examples</b>

---

### <b>Prerequisites</b>

    cmd

---

## <b>1. AWS CLI Examples</b>

#### <b>1-1. DAILY + Usage Type</b>

```bash
aws ce get-cost-and-usage ^
  --time-period Start=2026-05-10,End=2026-05-13 ^
  --granularity DAILY ^
  --metrics UnblendedCost ^
  --group-by Type=DIMENSION,Key=USAGE_TYPE ^
  --region us-east-1
```