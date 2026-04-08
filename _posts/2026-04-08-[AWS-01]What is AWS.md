---
layout: post
title: "01. What is AWS - Understanding Cloud Infrastructure"
date: 2026-04-08 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>AWS</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is AWS </b>

AWS (Amazon Web Services) is a cloud computing platform provided by Amazon.

Instead of buying and maintaining physical servers, AWS allows you to build systems using **on-demand infrastructure**.

> You don’t own servers anymore. You *use* them when needed.

## <b>2. Why AWS Exists</b>

Before cloud computing, building a service required:

- Buying physical servers
- Setting up network infrastructure
- Managing hardware failures
- Predicting traffic in advance

This caused several problems:

- High upfront cost
- Low flexibility
- Difficult scaling
- Maintenance overhead

#### Traditional Infrastructure (On-Premise)

- Fixed hardware capacity
- Manual scaling
- Hardware management required
- Long provisioning time

#### Why This Matters for Developers

- Think in **systems**, not servers
- Design for **failure**
- Build for **scalability**
- Automate everything


#### AWS (Cloud)

- Elastic resources (scale up/down anytime)
- Pay-as-you-go pricing
- Fully managed infrastructure
- Deploy in minutes

> **Cloud turns infrastructure into software**

## 🧩 Core AWS Services

AWS is composed of many services.  
Here are the most essential ones:

| Category | Service | Description |
|--------|--------|------------|
| Compute | EC2 | Virtual servers |
| Storage | S3 | Object storage |
| Database | RDS | Managed relational DB |
| Network | VPC | Private network |
| Scaling | Auto Scaling | Automatic scaling |
| Load Balance | ELB | Traffic distribution |

You don’t build everything from scratch.  
You **combine services to build systems**.
