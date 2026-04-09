---
layout: post
title: "12. Template"
date: 2026-04-09 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Template</b>

---

### <b>Prerequisites</b>


---

## <b>1. Template</b>

!["aws-ec0"](/assets/img/develop/aws-ec0.png)

Launch Template is a key component for managing EC2 instances at scale. It defines **How EC2 instances should be launched**

A Launch Template is:

* A configuration blueprint for EC2
* Used to launch instances consistently

👉 It contains all the settings required to launch an EC2 instance.

```
Launch Template → EC2 Instance
```

```
Template
 ├── AMI (Ubuntu + Docker)
 ├── t3.micro
 ├── Security Group (22, 80)
 └── User-data script
      ↓
EC2 Instances (multiple)
```

##### Without Launch Template:

* Manual configuration required
* Risk of inconsistency
* Difficult to scale

##### With Launch Template:

* Repeatable deployments
* Consistent environments
* Easy scaling
* Launch Templates support versions
* Infrastructure consistency

##### Only AMI Without Template

👉 Network / Security / Setting issue.

## <b>2. Components of Template</b>

* AMI (image)
* Instance type (CPU / memory)
* Key pair
* Security group
* Subnet / network settings
* Storage (EBS)
* User data (startup script)

## <b>3. How to create Template</b>

#### <b>3-1. Search EC2</b>

!["aws-ec0"](/assets/img/develop/aws-ec0.png)

#### <b>3-2. Click Navigation pane → "Launch Templates"</b>

!["aws-temp0"](/assets/img/develop/aws-temp0.png)

#### <b>3-3. Click Button → "Create launch template"</b>

!["aws-temp1"](/assets/img/develop/aws-temp1.png)

#### <b>3-4. Set Environments (Hardware)</b>

!["aws-temp2"](/assets/img/develop/aws-temp2.png)
!["aws-temp3"](/assets/img/develop/aws-temp3.png)

#### <b>3-5. Step 2. Set Environments (Security)</b>

!["aws-temp4"](/assets/img/develop/aws-temp4.png)

#### <b>3-6. Step 3. Set Environments (Storage)</b>

!["aws-temp5"](/assets/img/develop/aws-temp5.png)

#### <b>3-7. Launch Instance</b>

!["aws-temp6"](/assets/img/develop/aws-temp6.png)

## <b>4. Related Concepts</b>

- Components
    - Auto scaling
    - AMI
