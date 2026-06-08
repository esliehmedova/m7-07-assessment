# System Architecture — Personalized Recommendation Platform

```
flowchart LR
    A[Mobile App] --> B[API Gateway]
    B --> C[FastAPI Recommendation Service]
    C --> D[Feature Store]
    C --> E[Model Registry / MLflow]
    C --> F[Model Inference Engine]

    F --> G[Top-N Recommendations]
    G --> A

    C --> H[Monitoring: Prometheus]
    H --> I[Grafana Dashboards]
    H --> J[Alert Manager]
``` 

## Overview

The system is designed to serve real-time personalized product recommendations for a mobile retail application under high traffic conditions (800 RPS peak). It follows a microservices-based architecture deployed on Kubernetes with clear separation between data ingestion, feature engineering, model serving, and monitoring components.

## Core Components

### 1. API Gateway

* Entry point for mobile application requests
* Handles authentication and rate limiting
* Routes traffic to recommendation service

### 2. Recommendation Service (FastAPI)

* Hosts the inference endpoint
* Retrieves user features from Feature Store
* Calls model for ranking predictions
* Returns top-N product recommendations

### 3. Feature Store

* Stores precomputed user features (last 30 days behavior)
* Ensures low-latency feature retrieval
* Supports offline + online sync

### 4. Model Registry (MLflow)

* Stores versioned models
* Controls promotion (staging → production)
* Ensures reproducibility

### 5. Model Serving Layer

* Loads model from object storage (mounted volume)
* Serves inference requests
* Supports A/B testing via request headers

### 6. Monitoring Stack

* Prometheus collects metrics (latency, errors, throughput)
* Grafana dashboards visualize system health
* Alertmanager triggers SLO-based alerts

### 7. CI/CD Pipeline

* GitHub Actions builds Docker images
* Runs tests and security scans
* Deploys to Kubernetes using canary strategy

## Data Flow

1. Mobile app sends request
2. API Gateway forwards to FastAPI service
3. Service fetches user features from Feature Store
4. Model generates ranked recommendations
5. Response returned to user
6. Metrics logged to monitoring system

## Key Design Principles

* Low latency (<120ms p95)
* Horizontal scalability via Kubernetes
* Separation of training and serving
* Model versioning and traceability
* Safe deployment via canary releases
