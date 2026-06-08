# Load Test Plan — Recommendation System

## Objective

Validate system stability under peak load (800 RPS) while maintaining p95 latency under 120 ms.

---

## Tooling

* k6 (primary load testing tool)
* Locust (optional alternative)
* Prometheus for metrics collection

---

## Test Scenarios

### 1. Baseline Test

* Load: 100 RPS
* Duration: 10 minutes
* Goal: validate stability

### 2. Peak Load Test

* Load: 800 RPS
* Duration: 30 minutes
* Goal: validate SLO compliance

### 3. Spike Test

* Load: 0 → 900 RPS sudden spike
* Duration: 5 minutes
* Goal: test autoscaling response

---

## Success Criteria

* p95 latency ≤ 120 ms
* error rate < 0.1%
* no pod crashes under sustained load
* autoscaling responds within 60 seconds

---

## Observability During Test

* CPU usage per pod
* Feature store latency
* model inference time
* request error rate

---

## Failure Conditions

Test fails if:

* latency exceeds 150 ms for >5 minutes
* error rate exceeds 1%
* system does not recover after spike
