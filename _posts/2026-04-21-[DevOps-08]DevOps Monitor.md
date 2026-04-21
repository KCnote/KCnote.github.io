---
layout: post
title: "07. DevOps about Monitor"
date: 2026-04-21 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - DevOps]
tags: [Deploy, Deploy - DevOps]
pin: false
math: true
mermaid: true
---

# <b>DevOps about Monitor</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `Monitor` in DevOps?</b>

The **Monitor phase** is where you continuously observe the system to understand its behavior in production.

Monitoring answers a critical question:

> “Is the system actually running as expected right now?”

> Monitor is the phase where system metrics, logs, and behavior are continuously observed to detect issues, ensure performance, and maintain reliability.

## <b>2. Why Monitoring Matters</b>

Even a perfectly built and deployed system can fail in production.

##### ❌ Without monitoring

* issues go unnoticed
* performance degradation is invisible
* crashes are detected too late
* debugging becomes difficult
* users experience problems first

##### ✔ With monitoring

* issues are detected early
* performance is visible
* alerts can trigger automatically
* root cause analysis becomes easier

## <b>3. Goals of the Monitor Phase</b>

Monitoring ensures:

1. the system is alive (health)
2. performance meets expectations
3. resources are not exhausted
4. errors are visible
5. abnormal behavior is detected early

## <b>4. What to Monitor</b>

#### <b>4-1. Latency (Critical)</b>

Time taken per frame

```text
Target: < 10 ms per frame
```

If latency increases → performance drift

#### <b>4-2. Throughput (FPS)</b>

How many frames processed per second

```text
Target: > 100 FPS
```

If FPS drops → system overloaded

#### <b>4-3. CPU Usage</b>

```text
High CPU → bottleneck or overload
```

#### <b>4-4. Memory Usage</b>

```text
Memory increasing → memory leak
```

#### <b>4-5. Queue Size</b>

```text
Queue growing → processing slower than input
```

Early sign of failure

#### <b>4-6. Error Logs</b>

```text
Frame processing failed
Invalid input
```

Detect correctness issues

## <b>5. Types of Monitoring Data</b>

#### ✔ Metrics

Numerical data

* latency
* CPU
* memory
* FPS

#### ✔ Logs

Text-based records

```text
[INFO] Frame processed
[ERROR] Failed to decode image
```

#### ✔ Traces (advanced)

Track request flow through system

## <b>6. Monitoring Tools</b>

Common tools:

* Prometheus → metrics collection
* Grafana → dashboards
* Nagios → alerts

## <b>7. Monitoring Flow in DevOps</b>

```tex
System running
   ↓
Metrics collected
   ↓
Data visualized
   ↓
Alerts triggered if abnormal
```

#### Alerts (Very Important)

Monitoring is useless without alerts.

##### Example conditions

```text
Latency > 20ms
Queue size > threshold
Memory continuously increasing
```

###### Trigger:

* email
* Slack
* pager


#### Example Monitoring Scenario

##### Normal

```text
Latency: 5ms
FPS: 100
Memory: stable
Queue: small
```

##### Problem (early warning)

```text
Latency: 8ms → 12ms → 20ms
Queue: increasing
```

Action needed before crash

## <b>8. Common Issues Detected by Monitoring</b>

#### ❌ Performance drift

Gradual slowdown over time

#### ❌ Memory leak

Memory usage continuously increases

#### ❌ Queue overflow

Backlog grows → latency explosion

#### ❌ CPU saturation

System cannot keep up

#### ❌ Silent failure

System runs but produces wrong results
