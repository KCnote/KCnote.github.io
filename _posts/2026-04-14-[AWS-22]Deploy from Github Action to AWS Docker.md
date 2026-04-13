---
layout: post
title: "22. Deploy from Github Action to AWS Docker"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Deploy from Github Action to AWS Docker</b>

---

### <b>Prerequisites</b>
    - Install AWS CLI
    - Docker
    - CI/CD
    - Github Action
    - ECR
    - Lambda

---

## <b>1. Deploy from Github Action to AWS Docker</b>

#### <b> Step 0 — Availiable setting for create docker image   </b>

In this example, I use previous posting.

!["aws-ex03-00"](/assets/img/develop/aws-ex03-00.png)

https://kcnote.github.io/posts/MiniProject-01-CMake+Docker+CICD_01/
https://kcnote.github.io/posts/MiniProject-02-CMake+Docker+CICD_02/
https://kcnote.github.io/posts/MiniProject-02-CMake+Docker+CICD_03/
https://kcnote.github.io/posts/MiniProject-02-CMake+Docker+CICD_04/
https://kcnote.github.io/posts/MiniProject-02-CMake+Docker+CICD_05/

#### <b> Step 1 — Create ECR   </b>

##### From Console

!["aws-ex03-01"](/assets/img/develop/aws-ex03-01.png)
!["aws-ex03-02"](/assets/img/develop/aws-ex03-02.png)
!["aws-ex03-03"](/assets/img/develop/aws-ex03-03.png)

##### From CLI

```bash
aws ecr create-repository --repository-name {ECR.NAME} --region ap-southeast-2
```

##### Permissions - Edit JSON

!["aws-ex03-16"](/assets/img/develop/aws-ex03-16.png)

```json
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "LambdaECRImageRetrievalPolicy",
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": [
        "ecr:BatchGetImage",
        "ecr:GetDownloadUrlForLayer"
      ]
    }
  ]
}
```


#### <b> Step 2 — Initialize Docker on ECR  </b>

!["aws-ex03-04"](/assets/img/develop/aws-ex03-03.png)

##### Login in

```bash
aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin {ECR.URL}
```

##### Initialize Docker Images

```bash
cd deploy-folder
docker buildx build --platform linux/amd64 --provenance=false -t {ECR.URL} --push .
```

##### Confirm push

```bash
aws ecr list-images --repository-name {ECR.NAME} --region ap-southeast-2
```
aws ecr list-images --repository-name deploy/docker --region ap-southeast-2

#### <b> Step 3 — Create Lambda and Add Permissions </b>

!["aws-ex03-05"](/assets/img/develop/aws-ex03-05.png)

##### Permissions policy "AWSLambdaBasicExecutionRole" on IAM

!["aws-ex03-06"](/assets/img/develop/aws-ex03-06.png)

#### <b> Step 4 — For Github Action, Create OIDC Provider on IAM  </b>

##### Connect specific git address

!["aws-ex03-07"](/assets/img/develop/aws-ex03-07.png)
!["aws-ex03-08"](/assets/img/develop/aws-ex03-08.png)

##### Add role

!["aws-ex03-09"](/assets/img/develop/aws-ex03-09.png)
!["aws-ex03-10"](/assets/img/develop/aws-ex03-10.png)
!["aws-ex03-11"](/assets/img/develop/aws-ex03-11.png)
!["aws-ex03-12"](/assets/img/develop/aws-ex03-12.png)
!["aws-ex03-13"](/assets/img/develop/aws-ex03-13.png)
!["aws-ex03-14"](/assets/img/develop/aws-ex03-14.png)

###### Trust relationship
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::337164669284:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                },
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": "repo:KCnote/deploy-practice:ref:refs/heads/main"
                }
            }
        }
    ]
}
```

###### Policy
```bash
+ AmazonEC2ContainerRegistryPowerUser
+ AWSLambda_FullAccess
```

#### <b> Step 5 — Create GitHub Action yml  </b>

```bash
.github/workflows/aws-docker.yml
```

```yml
name: Build and Deploy Lambda Container Image

on:
  push:
    branches:
      - main

permissions:
  id-token: write
  contents: read

env:
  AWS_REGION: ap-southeast-2
  ECR_REPOSITORY: deploy-practice
  LAMBDA_FUNCTION_NAME: deploy-practice-lambda

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: #{Role.ARN}
          aws-region: ${{ env.AWS_REGION }} 

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build, tag, and push image
        env:
          REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          IMAGE_TAG: ${{ github.sha }}
        run: |
          IMAGE_URI=$REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
          docker build -t $IMAGE_URI .
          docker push $IMAGE_URI
          echo "IMAGE_URI=$IMAGE_URI" >> $GITHUB_ENV

      - name: Update Lambda function
        run: |
          aws lambda update-function-code \
            --function-name $LAMBDA_FUNCTION_NAME \
            --image-uri $IMAGE_URI

#AWS_REGION: ap-southeast-2
#ECR_REPOSITORY: deploy/docker
#LAMBDA_FUNCTION_NAME: lambda-git-deploy
#role-to-assume: {Role.ARN}
```

#### <b> Step 6 — Git Push  </b>

```bash
git status
git add .
git commit -m "aws project deploy"
git push
```

!["aws-ex03-19"](/assets/img/develop/aws-ex03-19.png)

##### ECR Status after push

!["aws-ex03-15"](/assets/img/develop/aws-ex03-15.png)

#### <b> Step 7 — Pull image from ECR  </b>

!["aws-ex03-17"](/assets/img/develop/aws-ex03-17.png)
!["aws-ex03-18"](/assets/img/develop/aws-ex03-18.png)
