---
layout: post
title: "09. Public/Private EC2"
date: 2026-04-08 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Public/Private EC2</b>

---

### <b>Prerequisites</b>


---

## <b>1. Differnece between Public and Private EC2</b>

| Feature         | Public EC2        | Private EC2  |
| --------------- | ----------------- | ------------ |
| Public IP       | Yes               | No           |
| Internet Access | Direct            | Via NAT      |
| Inbound Access  | Allowed           | Not allowed  |
| Subnet Type     | Public            | Private      |
| Use Case        | Web / Entry point | Backend / DB |

#### <b>1-1. Public EC2</b>

* Has a **public IP address**
* Exists in a **public subnet**
* Has a route to **Internet Gateway (IGW)**

```
EC2 → Route Table → IGW → Internet
Internet → IGW → EC2
```

#### <b>1-2. Private EC2</b>

* Has **no public IP**
* Exists in a **private subnet**
* No direct route to IGW

```
EC2 → Route Table → NAT Gateway → IGW → Internet
```

👉 Outbound only

## <b>2. Public EC2</b>

!["aws-ec2-pp0"](/assets/img/develop/aws-ec2-pp0.png)

## <b>3. Private EC2</b>

!["aws-ec2-pp1"](/assets/img/develop/aws-ec2-pp1.png)
