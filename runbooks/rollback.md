# Rollback Runbook — Recommendation Service

## Purpose

This runbook defines the steps to safely rollback the recommendation system in case of performance degradation, model failure, or SLO violations.

---

## 🚨 Rollback Triggers

Rollback is initiated automatically or manually if any of the following occur:

* p95 latency > 150ms for more than 10 minutes
* error rate > 1%
* CTR drops by more than 5%
* ModelVersionMismatch alert triggers repeatedly
* Canary deployment shows degradation in metrics

---

## 🔄 Rollback Strategy

We use **version-based rollback via MLflow model registry**.

Each production deployment has a known stable version:

* `v_current` → active model
* `v_previous` → last stable model

---

## 🛠️ Manual Rollback Steps

### Step 1 — Identify issue

Check dashboards:

* Grafana latency panel
* error rate panel
* model version distribution

---

### Step 2 — Confirm failing version

Verify:

* which model version is causing degradation
* compare with previous stable version

---

### Step 3 — Rollback deployment

In Kubernetes:

* switch traffic from `v_current` → `v_previous`
* update service deployment config
* restart pods if necessary

---

### Step 4 — Validate system recovery

Confirm:

* p95 latency returns below 120ms
* error rate drops below 0.1%
* traffic stabilizes

---

### Step 5 — Post-mortem (required)

Document:

* root cause
* impacted users
* fix applied
* prevention steps

---

## ⚠️ Important Principle

We never delete models.

We only **re-route traffic to a stable version**.

This ensures:

* reproducibility
* auditability
* safe recovery

---

## Summary

Rollback is fast (<5 minutes) because:

* models are versioned
* deployment is canary-based
* traffic routing is controlled by Kubernetes
