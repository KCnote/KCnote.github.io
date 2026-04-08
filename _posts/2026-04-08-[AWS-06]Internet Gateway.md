---
layout: post
title: "06. Internet Gateway"
date: 2026-04-08 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Internet Gateway</b>

---

### <b>Prerequisites</b>


---

## <b>1. Internet Gateway</b>

!["aws-vpc0"](/assets/img/develop/aws-vpc0.png)

Internet Gateway is a core component of AWS networking. It enables **Communication between your VPC and the Internet**

An Internet Gateway (IGW) is:

* A gateway attached to a VPC
* A bridge between private AWS network and public internet

👉 Without IGW:

* VPC is private
* EC2 cannot access internet
* External users cannot access EC2

> Your VPC is completely isolated from the internet

* Horizontally scaled (no bottleneck)
* Highly available
* Managed by AWS
* No additional cost

## <b>2. How to work IGW</b>

!["aws-igw0"](/assets/img/develop/aws-igw0.png)

```
Internet
   ↓
[ Internet Gateway ]
   ↓
[ Public Subnet ]
   └── EC2 (Web Server)
   ↓
[ Private Subnet ]
   └── RDS / Backend
```

#### <b>2-1. VPC</b>

Internet Gateway must be attached to VPC. 

#### <b>2-2. Subnet</b>

A subnet is public if it has a route to an Internet Gateway. NOT Private Subnet.

#### <b>2-3. Route Table</b>

Route Table must allow internet traffic. 

 - Example: All outbound traffic goes to the internet

```text
Destination: 0.0.0.0/0
Target: Internet Gateway
```

#### <b>2-4. Public IP</b>

Instance must have Public IP. Public IPv4 or Elastic IP required

#### <b>2-5. Security Group</b>

Security Group must allow traffic

Example:

* Port 22 (SSH)
* Port 80 (HTTP)

## <b>3. How to create IGW</b>

#### <b>3-1. Search VPC</b>

!["aws-vpc0"](/assets/img/develop/aws-vpc0.png)

#### <b>3-2. Click Navigation pane → "Internet gateways"</b>

!["aws-igw1"](/assets/img/develop/aws-igw1.png)

#### <b>3-3. Click Button → "Create internet gateway"</b>

!["aws-igw2"](/assets/img/develop/aws-igw2.png)

#### <b>3-4. Internet gateway settings</b>

!["aws-igw3"](/assets/img/develop/aws-igw3.png)

#### <b>3-5. Attach VPC</b>

!["aws-igw4"](/assets/img/develop/aws-igw4.png)
!["aws-igw5"](/assets/img/develop/aws-igw5.png)

## <b>4. Related Concepts</b>

- Components
    - VPC
    - Public Subnet
    - Router table
    - Nat Gateway

