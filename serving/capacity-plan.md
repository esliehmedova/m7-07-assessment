# Capacity Plan — Recommendation Service

## Workload Assumptions

* Peak traffic: 800 RPS
* Average traffic: 400 RPS
* Request type: real-time recommendation
* Payload size: small JSON (<2 KB)

---

## Latency Budget Breakdown

| Component               | Latency    |
| ----------------------- | ---------- |
| API Gateway             | 10 ms      |
| Feature Store Lookup    | 25 ms      |
| Model Inference         | 60 ms      |
| Network + Serialization | 25 ms      |
| **Total p95 target**    | **120 ms** |

---

## Scaling Strategy

We assume:

* 1 FastAPI pod handles ~80 RPS safely under p95 < 120 ms

### Required replicas:

800 RPS / 80 RPS per pod = **10 pods**

Add 30% buffer for spikes:

➡️ **13 pods recommended**

---

## Infrastructure Choice

* Kubernetes cluster (HPA enabled)
* CPU-based autoscaling
* Node type: 4 vCPU / 16 GB RAM

---

## Bottleneck Analysis

### Feature Store

* Must support <25 ms reads
* Cached hot features in Redis layer

### Model Inference

* Lightweight ranking model required
* No deep transformers due to latency constraints

---

## Failure Scenarios

* If Feature Store latency increases → degrade gracefully using cached features
* If model pod fails → traffic rerouted automatically via Kubernetes service discovery

---

## Summary

The system is designed to sustain:

* 800 RPS peak load
* 120 ms p95 latency
* 99.9% availability under autoscaling
