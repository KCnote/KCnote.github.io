---
layout: post
title: "07. Route Table - Traffic Control"
date: 2026-04-08 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Route Table</b>

---

### <b>Prerequisites</b>


---

## <b>1. Route Table</b>

!["aws-vpc0"](/assets/img/develop/aws-vpc0.png)


Route Table is a core networking component in AWS. It defines **Where network traffic should go**

A Route Table is:

* A set of rules (routes)
* Used to control traffic flow inside a VPC

👉 Think of it as:

> **A navigation system for network traffic**

Each route consists of:

* Destination (CIDR)
* Target (where traffic goes)


## <b>2. How to work Route Table</b>

```text
Destination     Target
10.0.0.0/16     local
0.0.0.0/0       IGW
```

* `10.0.0.0/16 → local`
  → Internal VPC communication

* `0.0.0.0/0 → IGW`
  → Internet traffic goes to Internet Gateway

#### <b>2-1. Subnet</b>

Route Tables are associated with **subnets**. Subnet determines which route table is used.

> Subnet is public or private based on its route table

##### Public Subnet

* Has route:

```text
0.0.0.0/0 → IGW
```

👉 Internet accessible

##### Private Subnet

* No route to IGW

👉 No direct internet access

#### <b>2-2. Route Targets</b>

Common targets:

| Target      | Description                    |
| ----------- | ------------------------------ |
| local       | Internal VPC communication     |
| IGW         | Internet Gateway               |
| NAT Gateway | Private subnet internet access |
| VPC Peering | Communication between VPCs     |

## <b>3. How to create Route Table</b>

#### <b>3-1. Search VPC</b>

!["aws-vpc0"](/assets/img/develop/aws-vpc0.png)

#### <b>3-2. Click Navigation pane → "Route tables"</b>

!["aws-rt0"](/assets/img/develop/aws-rt0.png)

#### <b>3-3. Click Button → "Create route table"</b>

!["aws-rt1"](/assets/img/develop/aws-rt1.png)

#### <b>3-4. Route table settings</b>

!["aws-rt2"](/assets/img/develop/aws-rt2.png)

#### <b>3-5. Step 1. Edit subnet associations</b>

!["aws-rt3"](/assets/img/develop/aws-rt3.png)

#### <b>3-6. Step 2. Add Public subnet</b>

!["aws-rt4"](/assets/img/develop/aws-rt4.png)

#### <b>3-7. Step 3. Edit routes</b>

!["aws-rt5"](/assets/img/develop/aws-rt5.png)

#### <b>3-8. Step 4. Arrange routes</b>

!["aws-rt6"](/assets/img/develop/aws-rt6.png)
!["aws-rt7"](/assets/img/develop/aws-rt7.png)

## <b>4. Related Concepts</b>

- Components
    - VPC
    - Subnet
    - Internet Gateway
    - Nat Gateway

