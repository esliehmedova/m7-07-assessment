# MLOps Lifecycle — Recommendation System

## Overview

The lifecycle describes how recommendation models are trained, validated, registered, and deployed into production. It ensures reproducibility, version control, and safe promotion of models across environments.

---

## 1. Data Collection

* User interaction data collected from mobile app:

  * clicks
  * views
  * purchases
* Data stored in data lake (batch + streaming)
* Retention window: 30 days rolling history

---

## 2. Feature Engineering

* User behavioral features:

  * last 30-day purchase frequency
  * category affinity
  * session duration

* Item features:

  * price
  * category embedding
  * popularity score

* Features stored in:

  * Offline store (training)
  * Online feature store (serving)

---

## 3. Model Training

* Training runs scheduled daily
* Candidate models:

  * collaborative filtering
  * ranking model (lightweight neural net)
* Training tracked in MLflow experiment runs

---

## 4. Model Evaluation

Each model is evaluated on:

* NDCG@10
* Precision@10
* Latency simulation tests

Minimum thresholds:

* NDCG@10 > baseline + 5%
* p95 inference time < 120ms (simulated)

---

## 5. Model Registry (MLflow)

Models are stored with:

* model name: `recommendation-ranker`
* version number: auto-increment
* metadata:

  * training dataset version
  * feature set version
  * evaluation metrics

---

## 6. Promotion Flow

1. Model → Staging
2. Shadow deployment (no user impact)
3. Canary release (10% traffic)
4. Full production rollout

Approval required from:

* ML engineer
* Product owner

---

## 7. Rollback Strategy

If performance drops:

* automatic rollback triggered if:

  * CTR drops > 5%
  * latency exceeds 150ms p95
* fallback to previous stable model version

---

## Key Principle

> No model goes directly to production without controlled evaluation, versioning, and gradual rollout.
