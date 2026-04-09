---
layout: post
title: "13. Auto Scaling Groups"
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

## <b>1. Auto Scaling Groups</b>

!["aws-ec0"](/assets/img/develop/aws-ec0.png)

Auto Scaling Group is a core service for building scalable systems in AWS. It enables **Automatic scaling and self-healing of EC2 instances**

```
Launch Template → Auto Scaling Group → EC2 Instances
```

Auto Scaling Group (ASG) is:

* A logical group of EC2 instances
* Managed automatically by AWS

👉 It ensures:

* The correct number of instances
* High availability
* Automatic recovery

```
Internet
   ↓
[ Load Balancer ]
   ↓
[ Auto Scaling Group ]
   ├── EC2 (AZ-a)
   ├── EC2 (AZ-b)
   └── EC2 (AZ-c)
```

#### <b>1-1. Behavior</b>

> **ASG maintains system stability automatically**

* If instances < desired → create new ones
* If instances > desired → terminate extra ones
* If instance fails → replace automatically

```text
Instance crashed → ASG detects → new instance launched
```

#### <b>1-2. Scaling Types</b>

##### Manual Scaling

* Change desired capacity manually

##### Dynamic Scaling ⭐

* Based on metrics (CPU, traffic)

👉 Example:

* CPU > 70% → scale out
* CPU < 30% → scale in

##### Scheduled Scaling

* Based on time

👉 Example:

* Increase capacity during business hours

#### <b>1-3. Multi-AZ Deployment</b>

ASG can span multiple Availability Zones:

```
AZ-a → EC2
AZ-b → EC2
AZ-c → EC2
```

👉 Benefits:

* High availability
* Fault tolerance

## <b>2. Components of Template</b>

#### <b>2-1. Launch Template</b>

* Defines how instances are created
* Includes AMI, instance type, network, etc.

#### <b>2-2. Desired Capacity</b>

* Number of instances you want

#### <b>2-3. Minimum / Maximum Size</b>

* Min → lower bound
* Max → upper bound

## <b>3. How to create Template</b>

#### <b>3-1. Search EC2</b>

!["aws-ec0"](/assets/img/develop/aws-ec0.png)

#### <b>3-2. Click Navigation pane → "Auto Scaling Groups"</b>

!["aws-asg0"](/assets/img/develop/aws-aws-asg0.png)

#### <b>3-3. Click Button → "Create Auto Scaling group"</b>

!["aws-asg1"](/assets/img/develop/aws-aws-asg1.png)

#### <b>3-4. Step 1. Choose launch template</b>

!["aws-asg2"](/assets/img/develop/aws-aws-asg2.png)

#### <b>3-5. Step 2. Choose instance launch options </b>

!["aws-asg3"](/assets/img/develop/aws-aws-asg3.png)

#### <b>3-6. Step 3. Integrate with other services - optional</b>

!["aws-asg4"](/assets/img/develop/aws-aws-asg4.png)

1. Load balancing: When we want to apply.
2. Health Check grace period: Standard of normal or not.

#### <b>3-7. Step 4. Configure group size and scaling - optional</b>

!["aws-asg5"](/assets/img/develop/aws-aws-asg5.png)
!["aws-asg6"](/assets/img/develop/aws-aws-asg6.png)

#### <b>3-7. Step 5. Add notifications - optional</b>

!["aws-asg7"](/assets/img/develop/aws-aws-asg7.png)

#### <b>3-8. Step 6. Add tags - optional</b>

!["aws-asg8"](/assets/img/develop/aws-aws-asg8.png)

#### <b>3-9. Check Activity and Current status</b>

!["aws-asg9"](/assets/img/develop/aws-aws-asg9.png)
!["aws-asg10"](/assets/img/develop/aws-aws-asg10.png)

## <b>4. Related Concepts</b>

- Components
    - Load Balancing
    - Target Groups
    - Template

