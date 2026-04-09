---
layout: post
title: "02. Internet → ALB → Target Group → ASG → EC2"
date: 2026-04-09 00:00:00 +0900
author: kang
categories: [AWS, AWS - Example]
tags: [AWS, AWS - Example]
pin: false
math: true
mermaid: true
---

# <b>Internet → ALB → Target Group → ASG → EC2</b>

---

### <b>Prerequisites</b>
    - EC2
    - NAT Gateway
    - IGW
    - Subnet
    - Route Table
    - Application Load Balancer (ALB)
    - Target Group
    - Auto Scaling Group (ASG)

---

## <b>1. What we need</b>

- Internet Gateway (IGW)
- Application Load Balancer (ALB)
- Target Group
- Auto Scaling Group (ASG)
- Route Tables

## <b>2. Setup</b>

#### ✔️ Architecture

Internet
→ Internet Gateway (IGW)
→ Public Route Table
→ ALB (Public Subnet)
→ Listener
→ Target Group
→ EC2 (Private Subnet, managed by ASG)

#### ✔️ Network Setup

##### VPC

10.0.0.0/16

##### Subnets

Public Subnet A: 10.0.1.0/24 (ALB)  
Public Subnet B: 10.0.2.0/24 (ALB)  

Private Subnet A: 10.0.11.0/24 (EC2)  
Private Subnet B: 10.0.12.0/24 (EC2)  

#### ✔️ Route Tables

##### Public Route Table

10.0.0.0/16 → local  
0.0.0.0/0   → Internet Gateway  

##### Private Route Table

10.0.0.0/16 → local  
0.0.0.0/0   → NAT Gateway  

(Note: NAT is not used for inbound traffic)

#### ✔️ Initial State

EC2-1: 10.0.11.10 (AZ1)  
EC2-2: 10.0.12.10 (AZ2)  

ASG:
Min: 2  
Desired: 2  
Max: 6  

## <b>2. Example</b>

#### <b> Step 1 — User → Internet → IGW (TCP + HTTPS)</b>

User sends request:

Client IP:        203.0.113.10  
Destination DNS:  myapp.com  
Resolved ALB IP:  3.25.100.50  

#### <b> Step 2 — IGW → Public Route Table → ALB</b>

Packet enters VPC:

Destination: 3.25.100.50 (ALB)

Routing:

10.0.0.0/16 → local  
0.0.0.0/0   → IGW  

#### <b> Step 3 — ALB → Target Group (L7 Routing)</b>

ALB processes request at Layer 7:

/api/* → Target Group A  

### ✔️ Load Distribution

Total Traffic: 120 req/sec  
Instances: 2  

EC2-1: 60 req/sec  
EC2-2: 60 req/sec  

#### <b> Step 4 — Target Group → EC2 (HTTP over VPC)</b>

ALB forwards request:

ALB → 10.0.11.10:80  

Protocol conversion:

Client → ALB: HTTPS (443)  
ALB → EC2: HTTP (80)  

### ✔️ Internal Routing

Source:      10.0.1.50 (ALB private IP)  
Destination: 10.0.11.10 (EC2)  

Route:

10.0.0.0/16 → local  

## ✔️ Initial State

Each EC2 handles 100 req/sec at ~60% CPU  

Total Traffic: 120 req/sec  

EC2-1: 60 req/sec → CPU ~35%  
EC2-2: 60 req/sec → CPU ~35%  

## 🚀 Scaling Scenario

Total Traffic: 360 req/sec  

Before scaling:

EC2-1: 180 req/sec → CPU ~90%  
EC2-2: 180 req/sec → CPU ~90%  

### ✔️ CloudWatch Metrics

CPUUtilization: 90%  
RequestCountPerTarget: 180  

### ✔️ Scaling Policy

CPU > 70% for 3 minutes → +1 EC2  

### ✔️ First Scale-out

EC2-3: 10.0.11.20  

### ✔️ Redistribution

360 ÷ 3 = 120 req/sec  

EC2-1: 120 req/sec → CPU ~70%  
EC2-2: 120 req/sec → CPU ~70%  
EC2-3: 120 req/sec → CPU ~70%  

### ✔️ Second Scale-out

EC2-4: 10.0.12.20  

### ✔️ Final State

360 ÷ 4 = 90 req/sec  

EC2-1: 90 req/sec → CPU ~55%  
EC2-2: 90 req/sec → CPU ~55%  
EC2-3: 90 req/sec → CPU ~55%  
EC2-4: 90 req/sec → CPU ~55%  
