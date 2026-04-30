---
layout: post
title: "29. Glue,Lambda and EventBridge"
date: 2026-04-30 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Glue,Lambda and EventBridge</b>

---

### <b>Prerequisites</b>

    S3

---

## <b>1. Glue,Lambda and EventBridge</b>

When building serverless setting, we use lambda. But lambda is event-driven when something happens and lambda just limited to a maximum execution time of 15 mins. Futhermore if we want to use parallelization for performance like image-preprocessing, we can utilize AWS Glue system

Order:

```
Run Glue
Automatically Call EventBridge
Automatically Call Lambda
```

## <b>2. Process</b>

#### <b>2-1. Create Lambda</b>

##### Deploy Code

```python
import boto3

glue = boto3.client("glue")

def lambda_handler(event, context):
    print("S3 trigger:", event)

    response = glue.start_job_run(
        JobName="invert-images-test"
    )

    print("Glue started:", response)
```

#### <b>2-2. Create Glue</b>

!["aws-glue00"](../assets/img/develop/aws-glue00.png)

The python file that i already made is uploaded in glue script.

##### Add S3 Policy Approach on Glue IAM roles

!["aws-glue01"](../assets/img/develop/aws-glue01.png)

Even if the S3 bucket has not been created yet or is not explicitly connected, we still need to grant permissions in advance because AWS Glue executes jobs in a distributed environment where resources are dynamically accessed at runtime. Without proper permissions, the job may fail when it attempts to read from or write to S3.

```json
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject",
    "s3:ListBucket"
  ],
  "Resource": [
    "arn:aws:s3:::aws-glue-assets-337164669284-ap-southeast-2",
    "arn:aws:s3:::aws-glue-assets-337164669284-ap-southeast-2/*"
  ]
}
```

!["aws-glue02"](../assets/img/develop/aws-glue02.png)

#### <b>2-3. Create EventBridge and Connect between Lambda and Glue</b>

!["aws-glue03"](../assets/img/develop/aws-glue03.png)
!["aws-glue04"](../assets/img/develop/aws-glue04.png)
!["aws-glue05"](../assets/img/develop/aws-glue05.png)
!["aws-glue06"](../assets/img/develop/aws-glue06.png)
!["aws-glue07"](../assets/img/develop/aws-glue07.png)

Result:

##### Condition1: trigger condition from specific glue to EventBridge

!["aws-glue08"](../assets/img/develop/aws-glue08.png)

##### Condition2: trigger condition from EventBridge to Lambda 

!["aws-glue09"](../assets/img/develop/aws-glue09.png)

#### <b>2-4. Run Result</b>

##### Start Running Glue: SUCCEEDED

!["aws-glue10"](../assets/img/develop/aws-glue10.png)

##### Trigger EventBridge

!["aws-glue11"](../assets/img/develop/aws-glue11.png)

##### Trigger Lambda

!["aws-glue12"](../assets/img/develop/aws-glue12.png)
