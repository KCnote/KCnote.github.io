---
layout: post
title: "AI Model from deploy to EC2 Server with SageMaker"
date: 2026-04-30 00:00:00 +0900
author: kang
categories: [AWS, AWS - Fundamental]
tags: [AWS, AWS - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>AI Model from deploy to EC2 Server with SageMaker</b>

---

### <b>Prerequisites</b>

    Deploy of endpoint from SageMaker 

---

## <b>1. Connection with endpoint and EC2</b>

#### <b>1-1. Check endpoint exist</b>

```bash
aws sagemaker describe-endpoint \
  --endpoint-name mnist-cnn-endpoint \
  --query EndpointStatus
```

Result: `InService`

#### <b>1-1. Create EC2</b>

- Key pair(login)
- VPC, public subnet
- Security Group 80/443/8000 allowance
- Add IAM rule on EC2 with InvokeEndpoint

```json
{
  "Effect": "Allow",
  "Action": "sagemaker:InvokeEndpoint",
  "Resource": "arn:aws:sagemaker:ap-southeast-2:337164669284:endpoint/mnist-cnn-endpoint"
}
```

#### <b>1-2. Connect EC2</b>

##### local bash1

```bash
cd where-is-path-at-pem
ssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>

--> ssh -i ec2-mnist-server.pem ubuntu@3.27.158.163

sudo apt update
sudo apt install -y python3-venv

python3 -m venv venv
source venv/bin/activate

pip install fastapi uvicorn boto3 pillow numpy python-multipart
```

##### another local bash2

```bash
cd where-is-path-at-pem-and-api-file
scp -i C:\\pair_key.pem api.py ubuntu@{EC2_PUBLIC_IP}:~

--> scp -i ec2-mnist-server.pem api.py ubuntu@3.27.158.163:~
```

> api.py

```python
import io
import json
import boto3
import numpy as np
from PIL import Image

from fastapi import FastAPI, UploadFile, File
from fastapi.responses import HTMLResponse

app = FastAPI()

# 🔥 설정
REGION = "ap-southeast-2"
ENDPOINT_NAME = "mnist-cnn-endpoint"

runtime = boto3.client("sagemaker-runtime", region_name=REGION)


# 🔥 이미지 전처리 (MNIST)
def preprocess_image(image_bytes):
    image = Image.open(io.BytesIO(image_bytes)).convert("L")
    image = image.resize((28, 28))

    array = np.array(image).astype("float32") / 255.0
    array = (array - 0.1307) / 0.3081

    array = array.reshape(1, 1, 28, 28)

    return array


# 🔥 UI 페이지
@app.get("/", response_class=HTMLResponse)
def home():
    return """
    <!DOCTYPE html>
    <html>
    <head>
        <title>MNIST Classifier</title>
    </head>
    <body>
        <h1>MNIST CNN Classifier</h1>

        <input type="file" id="fileInput" accept="image/*">
        <button onclick="predict()">Predict</button>

        <h2 id="result"></h2>

        <script>
            async function predict() {
                const fileInput = document.getElementById("fileInput");
                const file = fileInput.files[0];

                if (!file) {
                    alert("Please select an image.");
                    return;
                }

                const formData = new FormData();
                formData.append("file", file);

                const response = await fetch("/predict", {
                    method: "POST",
                    body: formData
                });

                const data = await response.json();

                document.getElementById("result").innerText =
                    "Prediction: " + data.prediction[0];
            }
        </script>
    </body>
    </html>
    """


# 🔥 헬스 체크
@app.get("/health")
def health():
    return {"status": "ok"}


# 🔥 예측 API
@app.post("/predict")
async def predict(file: UploadFile = File(...)):
    image_bytes = await file.read()

    image_array = preprocess_image(image_bytes)

    payload = {
        "inputs": image_array.tolist()
    }

    response = runtime.invoke_endpoint(
        EndpointName=ENDPOINT_NAME,
        ContentType="application/json",
        Accept="application/json",
        Body=json.dumps(payload)
    )

    result = json.loads(response["Body"].read().decode("utf-8"))

    return result
```

##### local bash1

```bash
ls
uvicorn api:app --host 0.0.0.0 --port 8000
```

#### <b>1-3. Connect the server</b>

##### Brower

```
http://<EC2_PUBLIC_IP>:8000/

--> http://3.27.158.163:8000/
```

!["aws-sa01"](../assets/img/develop/aws-sa04-01.png)
!["aws-sa02"](../assets/img/develop/aws-sa04-02.png)

