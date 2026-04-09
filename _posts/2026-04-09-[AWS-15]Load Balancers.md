---
layout: post
title: "15. Load Balancers"
date: 2026-04-09 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Load Balancers</b>

---

### <b>Prerequisites</b>


---

## <b>1. Load Balancers</b>

!["aws-ec0"](/assets/img/develop/aws-ec0.png)

Load Balancer is a core component of modern cloud architecture. It provides **Traffic distribution across multiple servers**

A Load Balancer is:

* An entry point for user requests
* A service that distributes traffic to multiple targets

👉 Instead of:

```
User → EC2 (single server)
```

👉 You use:

```
User → Load Balancer → Multiple EC2
```

```
Load Balancer → Target Group ← Auto Scaling Group
```

##### Without Load Balancer:

* Single point of failure
* Cannot scale easily
* Uneven traffic distribution

##### With Load Balancer:

* High availability
* Scalability
* Fault tolerance

```
User → Load Balancer → Target Group → EC2
```

Load Balancer does NOT directly manage EC2. It uses Target Groups

* Removes unhealthy instances
#### <b>1-1. Health Check</b>

Load Balancer uses Target Group health checks:

* Healthy → receive traffic
* Unhealthy → excluded

```
GET /health
```

#### <b>1-2. Concept</b>

##### ✔️ Decoupling

* Load Balancer separates users from backend services
* Backend instances can be replaced without affecting clients

##### ✔️ Scalability

* Works with Auto Scaling Group (ASG)
* Automatically distributes traffic as instances scale in/out

##### ✔️ Fault Tolerance

* Detects unhealthy instances using health checks
* Routes traffic only to healthy targets
* Prevents service disruption when instances fail

## <b>2. Types of Load Balancer</b>

#### <b>2-1. ALB (Application Load Balancer)</b>

* Layer 7 (HTTP/HTTPS)
* Path-based routing
* Host-based routing


```
/api → Service A
/web → Service B
```

#### <b>2-2. NLB (Network Load Balancer)</b>

* Layer 4 (TCP/UDP)
* High performance
* Low latency


#### <b>2-3. GWLB (Gateway Load Balancer)</b>

* Layer 3 / Layer 4
* Used for network security appliances
* Transparent traffic inspection
* Uses GENEVE protocol (port 6081)

👉 Example:

```
Client → GWLB → Firewall (EC2) → GWLB → Destination
```

👉 Use cases:

* Firewall
* Intrusion Detection System (IDS)
* Deep Packet Inspection

#### <b>2-4. CLB (Classic Load Balancer)</b>

* Legacy (not recommended)
* Limited features compared to ALB/NLB


## <b>3. How to create Target Groups</b>

#### <b>3-1. Search EC2</b>

!["aws-ec0"](/assets/img/develop/aws-ec0.png)

#### <b>3-2. Click Navigation pane → "Load Balancers"</b>

!["aws-lb0"](/assets/img/develop/aws-lb0.png)

#### <b>3-3. Click Button → "Create load balancer"</b>

!["aws-lb1"](/assets/img/develop/aws-lb1.png)

#### <b>3-4. Step 1. Select Load balancer types</b>

!["aws-lb2"](/assets/img/develop/aws-lb2.png)

#### <b>3-5. Step 2. Basic Configuration</b>

!["aws-lb3"](/assets/img/develop/aws-lb3.png)

#### <b>3-6. Step 3. Network mapping</b>

!["aws-lb4"](/assets/img/develop/aws-lb4.png)

#### <b>3-7. Step 4. Set Listner</b>

!["aws-lb5"](/assets/img/develop/aws-lb5.png)

#### <b>3-8. Check Security and In/Outbound rules</b>

!["aws-lb6"](/assets/img/develop/aws-lb6.png)
!["aws-lb7"](/assets/img/develop/aws-lb7.png)

## <b>4. Related Concepts</b>

- Components
    - Target Groups
    - Auto Scaling Groups