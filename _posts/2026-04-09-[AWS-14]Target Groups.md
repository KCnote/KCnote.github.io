---
layout: post
title: "14. Target Groups"
date: 2026-04-09 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Target Groups</b>

---

### <b>Prerequisites</b>


---

## <b>1. Target Groups</b>

!["aws-ec0"](/assets/img/develop/aws-ec0.png)

Target Group is a core component used with Load Balancers. It defines **Where incoming traffic should be sent**

```
User → Load Balancer → Target Group → EC2
```

A Target Group is:

* A logical group of backend resources
* Used by a Load Balancer to route traffic

👉 Targets can be:

* EC2 instances
* IP addresses
* Lambda functions

Load Balancer does NOT directly send traffic to EC2. Instead, Load Balancer → Target Group → Targets

##### Without Target Group:

* Load Balancer must manage instances directly
* No flexibility
* No grouping

##### With Target Group:

* Logical grouping of instances
* Easy scaling
* Flexible routing
* High availability
* Fault tolerance
* Clean architecture

#### <b>1-1. Relationship with ASG</b>

```
Auto Scaling Group → Target Group → Load Balancer
```

##### ASG automatically:

* Adds instances to Target Group
* Removes terminated instances

#### <b>1-2. Routing Logic</b>

Target Group allows:

* Decoupling Load Balancer and EC2
* Flexible scaling
* Service-based routing

```
User
 ↓
[ Load Balancer ]
   ↓
[ Target Group ]
   ├── EC2 (Instance A)
   ├── EC2 (Instance B)
   └── EC2 (Instance C)
```

Load Balancer uses Target Group to:

* Distribute traffic
* Apply routing rules

```
/api → Target Group A
/web → Target Group B
```

* Path-based routing
* Microservices architecture

##### Target Types

| Type     | Description         |
| -------- | ------------------- |
| Instance | EC2 instance ID     |
| IP       | Private IP address  |
| Lambda   | Serverless function |

## <b>2. Components of Template</b>

#### <b>2-1. Health Check</b>

Target Group performs health checks.

```
GET /health
```

##### Behavior

* Healthy → receives traffic
* Unhealthy → removed from routing

> Only healthy instances receive traffic


## <b>3. How to create Target Groups</b>

#### <b>3-1. Search EC2</b>

!["aws-ec0"](/assets/img/develop/aws-ec0.png)

#### <b>3-2. Click Navigation pane → "Target Groups"</b>

!["aws-tar0"](/assets/img/develop/aws-tar0.png)

#### <b>3-3. Click Button → "Create Auto Scaling group"</b>

!["aws-tar1"](/assets/img/develop/aws-tar1.png)

#### <b>3-4. Step 1. Create target group</b>

!["aws-tar2"](/assets/img/develop/aws-tar2.png)
!["aws-tar3"](/assets/img/develop/aws-tar3.png)

#### <b>3-5. Step 2. Register targets - recommended</b>

!["aws-tar4"](/assets/img/develop/aws-tar4.png)

## <b>4. Related Concepts</b>

- Components
    - Load Balancing
    - Auto Scaling Groups
