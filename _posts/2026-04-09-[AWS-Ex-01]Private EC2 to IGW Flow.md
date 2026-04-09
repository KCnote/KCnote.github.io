---
layout: post
title: "01. Private EC2 → NAT Gateway → IGW (IPv4 Flow)"
date: 2026-04-09 00:00:00 +0900
author: kang
categories: [AWS, AWS - Example]
tags: [AWS, AWS - Example]
pin: false
math: true
mermaid: true
---

# <b>Private EC2 → NAT Gateway → IGW (IPv4 Flow)</b>

---

### <b>Prerequisites</b>
    - EC2
    - NAT Gateway
    - IGW
    - Subnet
    - Route Table

---

## <b>1. What we need</b>

- Private EC2 (No Public IP)
- Private Route Table
- NAT Gateway (Located in Public Subnet)
- Public Route Table
- Internet Gateway (IGW)

## <b>2. Example</b>

#### <b> Step 1 — Request from Private EC2 </b>

Private EC2 (`10.0.2.15`) → External server (`8.8.8.8`)

Source:      10.0.2.15  
Destination: 8.8.8.8  

#### <b>  Step 2 — Private Route Table </b>

10.0.0.0/16 → local  
0.0.0.0/0   → NAT Gateway  

👉 External traffic is routed to NAT Gateway

#### <b> Step 3 — NAT Gateway (Core) </b>

Performs IP translation (NAT)

Before:  
  Source: 10.0.2.15  

After:  
  Source: 3.25.100.10 (Elastic IP)  

#### <b> Step 4 — Public Route Table </b>

10.0.0.0/16 → local  
0.0.0.0/0   → Internet Gateway  

👉 Forwarded to IGW

#### <b> Step 5 — Internet Gateway → Internet </b>

Traffic is sent to external destination (`8.8.8.8`)

#### <b> Step 6 — Response Flow </b>

Internet (8.8.8.8)  
→ IGW  
→ NAT Gateway  
→ Private EC2 (10.0.2.15)  

## <b>3. Full Flow (One Line)</b>

!["aws-ex01-01"](/assets/img/develop/aws-ex01-01.png)

Private EC2 (10.0.2.15)  
→ Private Route Table (0.0.0.0/0 → NAT)  
→ NAT Gateway (translated to 3.25.100.10)  
→ Public Route Table (0.0.0.0/0 → IGW)  
→ Internet Gateway  
→ Internet  

## <b> 4. Why NAT Gateway is Needed? </b>

### 🔸 Maintain Private Subnet
- No Public IP required
- Improved security

### 🔸 Outbound Only
- Can send requests to Internet
- Cannot receive inbound connections

### 🔸 IP Masking
- Internal IP is hidden
- Only Elastic IP is visible externally
