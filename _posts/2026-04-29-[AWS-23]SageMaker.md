---
layout: post
title: "23. AWS SageMaker for ML/AI"
date: 2026-04-29 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>AWS SageMaker</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `AWS SageMaker`</b>

AWS SageMaker is a **fully managed machine learning (ML) platform** that allows you to:

- build models  
- train models  
- deploy models  

all in one place, without managing infrastructure

## <b>2. Why `AWS SageMaker` exists</b>

In real-world ML, the problem is not just "making a model".

The real challenges are:

- setting up servers (CPU / GPU)
- scaling training
- deploying models as APIs
- managing versions
- handling traffic

SageMaker solves these problems.

> SageMaker = "ML + Infrastructure Automation"

Instead of this:

    Write model → Setup server → Install dependencies → Train → Deploy API → Maintain server

You do this:

    Write model → SageMaker handles everything else

## <b>3. What You Actually Use It For</b>

#### <b>3-1. Model Training</b>

You can train models using:

- your own code (PyTorch, TensorFlow)
- built-in algorithms
- pre-trained models

without setting up GPU machines manually

#### <b>3-2. Model Deployment</b>

SageMaker can turn your model into:

- a REST API
- real-time prediction service

no need to build backend servers

#### <b>3-3. Scalable Infrastructure</b>

- automatically uses powerful instances
- can scale up/down depending on workload

useful for large datasets or production traffic

#### <b>3-4. Experiment Management</b>

- track training jobs
- manage different model versions

helps when experimenting with multiple models

```mermaid
flowchart LR
    A[Data] --> B[Training]
    B --> C[Model]
    C --> D[Endpoint]
    D --> E[Prediction API]
```

## <b>4. When You Should Use It</b>

#### <b>4-1. Good Use Cases</b>

- production ML service
- large-scale training
- cloud-based deployment
- team collaboration

#### <b>4-2. Not Necessary When</b>

- simple experiments
- learning ML basics
- small datasets

## <b>5. How to create</b>

#### <b>5-1. Set Network</b>

SageMaker is usually used on private subnect that can approach resource from air-gapped network for security. So we should build VPC, Subnet, NAT, IGW for the private network.

If it is not set, some aws servies like s3 can be approached

```
[SageMaker Studio]
        ↓
[Private Subnet]
        ↓
(NAT Gateway)
        ↓
Internet
```

#### How to build network in AWS 

![img1](../assets/img/develop/aws-sa01-01.png)
![img2](../assets/img/develop/aws-sa01-02.png)
![img3](../assets/img/develop/aws-sa01-03.png)
![img4](../assets/img/develop/aws-sa01-04.png)
![img5](../assets/img/develop/aws-sa01-05.png)

----------------------------

- How to build VPC    :  [VPC](2026-04-08-[AWS-04]VPC.md)
- How to build Subnet :  [Subnet](2026-04-08-[AWS-05]Subnet.md)
- How to build IGW    :  [Internet Gateway](<2026-04-08-[AWS-06]Internet Gateway.md>)
- How to build Route  :  [Route Table](<2026-04-08-[AWS-07]Route Table.md>)
- How to build NAT    :  [NAT Gateway](<2026-04-08-[AWS-08]NAT Gateway.md>)

----------------------------

#### <b>5-2. Search SageMaker</b>

![aws-sagemaker-01](../assets/img/develop/aws-sa01-06.png)

#### <b>5-3. Click Navigation pane → "Domains"</b>

![aws-sagemaker-02](../assets/img/develop/aws-sa01-07.png)

#### <b>5-4. Click Button → "Create Domain"</b>

![aws-sagemaker-03](../assets/img/develop/aws-sa01-08.png)

#### <b>5-5. Create an Amazon SageMaker Unified Studio domain</b>

![aws-sagemaker-04](../assets/img/develop/aws-sa01-09.png)
![aws-sagemaker-05](../assets/img/develop/aws-sa01-10.png)
![aws-sagemaker-06](../assets/img/develop/aws-sa01-11.png)

#### <b>5-6. Enter Domain management</b>

![aws-sagemaker-07](../assets/img/develop/aws-sa01-12.png)

#### <b>5-7. Create Project on Domain</b>

![aws-sagemaker-08](../assets/img/develop/aws-sa01-13.png)
![aws-sagemaker-09](../assets/img/develop/aws-sa01-14.png)
![aws-sagemaker-10](../assets/img/develop/aws-sa01-15.png)

#### <b>5-8. Connect Project</b>

![aws-sagemaker-11](../assets/img/develop/aws-sa01-16.png)
![aws-sagemaker-12](../assets/img/develop/aws-sa01-17.png)

#### <b>5-9. Create Editor VS Code</b>

If you want to create VS Code Editor, you should use instance over 8GB RAM without `t` instance type.

![aws-sagemaker-13](../assets/img/develop/aws-sa01-18.png)
![aws-sagemaker-14](../assets/img/develop/aws-sa01-19.png)
![aws-sagemaker-15](../assets/img/develop/aws-sa01-20.png)

![aws-sagemaker-16](../assets/img/develop/aws-sa01-21.png)
![aws-sagemaker-17](../assets/img/develop/aws-sa01-22.png)
![aws-sagemaker-18](../assets/img/develop/aws-sa01-23.png)
