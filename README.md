# Cosmocloud Deploy Helm Chart - Detailed Overview

This Helm chart is designed to deploy a full-stack application, including a backend service, frontend application, and Redis database, on a Kubernetes cluster. Below is a detailed breakdown of the project, including setup instructions, architecture, and service configurations.

---

## Project Components

### 1. **Backend Service**
   - **Image**: `shreybatra/sample-backend`
   - **Service Type**: `ClusterIP`
   - **Port**: `8000`
   - **Environment Variable**:
     - `REDIS_URI`: `redis://redis-svc:6379`
   - **Description**: The backend service handles application logic and connects to the Redis database for data storage.

---

### 2. **Frontend Application**
   - **Image**: `shreybatra/sample-frontend`
   - **Service Type**: `NodePort`
   - **Port**: `5173` (assignment mentions `5175`, but the actual service runs on `5173`)
   - **NodePort**: `31000`
   - **Environment Variable**:
     - `BACKEND_URL`: `http://backend-svc:8000`
   - **Description**: The frontend provides a user interface for the application and communicates with the backend via the specified URL.

---

### 3. **Redis Database**
   - **Image**: `redis`
   - **Service Type**: `ClusterIP`
   - **Port**: `6379`
   - **Description**: Redis is used as an in-memory data store for caching or database functionalities.

---

## Installation Instructions

### Prerequisites
Ensure the following tools and setups are in place:
- **Docker**: Installed and running.
- **Kubernetes Cluster**: Created using `kind` or another method.
- **Helm**: Version 3.x or above.
- **kubectl**: Configured to access your cluster.

### Steps to Install

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Chirag-Punia/cosmocloud-deploy
   cd cosmocloud-deploy
   ```

2. **Install the Helm Chart**
   Use the `helm install` command to deploy the application:
   ```bash
   helm install testapp cosmocloud-deploy --atomic --timeout 30s
   ```

3. **Verify Deployment**
   Check if all components are running:
   ```bash
   kubectl get pods
   kubectl get services
   ```

---

## Architecture Overview

### Key Features:
- **Frontend**: Exposed via a `NodePort` service on port `31000`.
- **Backend**: Internal service accessible via `ClusterIP`.
- **Redis**: Internal database service accessible via `ClusterIP`.
- **Communication**:
  - Frontend interacts with the backend via `http://backend-svc:8000`.
  - Backend connects to Redis using the URI `redis://redis-svc:6379`.
- **Replicas**:
  - All services start with **1 replica** for simplicity.
  - Scaling can be adjusted via `values.yaml`.

---

## Configuration File: `values.yaml`

This file defines all configurable parameters of the Helm chart. Below are the key parameters and their default values:

| Parameter             | Description                     | Default                     |
|-----------------------|---------------------------------|----------------------------|
| `backend.image`       | Backend container image         | `shreybatra/sample-backend` |
| `backend.replicas`    | Number of backend replicas      | `1`                        |
| `frontend.image`      | Frontend container image        | `shreybatra/sample-frontend` |
| `frontend.replicas`   | Number of frontend replicas     | `1`                        |
| `redis.image`         | Redis container image           | `redis`                    |
| `redis.replicas`      | Number of Redis replicas        | `1`                        |

---

## Commands for Debugging and Monitoring

### List All Pods:
```bash
kubectl get pods
```

### List All Services:
```bash
kubectl get services
```

### Check Logs of a Pod:
```bash
kubectl logs <pod-name>
```

### Scale a Deployment:
Adjust replicas dynamically:
```bash
kubectl scale deployment <deployment-name> --replicas=<desired-replicas>
```

---

## Sample Architecture Diagram

```plaintext
                      +-------------------+
                      |  Frontend (NodePort)  |
                      | Port: 31000         |
                      +-------------------+
                               |
                               v
                     +-------------------+
                     |  Backend (ClusterIP) |
                     | Port: 8000           |
                     +-------------------+
                               |
                               v
                     +-------------------+
                     |  Redis (ClusterIP) |
                     | Port: 6379          |
                     +-------------------+
```

---

## Notes

- Ensure **Docker** is running before setting up the Kubernetes cluster.
- Use `helm uninstall testapp` to remove the deployed chart if necessary.
- For modifications, edit the `values.yaml` file and redeploy using `helm upgrade`.

This chart provides a seamless way to deploy a full-stack application in a Kubernetes environment, enabling efficient development and scaling capabilities.