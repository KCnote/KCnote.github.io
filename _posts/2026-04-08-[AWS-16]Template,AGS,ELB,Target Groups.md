---
layout: post
title: "16. How Template, Auto Scaling Group, Elastic Load Balancer and Target Group work together"
date: 2026-04-09 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>How Template, Auto Scaling Group, Elastic Load Balancer and Target Group work together</b>

---

### <b>Prerequisites</b>


---

## <b>1. How to work together</b>

In AWS, these four components work together to build a scalable, highly available architecture.

A **Launch Template** defines how EC2 instances should be created. It includes configuration such as the AMI, instance type, key pair, storage, and optional network settings. Think of it as a blueprint for instances.

An **Auto Scaling Group (ASG)** uses this Launch Template to automatically create and manage EC2 instances. It ensures the desired number of instances is always running, scales out or in based on demand, and replaces unhealthy instances when necessary.

An **Elastic Load Balancer (ELB)** sits in front of these instances and distributes incoming traffic. It receives client requests and forwards them to backend instances based on defined rules (listeners).

A **Target Group** acts as the bridge between the Load Balancer and the EC2 instances. It contains a list of targets (typically EC2 instances created by the ASG) and defines how traffic should be routed to them (protocol, port). It also performs health checks to determine which instances are able to handle requests.

The integration works as follows:

- The Launch Template is used by the ASG to launch EC2 instances
- The ASG is attached to the Load Balancer, enabling automatic registration and deregistration of instances
- The Target Group is registered to the Load Balancer’s listener, defining where traffic should be sent
- The Load Balancer forwards incoming requests to the Target Group, which then routes traffic to healthy EC2 instances
- The Load Balancer performs health checks, and the ASG uses those results to replace unhealthy instances if ELB health checks are enabled

!["aws-comb0"](/assets/img/develop/aws-comb0.png)

## <b>2. What components they should have</b>

!["aws-comb1"](/assets/img/develop/aws-comb1.png)
