---
layout: post
title: "02. RNN, LSTM, GRU with Time Series Data"
date: 2026-02-25 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - PyTorch]
tags: [Artificial Intelligence, Artificial Intelligence - PyTorch]
pin: false
math: true
mermaid: true
---

---
# <b>RNN with Time Series Data and PyTorch</b>
---

## <b>1. Overall Pipeline of Model Implementation</b>

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

We generate synthetic time-series data:

- Normal signal → sine wave + small noise  
- Anomaly signal → sine wave + spike + noise  

```python
def generate_data(num_samples=1000, seq_len=50):
```

```python
import numpy as np

def generate_data(num_samples=1000, seq_len=50):
    X = []
    y = []

    for _ in range(num_samples):
        t = np.linspace(0, 10, seq_len)

        if np.random.rand() > 0.5:
            signal = np.sin(t) + np.random.normal(0, 0.1, seq_len)
            label = 0  # normal
        else:
            signal = np.sin(t)
            spike = np.random.randint(10, seq_len-10)
            signal[spike:spike+3] += np.random.normal(3, 0.5, 3)
            signal += np.random.normal(0, 0.3, seq_len)
            label = 1  # anomaly

        X.append(signal)
        y.append(label)

    return np.array(X), np.array(y)

seq_len = 50
X, y = generate_data(2000, seq_len)
```

##### Check Data

```python
import matplotlib.pyplot as plt

normal_idx = np.where(y == 0)[0][0]
plt.plot(X[normal_idx])
plt.title("Normal Signal")
plt.show()

anomaly_idx = np.where(y == 1)[0][0]
plt.plot(X[anomaly_idx])
plt.title("Anomaly Signal")
plt.show()
```

#### Data Structure

After preprocessing:

```python
X_train.shape = (batch, seq_len, input_size)
```

```
(1600, 50, 1)
```

- batch = number of samples  
- seq_len = time steps  
- input_size = feature per time step  

##### Data Convert

```python
X_train = torch.FloatTensor(X_train).unsqueeze(-1)
```

Converts:

```
(1600, 50) → (1600, 50, 1)
```

Required for RNN input format

##### Divide Train/Test

```python
split = int(len(X) * 0.8)
X_train, X_test = X[:split], X[split:]
y_train, y_test = y[:split], y[split:]

X_train = torch.FloatTensor(X_train).unsqueeze(-1).to(device)
X_test = torch.FloatTensor(X_test).unsqueeze(-1).to(device)

y_train = torch.LongTensor(y_train).to(device)
y_test = torch.LongTensor(y_test).to(device)
```

## <b>3. RNN</b>

```python

self.rnn = nn.RNN(
    input_size,
    hidden_size,
    num_layers=1,
    nonlinearity='tanh',
    bias=True,
    batch_first=False,
    dropout=0.0,
    bidirectional=False
)
```


##### <b>input_size</b>

```python
input_size = feature size per time step
```

```
( batch, seq_len, 1 ) → input_size = 1
```

##### <b>hidden_size</b>

```python
hidden_size = number of features in hidden state
```

Controls model capacity

- small → fast but weak
- large → powerful but heavy

##### <b>num_layers</b>

```python
num_layers = stacked RNN layers
```

##### <b>nonlinearity</b>

```python
nonlinearity = 'tanh' or 'relu'
```

###### Options

| Type | features |
|------|------|
| tanh | default, stable |
| relu | faster, less stable |

##### <b>bias</b>

```python
bias=True
```

Adds bias term:

```
h = Wx + Uh + b
```

##### <b>batch_first</b>

```python
batch_first=True or False
```

###### False (default)

```
(seq_len, batch, input_size)
```

###### True (recommended)

```
(batch, seq_len, input_size)
```

##### <b>dropout</b>

```python
dropout=0.0
```

Applies dropout **between RNN layers**

###### Important

```text
only works when num_layers > 1
```

##### <b>bidirectional</b>

```python
bidirectional=True
```

runs RNN in both directions:

```
forward + backward
```

###### Example

```
x₁ → x₂ → x₃ → x₄
x₄ → x₃ → x₂ → x₁
```

###### Output size changes

```text
hidden_size → hidden_size * 2
```

##### <b>Output</b>

##### Output Shape

###### Input

```
(batch, seq_len, input_size)
```

###### Output

```
(batch, seq_len, hidden_size)
```

if bidirectional:

```
hidden_size * 2
```

##### <b>Hidden State</b>

```python
h0 = torch.zeros(num_layers, batch, hidden_size)
```

shape depends on:

- num_layers  
- bidirectional  

```python
class RNNModel(nn.Module):
  def __init__(self, input_size=1, hidden_size=64, num_layers=1):
    super(RNNModel, self).__init__()

    self.rnn = nn.RNN(input_size, hidden_size, num_layers, batch_first=True)
    self.fc = nn.Linear(hidden_size, 2)

  def forward(self,x):
    h0 = torch.zeros(1, x.size(0), 64).to(x.device)

    out, _ = self.rnn(x,h0)

    out = out[:,-1,:]
    out = self.fc(out)
    return out

model = RNNModel().to(device)
```


## <b>4. LSTM</b>

```python
self.lstm = nn.LSTM(input_size, hidden_size, num_layers=1, bias=True, batch_first=False, dropout=0.0, bidirectional=False, proj_size=0, device=None, dtype=None)
```

```python
h0, c0 = hidden state + cell state
```

LSTM has **memory cell**

```python
class LSTMModel(nn.Module):
    def __init__(self, input_size=1, hidden_size=64, num_layers=1, num_classes=2):
        super(LSTMModel, self).__init__()

        self.hidden_size = hidden_size
        self.num_layers = num_layers

        self.lstm = nn.LSTM(
            input_size=input_size,
            hidden_size=hidden_size,
            num_layers=num_layers,
            batch_first=True
        )

        self.fc = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        h0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size, device=x.device)
        c0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size, device=x.device)

        out, (hn, cn) = self.lstm(x, (h0, c0))

        out = out[:, -1, :]
        out = self.fc(out)
        return out

model = LSTMModel().to(device)
```

##### Forward

```python
out, (hn, cn) = self.lstm(x, (h0, c0))
out = out[:, -1, :] # (batch_size, seq_len, hidden_dim) -> (batch_size, hidden_dim), last hidden state
```

## <b>5. GRU</b>

```python
self.gru = nn.GRU(input_size, hidden_size, num_layers=1, bias=True, batch_first=False, dropout=0.0, bidirectional=False, device=None, dtype=None)

```

##### Forward

```python
out, hn = self.gru(x, h0)
out = out[:, -1, :]
```

```python
class GRUModel(nn.Module):
    def __init__(self, input_size=1, hidden_size=64, num_layers=1, num_classes=2):
        super(GRUModel, self).__init__()

        self.hidden_size = hidden_size
        self.num_layers = num_layers

        self.gru = nn.GRU(
            input_size=input_size,
            hidden_size=hidden_size,
            num_layers=num_layers,
            batch_first=True
        )

        self.fc = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        h0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size, device=x.device)

        out, hn = self.gru(x, h0)

        out = out[:, -1, :]   # 마지막 시점
        out = self.fc(out)
        return out

model = GRUModel().to(device)
```

## <b>6. Training Loop</b>

```python
outputs = model(X_train)
loss = criterion(outputs, y_train)

optimizer.zero_grad()
loss.backward()
optimizer.step()
```

Standard PyTorch pipeline

##### Loss

```python
criterion = nn.CrossEntropyLoss()
```

Multi-class classification (normal vs anomaly)

##### Evaluation

```python
_, predicted = torch.max(outputs, 1)
accuracy = (predicted == y_test).sum().item() / len(y_test)
```

##### Why Last Time Step?

```python
out = out[:, -1, :]
```

The model compresses the entire sequence into:

```
last hidden state
```

This acts like a summary of the sequence

##### Visualization

The code visualizes predictions:

```python
plt.plot(X_test_cpu[i])
plt.title(f"True: {true_label}, Pred: {pred_label}")
```

Helps understand model behavior


```python
import torch
import torch.nn as nn
import torch.optim as optim
from torchvision import datasets, transforms
import matplotlib.pyplot as plt

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

import torch

print("cuda available:", torch.cuda.is_available())
print("device count:", torch.cuda.device_count())

if torch.cuda.is_available():
    print("device name:", torch.cuda.get_device_name(0))

import numpy as np

def generate_data(num_samples=1000, seq_len=50):
    X = []
    y = []

    for _ in range(num_samples):
        t = np.linspace(0, 10, seq_len)

        if np.random.rand() > 0.5:
            signal = np.sin(t) + np.random.normal(0, 0.1, seq_len)
            label = 0  # normal
        else:
            signal = np.sin(t)
            spike = np.random.randint(10, seq_len-10)
            signal[spike:spike+3] += np.random.normal(3, 0.5, 3)
            signal += np.random.normal(0, 0.3, seq_len)
            label = 1  # anomaly

        X.append(signal)
        y.append(label)

    return np.array(X), np.array(y)

seq_len = 50
X, y = generate_data(2000, seq_len)

normal_idx = np.where(y == 0)[0][0]
plt.plot(X[normal_idx])
plt.title("Normal Signal")
plt.show()

anomaly_idx = np.where(y == 1)[0][0]
plt.plot(X[anomaly_idx])
plt.title("Anomaly Signal")
plt.show()

split = int(len(X) * 0.8)
X_train, X_test = X[:split], X[split:]
y_train, y_test = y[:split], y[split:]

X_train = torch.FloatTensor(X_train).unsqueeze(-1).to(device)
X_test = torch.FloatTensor(X_test).unsqueeze(-1).to(device)

y_train = torch.LongTensor(y_train).to(device)
y_test = torch.LongTensor(y_test).to(device)


class RNNModel(nn.Module):
  def __init__(self, input_size=1, hidden_size=64, num_layers=1):
    super(RNNModel, self).__init__()

    self.rnn = nn.RNN(input_size, hidden_size, num_layers, batch_first=True)
    self.fc = nn.Linear(hidden_size, 2)

  def forward(self,x):
    h0 = torch.zeros(1, x.size(0), 64).to(x.device)

    out, _ = self.rnn(x,h0)

    out = out[:,-1,:]
    out = self.fc(out)
    return out

model = RNNModel().to(device)

class LSTMModel(nn.Module):
    def __init__(self, input_size=1, hidden_size=64, num_layers=1, num_classes=2):
        super(LSTMModel, self).__init__()

        self.hidden_size = hidden_size
        self.num_layers = num_layers

        self.lstm = nn.LSTM(
            input_size=input_size,
            hidden_size=hidden_size,
            num_layers=num_layers,
            batch_first=True
        )

        self.fc = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        h0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size, device=x.device)
        c0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size, device=x.device)

        out, (hn, cn) = self.lstm(x, (h0, c0))

        out = out[:, -1, :]
        out = self.fc(out)
        return out

model = LSTMModel().to(device)

class GRUModel(nn.Module):
    def __init__(self, input_size=1, hidden_size=64, num_layers=1, num_classes=2):
        super(GRUModel, self).__init__()

        self.hidden_size = hidden_size
        self.num_layers = num_layers

        self.gru = nn.GRU(
            input_size=input_size,
            hidden_size=hidden_size,
            num_layers=num_layers,
            batch_first=True
        )

        self.fc = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        h0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size, device=x.device)

        out, hn = self.gru(x, h0)

        out = out[:, -1, :]
        out = self.fc(out)
        return out

model = GRUModel().to(device)

criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)

epochs = 100

for epoch in range(epochs):
  model.train()
  
  outputs = model(X_train)
  loss = criterion(outputs, y_train)

  optimizer.zero_grad()
  loss.backward()
  optimizer.step()

  print(f"Epoch:[{epoch+1}/{epochs}], Loss: {loss.item(): 4f}")

import matplotlib.pyplot as plt

model.eval()

with torch.no_grad():
    outputs = model(X_test)
    _, predicted = torch.max(outputs, 1)


X_test_cpu = X_test.cpu().squeeze(-1).numpy()
y_test_cpu = y_test.cpu().numpy()
predicted_cpu = predicted.cpu().numpy()


num_samples = 8

plt.figure(figsize=(14, 10))

for i in range(num_samples):
    plt.subplot(4, 2, i + 1)
    plt.plot(X_test_cpu[i])

    true_label = y_test_cpu[i]
    pred_label = predicted_cpu[i]

    result = "Correct" if true_label == pred_label else "Wrong"
    plt.title(f"True: {true_label}, Pred: {pred_label} ({result})")
    plt.tight_layout()

plt.show()
```