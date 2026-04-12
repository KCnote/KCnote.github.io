---
layout: post
title: "04. Image Converter with Lambda"
date: 2026-04-13 00:00:00 +0900
author: kang
categories: [AWS, AWS - Example]
tags: [AWS, AWS - Example]
pin: false
math: true
mermaid: true
---

# <b>Send Text Message with Lambda</b>

---

### <b>Prerequisites</b>
    - Lambda
    - Python

---

## <b>1. Example</b>

#### <b> Step 1 — Create S3 Buckets </b>

!["aws-ex02-01"](/assets/img/develop/aws-ex02-01.png)

#### <b> Step 2 — Create Lambda </b>

!["aws-ex02-02"](/assets/img/develop/aws-ex02-02.png)

#### <b> Step 3 — Set Lambda Role Permission from IAM  </b>

!["aws-ex02-03"](/assets/img/develop/aws-ex02-03.png)
!["aws-ex02-04"](/assets/img/develop/aws-ex02-04.png)

#### <b> Step 4 — Regist Layer on Lambda Layer Pool  </b>

##### Bring library zip with docker(for Linux)

```bash
docker run -it --rm -v ${PWD}:/var/task public.ecr.aws/lambda/python:3.14 bash
```

```bash
mkdir python
pip install Pillow -t python
exit
```

Make zip file and **MUST KEEP TREE!**

!["aws-ex02-05"](/assets/img/develop/aws-ex02-05.png)

##### Regist library layer on Lambda Layer Pool

!["aws-ex02-06"](/assets/img/develop/aws-ex02-06.png)
!["aws-ex02-07"](/assets/img/develop/aws-ex02-07.png)

#### <b> Step 5 — Save Lambda Trigger  </b>

!["aws-ex02-08"](/assets/img/develop/aws-ex02-08.png)
!["aws-ex02-09"](/assets/img/develop/aws-ex02-09.png)

#### <b> Step 6 — Check Trigger on S3  </b>

!["aws-ex02-10"](/assets/img/develop/aws-ex02-10.png)
!["aws-ex02-11"](/assets/img/develop/aws-ex02-11.png)

#### <b> Step 7 — Set Layer on Lambda function  </b>

!["aws-ex02-12"](/assets/img/develop/aws-ex02-12.png)
!["aws-ex02-13"](/assets/img/develop/aws-ex02-13.png)
!["aws-ex02-14"](/assets/img/develop/aws-ex02-14.png)
!["aws-ex02-15"](/assets/img/develop/aws-ex02-15.png)

#### <b> Step 8 — Write Code and Deploy </b>

!["aws-ex02-16"](/assets/img/develop/aws-ex02-16.png)

```python
import json
import boto3
from io import BytesIO
from PIL import Image

s3 = boto3.client("s3")

def lambda_handler(event, context):
    print("EVENT =", json.dumps(event))

    try:
        record = event["Records"][0]
        input_bucket = record["s3"]["bucket"]["name"]
        input_key = record["s3"]["object"]["key"]

        print("INPUT BUCKET:", input_bucket)
        print("INPUT KEY:", input_key)

        obj = s3.get_object(Bucket=input_bucket, Key=input_key)
        print("S3 GET SUCCESS")

        bmp_data = obj["Body"].read()

        image = Image.open(BytesIO(bmp_data)).convert("RGB")
        print("IMAGE LOAD SUCCESS")

        jpg_buffer = BytesIO()
        image.save(jpg_buffer, format="JPEG")
        jpg_buffer.seek(0)

        output_key = input_key.replace(".bmp", ".jpg")

        OUTPUT_BUCKET = "stevekc-image-jpg"

        print("OUTPUT BUCKET:", OUTPUT_BUCKET)
        print("OUTPUT KEY:", output_key)

        s3.put_object(
            Bucket=OUTPUT_BUCKET,
            Key=output_key,
            Body=jpg_buffer.getvalue(),
            ContentType="image/jpeg"
        )

        print("UPLOAD SUCCESS")

        return {
            "statusCode": 200,
            "body": f"Converted {input_bucket}/{input_key} -> {OUTPUT_BUCKET}/{output_key}"
        }

    except Exception as e:
        print("ERROR:", str(e))
        raise
```

#### <b> Step 9 — Upload S3 for trigger  </b>

##### INPUT S3 : stevekc-image-bmp

* Trigger condition: when file is upload that is set to images/*.bmp

!["aws-ex02-17"](/assets/img/develop/aws-ex02-17.png)
!["aws-ex02-18"](/assets/img/develop/aws-ex02-18.png)

#### <b> Step 10 — Check S3 result  </b>

##### OUTPUT S3 : stevekc-image-jpg

!["aws-ex02-19"](/assets/img/develop/aws-ex02-19.png)

## <b>2. Miscellaneous</b>

#### <b> 2.1 Check Log from CloudWatch  </b>

##### SUCCESS

!["aws-ex02-20"](/assets/img/develop/aws-ex02-20.png)

##### FAIL

!["aws-ex02-21"](/assets/img/develop/aws-ex02-21.png)

#### <b> 2.2 Lambda Test Code  </b>

!["aws-ex02-22"](/assets/img/develop/aws-ex02-22.png)

```json
{
  "Records": [
    {
      "s3": {
        "bucket": {
          "name": "stevekc-image-bmp"
        },
        "object": {
          "key": "now.bmp"
        }
      }
    }
  ]
}
```

