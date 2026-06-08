# Container Strategy — Recommendation Service

## Overview

This container packages the FastAPI-based recommendation inference service using a multi-stage Docker build to ensure a minimal and secure production image.

---

## Build Strategy

### Stage 1 — Builder

* Installs dependencies
* Compiles Python packages if needed
* Outputs dependencies to `/install`

### Stage 2 — Runtime

* Uses clean `python:3.11-slim` base image
* Copies only required dependencies
* Copies application code only

---

## Model Handling Strategy

The model is NOT baked into the container image.

Instead:

* Model is fetched from MLflow or object storage at startup
* Enables independent model updates without rebuilding image

---

## Image Optimization

* Multi-stage build reduces final image size
* No dev dependencies in runtime image
* No caching or unnecessary OS packages

---

## Security Considerations

* Minimal base image reduces attack surface
* No root-level operations at runtime
* Dependencies pinned in requirements.txt (recommended)

---

## Estimated Image Size

* Base runtime image: ~120MB
* Dependencies: ~150–250MB
* Final image estimate: ~300–400MB

---

## Runtime

The service runs using:

* FastAPI application
* Uvicorn ASGI server
* Port: 8000
