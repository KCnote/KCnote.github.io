---
layout: post
title: "10. How EC2, VPC, Subnets, Internet Gateway, Route Tables, and NAT Gateway work together"
date: 2026-04-08 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>How EC2, VPC, Subnets, Internet Gateway, Route Tables, and NAT Gateway work together</b>

---

### <b>Prerequisites</b>


---

## <b>1. How to work together</b>

A VPC consists of multiple components that work together to support EC2 instances. It contains subnets that are used to logically separate resources for better management and security. To enable communication with the public internet, an Internet Gateway is attached to the VPC. Route tables are used to control how traffic is routed within the network. For instances in private subnets that require outbound internet access, a NAT Gateway is used.

#### <b>1-1. Process</b>

!["aws-arch5"](/assets/img/develop/aws-arch5.png)

#### <b>1-2. Simple Process</b>

!["aws-arch0"](/assets/img/develop/aws-arch0.png)

## <b>2. How to work at Public EC2</b>

#### <b>2-1. Outbound with request</b>

!["aws-arch1"](/assets/img/develop/aws-arch1.png)

#### <b>2-2. Inbound with response</b>

!["aws-arch2"](/assets/img/develop/aws-arch2.png)

## <b>3. How to work at Private EC2</b>

!["aws-arch3"](/assets/img/develop/aws-arch3.png)

## <b>4. What components they should have</b>

!["aws-arch4"](/assets/img/develop/aws-arch4.png)
