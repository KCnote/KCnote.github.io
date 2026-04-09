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

!["aws-ec2-0"](/assets/img/develop/aws-ec2-0.png)

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

!["aws-ec2-0"](/assets/img/develop/aws-ec2-0.png)

#### <b>3-2. Click Navigation pane → "Auto Scaling Groups"</b>

!["aws-asg0"](/assets/img/develop/aws-asg0.png)

#### <b>3-3. Click Button → "Create Auto Scaling group"</b>

!["aws-asg1"](/assets/img/develop/aws-asg1.png)

#### <b>3-4. Step 1. Choose launch template</b>

!["aws-asg2"](/assets/img/develop/aws-asg2.png)

#### <b>3-5. Step 2. Choose instance launch options </b>

!["aws-asg3"](/assets/img/develop/aws-asg3.png)

#### <b>3-6. Step 3. Integrate with other services - optional</b>

!["aws-asg4"](/assets/img/develop/aws-asg4.png)

1. Load balancing: When we want to apply.
2. Health Check grace period: Standard of normal or not.

#### <b>3-7. Step 4. Configure group size and scaling - optional</b>

!["aws-asg5"](/assets/img/develop/aws-asg5.png)
!["aws-asg6"](/assets/img/develop/aws-asg6.png)

#### <b>3-7. Step 5. Add notifications - optional</b>

!["aws-asg7"](/assets/img/develop/aws-asg7.png)

#### <b>3-8. Step 6. Add tags - optional</b>

!["aws-asg8"](/assets/img/develop/aws-asg8.png)

#### <b>3-9. Check Activity and Current status</b>

!["aws-asg9"](/assets/img/develop/aws-asg9.png)
!["aws-asg10"](/assets/img/develop/aws-asg10.png)

### <b>4. OPTIONAL</b>

---

#### <b>4-1. Load Balancer Integrations</b>

!["aws-asg11"](/assets/img/develop/aws-asg11.png)
!["aws-asg12"](/assets/img/develop/aws-asg12.png)

#### <b>4-2. Approach DNS with Load Balancing</b>

!["aws-asg13"](/assets/img/develop/aws-asg13.png)

---

#### <b>4-1. Health Check on ASG</b>

!["aws-asg14"](/assets/img/develop/aws-asg14.png)
!["aws-asg15"](/assets/img/develop/aws-asg15.png)

1. Turn on Elastic Load Balancing health checks (Recommended)

Instead of only checking whether the EC2 instance is running, it verifies whether the instance is actually capable of handling incoming traffic.

For example, an EC2 instance may be in a running state, but if the application on it has crashed or is not responding, it will be marked as unhealthy. In this case, the Auto Scaling Group (ASG) will automatically terminate and replace the instance.

Therefore, enabling Elastic Load Balancing health checks is considered a best practice in most production environments.

---

#### <b>4-2. Instance Management</b>

!["aws-asg16"](/assets/img/develop/aws-asg16.png)
!["aws-asg17"](/assets/img/develop/aws-asg17.png)

If the instance become unhealthy, not directly deregistration. According to target groups's deregistration management, few times later it become deregistration.

## <b>5. Related Concepts</b>

- Components
    - Load Balancing
    - Target Groups
    - Template

