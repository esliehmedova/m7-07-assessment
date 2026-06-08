# MLOps System — Personalized Recommendation Platform

## 🧠 Executive Summary

This project designs a production-grade MLOps system for a mobile retail application that serves personalized product recommendations in real time. The system handles up to 800 requests per second with a p95 latency target of 120 ms. It includes feature storage, model versioning, CI/CD automation, canary deployment, monitoring, and rollback mechanisms. The architecture is designed for scalability, observability, and safe model iteration using real-world MLOps practices.

---

## 🏗️ Architecture

The system is built using a microservices architecture deployed on Kubernetes.

Key components:

* API Gateway
* FastAPI Recommendation Service
* Feature Store
* MLflow Model Registry
* Monitoring Stack (Prometheus + Grafana)
* CI/CD Pipeline (GitHub Actions)

📌 Diagram:
See: `architecture/architecture.md`

---

## 📊 Key System Metrics

| Metric              | Value                |
| ------------------- | -------------------- |
| Peak Traffic        | 800 RPS              |
| p95 Latency         | 120 ms               |
| Availability SLO    | 99.9%                |
| Active Pods         | 13                   |
| Model Registry      | MLflow               |
| Deployment Strategy | Canary (90/10 split) |
| Container Size      | ~300–400 MB          |

---

## 📁 Repository Structure

```
architecture/        → System design + ADRs
lifecycle/           → MLOps lifecycle + model registry
container/           → Dockerfile + container strategy
api/                 → OpenAPI contract + examples
serving/             → Capacity plan + SLOs + load testing
cicd/                → CI/CD pipeline (GitHub Actions)
monitoring/          → Alerts and observability
runbooks/            → Rollback procedures
```

---

## 🔁 System Flow

1. Mobile app sends request
2. API Gateway routes to FastAPI service
3. Feature Store retrieves user behavior data
4. Model generates ranked recommendations
5. Response returned to user
6. Metrics sent to monitoring system
7. CI/CD handles safe deployments via canary releases

---

## ❓ Open Questions

* Should feature store move to fully real-time streaming (Kafka-based)?
* Do we need a more advanced ranking model (e.g., transformer-based) under latency constraints?
* Should A/B testing be moved to a dedicated experimentation platform?

---

## 🚀 Design Principles

* Low latency (<120 ms p95)
* High availability (99.9%)
* Safe deployment via canary releases
* Fully versioned model lifecycle
* Observability-first design
