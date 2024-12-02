# Cosmocloud Deploy Helm Chart

This Helm chart deploys a complete application stack consisting of a backend service, frontend application, and Redis database.

## Components

1. Backend Service
   - Image: shreybatra/sample-backend
   - Service Type: ClusterIP
   - Port: 8000
   - Environment Variables:
     - REDIS_URI: redis://redis-svc:6379

2. Frontend Application
   - Image: shreybatra/sample-frontend
   - Service Type: NodePort
   - Port: 5173(IN ASSIGNMET IS WAS MENTIONED AS 5175 BUT ACTUAL IT WORKS ON 5173)
   - NodePort: 31000
   - Environment Variables:
     - BACKEND_URL: http://backend-svc:8000

3. Redis Database
   - Image: redis
   - Service Type: ClusterIP
   - Port: 6379


## Installation

### Prerequisites
- Kubernetes cluster (kind)
- Helm v3.x
- kubectl configured to access your cluster

### Setting up the Repository
 Clone the repository:
```bash
git clone https://github.com/Chirag-Punia/cosmocloud-deploy
cd cosmocloud-deploy
```

### Installing the Chart
To install the chart:

```bash
helm install testapp cosmocloud-deploy --atomic --timeout 30s
```

### Verifying the Installation
Check if all pods are running:
```bash
kubectl get pods
kubectl get services
```

## Configuration

Configuration values are stored in `values.yaml`. The following table lists the configurable parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| backend.image | Backend container image | shreybatra/sample-backend |
| backend.replicas | Number of backend replicas | 1 |
| frontend.image | Frontend container image | shreybatra/sample-frontend |
| frontend.replicas | Number of frontend replicas | 1 |
| redis.image | Redis container image | redis |
| redis.replicas | Number of Redis replicas | 1 |

## Architecture

The application stack is designed with the following architecture:

- Frontend service exposed via NodePort (31000)
- Backend service accessible within the cluster via ClusterIP
- Redis service accessible within the cluster via ClusterIP
- All components are deployed with 1 replica each
- Services are properly configured for internal communication