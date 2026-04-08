---
layout: post
title: "08. NAT Gateway"
date: 2026-04-08 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>NAT Gateway</b>

---

### <b>Prerequisites</b>


---

## <b>1. NAT Gateway</b>

!["aws-vpc0"](/assets/img/develop/aws-vpc0.png)


NAT Gateway is an essential component for secure AWS networking. It enables **Outbound internet access for private subnet resources**

NAT Gateway is:

* A managed network service
* Placed in a **public subnet**
* Used by **private subnet resources**

> Private instances → access the internet
> Internet → cannot directly access private instances

Private subnet:

* No route to Internet Gateway
* No public IP
* Not accessible from internet

👉 Problem:

* Cannot install packages
* Cannot call external APIs
* Cannot update systems

👉 Solution:

> NAT Gateway

👉 High availability:

* Create NAT Gateway per AZ


## <b>2. How to work NAT Gateway</b>

!["aws-ngw0"](/assets/img/develop/aws-ngw0.png)

```
Internet
   ↓
[ IGW ]
   ↓
[ Public Subnet ]
   └── NAT Gateway
   ↓
[ Private Subnet ]
   └── EC2 / RDS
```

#### <b>2-1. NAT Gateway in Public Subnet</b>

* Must be placed in a subnet with IGW access

#### <b>2-2. Elastic IP Required</b>

* NAT Gateway must have a public IP

#### <b>2-3. Route Table (Private Subnet)</b>

Example:

```text
Destination: 0.0.0.0/0
Target: NAT Gateway
```

> All outbound traffic goes through NAT Gateway

#### <b>2-4. Security Behavior</b>

> NAT = outbound only

##### ✔️ Allowed

* Private → Internet (outbound)

##### ❌ Not Allowed

* Internet → Private (inbound)

#### <b>2-5. Cost Consideration</b>

* Charged per hour
* Charged per data processed

## <b>3. How to create NAT Gateway</b>

#### <b>3-1. Search VPC</b>

!["aws-vpc0"](/assets/img/develop/aws-vpc0.png)

#### <b>3-2. Click Navigation pane → "NAT gateways"</b>

!["aws-ngw1"](/assets/img/develop/aws-ngw1.png)

#### <b>3-3. Click Button → "Create NAT gateway"</b>

!["aws-ngw2"](/assets/img/develop/aws-ngw2.png)

#### <b>3-4. NAT gateway settings</b>

!["aws-ngw3"](/assets/img/develop/aws-ngw3.png)

#### <b>3-5. Step 1. Create Route tables</b>

!["aws-ngw4"](/assets/img/develop/aws-ngw4.png)
!["aws-ngw5"](/assets/img/develop/aws-ngw5.png)

#### <b>3-6. Step 2. Edit Subnet associations</b>

!["aws-ngw6"](/assets/img/develop/aws-ngw6.png)
!["aws-ngw7"](/assets/img/develop/aws-ngw7.png)

#### <b>3-7. Step 3. Edit Routes</b>

!["aws-ngw8"](/assets/img/develop/aws-ngw8.png)
!["aws-ngw9"](/assets/img/develop/aws-ngw9.png)
!["aws-ngw10"](/assets/img/develop/aws-ngw10.png)

## <b>4. Related Concepts</b>

- Components
    - VPC
    - Subnet
    - Internet Gateway
    - Route Tables

