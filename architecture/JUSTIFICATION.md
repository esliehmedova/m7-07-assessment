# Architecture Justification

## Why Microservices?

A microservices architecture is chosen to ensure independent scaling of components. Recommendation serving is latency-sensitive, while feature processing and model management can scale independently.

## Why Feature Store?

Real-time personalization requires consistent feature retrieval across training and inference. A feature store prevents training-serving skew and ensures reproducibility.

## Why Kubernetes?

Kubernetes provides:

* Horizontal autoscaling for traffic spikes
* Rolling updates for safe deployment
* Isolation between services

## Why Canary Deployment?

Since model performance directly affects user experience, canary deployment reduces risk by gradually exposing new models to traffic before full rollout.

## Why MLflow?

MLflow provides:

* Model versioning
* Experiment tracking
* Controlled promotion between environments

## Trade-offs

* Slight infrastructure complexity is introduced
* Increased operational overhead is offset by production reliability