---
layout: post
title: "01. CNN MNIST"
date: 2026-02-25 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - PyTorch]
tags: [Artificial Intelligence, Artificial Intelligence - PyTorch]
pin: false
math: true
mermaid: true
---

---
# <b>CNN and PyTorch</b>
---

## <b>1. Overall Pipeline of Model Implementation</b>

A typical PyTorch workflow:

```
Dataset → DataLoader → Model → Loss → Optimizer → Train → Evaluate
```

#### <b>1-1. Device Setup</b>

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```

- Use GPU if available, otherwise CPU  

```python
print(torch.cuda.is_available())
print(torch.cuda.device_count())
```

## <b>2. Dataset</b>

```python
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5),(0.5,))
])
```

| Step | Description |
|------|------------|
| ToTensor | Converts image to tensor (0~1) |
| Normalize | Stabilizes training |

Normalization is critical for CNN convergence  

```python
from torchvision import datasets, transforms

train_dataset = datasets.MNIST(root='./data', train=True, transform=transform, download=True)
test_dataset = datasets.MNIST(root='./data', train=False, transform=transform)
```

#### <b>2-1. DataLoader</b>

```python
from torch.utils.data import DataLoader

train_loader = DataLoader(train_dataset, batch_size=64, shuffle=True)
```

| Parameter | Meaning |
|----------|--------|
| batch_size | samples per iteration |
| shuffle | improves generalization |
| num_workers | parallel data loading |

- Larger batch → better GPU utilization  
- Increase `num_workers` → reduce IO bottleneck  

## <b>3. CNN</b>

```python
class CNN(nn.Module):
    def __init__(self):
        super(CNN, self).__init__()

        self.conv1 = nn.Conv2d(1, 32, kernel_size=3)
        self.conv2 = nn.Conv2d(32, 64, kernel_size=3)
        self.pool = nn.MaxPool2d(2,2)

        self.fc1 = nn.Linear(64*12*12, 128)
        self.fc2 = nn.Linear(128, 10)

        self.relu = nn.ReLU()
```

#### <b>3-1. Convolution Layers</b>

```python
nn.Conv2d(in_channel, out_channel, kernel_size,
            stride=1, padding=0, dilation=1, groups=1,
            bias=True, padding_mode='zeros')
```

##### <b>in_channels / out_channels</b>

```python
nn.Conv2d(1, 32, 3)
```

- `in_channels`: input feature channels (e.g., grayscale=1, RGB=3)
- `out_channels`: number of filters (feature maps)

More channels = more expressive power, but higher compute/memory

##### <b>kernel_size</b>

```python
kernel_size=3
```

- Size of the convolution filter (e.g., 3x3, 5x5)

##### <b>stride</b>

```python
stride=2
```

- Controls how far the kernel moves

| stride | effect |
|--------|--------|
| 1 | detailed features |
| 2 | downsampling |

- Often replaces pooling in modern architectures

##### <b>padding</b>

```python
padding=1
```

- Adds zeros around input

##### <b>dilation</b>

```python
dilation=2
```

- Expands kernel spacing

Example:

```
3x3 kernel → behaves like larger receptive field
```

- semantic segmentation
- context-aware models

##### <b>groups</b>

```python
groups=1
```

Controls channel connectivity

| groups | behavior |
|--------|---------|
| 1 | normal convolution |
| in_channels | depthwise convolution |

- MobileNet
- efficient models

##### <b>bias</b>

```python
bias=True
```

- Adds bias term (ax + b, b is bias)

Often disabled when using BatchNorm:

```python
bias=False
```

##### Output Size Formula

Understanding output shape is critical.

```
Output = (W - K + 2P) / S + 1
```

- W = input size  
- K = kernel size  
- P = padding  
- S = stride  

#### <b>3-2. Pooling</b>

```python
self.pool = nn.MaxPool2d(2,2)
```

- Downsampling  
- Reduces computation  
- Adds translation invariance  

#### <b>3-3. Fully Connected</b>

```python
self.fc1 = nn.Linear(64*12*12, 128)
```

### Shape Flow

```
28x28
→ conv1 → 26x26
→ conv2 → 24x24
→ pool → 12x12
→ flatten → 64 * 12 * 12
```

#### <b>3-4. Activation</b>

```python
self.relu = nn.ReLU()
```

- Adds non-linearity  

## <b>4. Forward Pass</b>

```python
def forward(self, x):
    x = self.relu(self.conv1(x))
    x = self.relu(self.conv2(x))
    x = self.pool(x)
    x = x.view(x.size(0), -1)
    x = self.relu(self.fc1(x))
    x = self.fc2(x)
    return x
```

```
Conv → ReLU → Conv → ReLU → Pool → Flatten → FC → Output
```

## <b>5. Loss & Optimizer</b>

```python
criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)
```

- CrossEntropy = Softmax + Log + NLL  

| Optimizer | Use Case |
|----------|--------|
| SGD | stable |
| Adam | fast |
| AdamW | better generalization |

## <b>6. Training Loop</b>

```python
for images, labels in train_loader:
    optimizer.zero_grad() // initalize gradient
    outputs = model(images)
    loss = criterion(outputs, labels) // define loss function
    loss.backward() // backpropagation just calculation
    optimizer.step() // update weights
```

```
forward → loss → backward → update
```

#### Update weights at once included serveral train backpropagation

```python
optimizer.zero_grad()

for i, (x, y) in enumerate(loader):
    output = model(x)
    loss = criterion(output, y)

    loss.backward()

    if (i+1) % 4 == 0:
        optimizer.step()
        optimizer.zero_grad()
        
```

#### Update weights after clipping

```python
loss.backward()

torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)

optimizer.step()
```

#### Update weights with combining multi loss training

```cpp
loss1 = criterion1(...)
loss2 = criterion2(...)

(loss1 + loss2).backward()

optimizer.step()
```

#### Update weights when fine-tuning

```python
loss.backward()

for name, param in model.named_parameters():
    if "backbone" in name:
        param.grad = None

optimizer.step()
```


## <b>7. Evaluation</b>

```python
model.eval()
```

- disables dropout / batchnorm  

```python
with torch.no_grad():
```

- disables gradient calculation  

```python
_, predicted = torch.max(outputs, 1)
```

- select predicted class  

## <b>8. Accuracy</b>

```python
correct += (predicted == labels).sum().item()
```

## <b>9. Whole Code</b>

```python
import torch
import torch.nn as nn
import torch.optim as optim
from torchvision import datasets, transforms

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

import torch

print("cuda available:", torch.cuda.is_available())
print("device count:", torch.cuda.device_count())

if torch.cuda.is_available():
    print("device name:", torch.cuda.get_device_name(0))

transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.5),(0.5,))])

train_dataset = datasets.MNIST(root='./data', train=True, transform=transform, download=True)
test_dataset = datasets.MNIST(root='./data', train=True, transform=transform)

train_loader = torch.utils.data.DataLoader(train_dataset, batch_size=64, shuffle=True)
test_loader = torch.utils.data.DataLoader(test_dataset, batch_size=64, shuffle=False)

class CNN(nn.Module):
  def __init__(self):
    super(CNN, self).__init__()

    self.conv1 = nn.Conv2d(1,32,kernel_size=3)
    self.conv2 = nn.Conv2d(32,64,kernel_size=3)
    self.pool = nn.MaxPool2d(2,2)
    
    self.fc1 = nn.Linear(64*12*12, 128)
    self.fc2 = nn.Linear(128, 10)

    self.relu = nn.ReLU()

  def forward(self, x):
    x = self.relu(self.conv1(x))
    x = self.relu(self.conv2(x))
    x = self.pool(x)

    x = x.view(x.size(0), -1)

    x = self.relu(self.fc1(x))
    x = self.fc2(x)
    return x

model = CNN().to(device)

criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr = 0.001)


epochs = 10

for epoch in range(epochs):
  model.train()
  total_loss = 0

  for images, labels in train_loader:
    images, labels = images.to(device), labels.to(device)
    
    optimizer.zero_grad()
    outputs = model(images)
    loss = criterion(outputs, labels)

    loss.backward()
    optimizer.step()

    total_loss += loss.item()

  print(f"Epoch [{epoch+1}/{epochs}], Loss: {total_loss:4f}")

model.eval()
correct = 0
total = 0

with torch.no_grad():
    for images, labels in test_loader:
        images, labels = images.to(device), labels.to(device)

        outputs = model(images)
        _, predicted = torch.max(outputs, 1)

        total += labels.size(0)
        correct += (predicted == labels).sum().item()

print(f"Accuracy: {100 * correct / total:.2f}%")

import matplotlib.pyplot as plt

# 데이터 하나 가져오기
image, label = train_dataset[1]

# tensor → numpy
img = image.numpy().squeeze()

plt.imshow(img, cmap='gray')
plt.title(f"Label: {label}")
plt.show()
```

