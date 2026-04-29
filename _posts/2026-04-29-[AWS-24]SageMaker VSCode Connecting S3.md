---
layout: post
title: "24. SageMaker VSCode Connecting S3"
date: 2026-04-29 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>SageMaker VSCode Connecting S3</b>

---

### <b>Prerequisites</b>

---

## <b>1. Why we should set connecting S3</b>

If SageMake should create network with S3, because it is located in private subnet. That's why we're building the network, vpc, subnet, igw, nat network.

![aws-sagemaker-01](../assets/img/develop/aws-sa02-01.png)

If we want to allow to authorize approaching S3, we should create Roles for it.

- STS timeout = Network issue
- S3 AccessDenied = IAM authorization issue

#### <b>1-1. Check role in SageMaker Project</b>

```bash
aws sts get-caller-identity
```

![aws-sagemaker-02](../assets/img/develop/aws-sa02-02.png)

#### <b>1-2. Create S3</b>

How to create S3: https://kcnote.github.io/posts/AWS-18-S3/

In this case, I'll use name "my-sage-bucket-xxxxxxxx-ap-southeast-2-an"

And I make test-folder for test.

#### <b>1-3. Add policy on role</b>

![aws-sagemaker-03](../assets/img/develop/aws-sa02-03.png)
![aws-sagemaker-04](../assets/img/develop/aws-sa02-04.png)
![aws-sagemaker-05](../assets/img/develop/aws-sa02-05.png)
![aws-sagemaker-06](../assets/img/develop/aws-sa02-06.png)

#### <b>1-4. Check whether it is allowed to approach s3 or not</b>

![aws-sagemaker-07](../assets/img/develop/aws-sa02-07.png)