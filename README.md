



# Cosmocloud-Deploy Helm Chart

## Introduction

`Cosmocloud-deploy` is a Helm chart designed to deploy a complete application stack consisting of:
- **Backend**: A scalable backend service.
- **Frontend**: A user-friendly frontend service.
- **Redis**: A fast in-memory database for caching and data storage.

This chart is structured for simplicity, scalability, and modularity, leveraging Kubernetes and Helm to manage deployments efficiently.

---

## Features
1. **Applications Deployed:**
   - Backend: Containerized service exposing REST APIs.
   - Frontend: Web application interacting with the backend.
   - Redis: Key-value store.

2. **Service Types:**
   - Backend: ClusterIP for internal communication.
   - Frontend: NodePort for external exposure.
   - Redis: ClusterIP for internal communication.

3. **Environment Variables:**
   - Backend: `REDIS_URI` for Redis connection.
   - Frontend: `BACKEND_URL` for backend communication.

4. **Namespace:** Default namespace is used.

---

## Prerequisites
- **Kubernetes Cluster** (v1.22+)
- **Helm** (v3.0+)
- Sufficient cluster resources to deploy the application stack.

---

## Installation Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/SUGAM-ARORA/Cosmocloud-deploy.git
cd Cosmocloud-deploy
```

### 2. Lint the Helm Chart
Ensure the Helm chart is syntactically correct:
```bash
helm lint .
```

### 3. Package the Chart
Package the chart into a `.tgz` file:
```bash
helm package .
```

### 4. Install the Helm Chart
Deploy the application stack using Helm:
```bash
helm install testapp . --atomic --timeout 30s
```

---

## Configuration
### Default Values
The chart's default values are defined in `values.yaml`. Below are the configurable parameters:

```yaml
backend:
  image:
    repository: shreybatra/sample-backend
    tag: latest
  env:
    REDIS_URI: redis://redis-svc:6379
  service:
    type: ClusterIP
    port: 8000

frontend:
  image:
    repository: shreybatra/sample-frontend
    tag: latest
  env:
    BACKEND_URL: http://backend-svc:8000
  service:
    type: NodePort
    port: 5173
    nodePort: 31000

redis:
  image:
    repository: redis
    tag: latest
  service:
    type: ClusterIP
    port: 6379
```

### Customization
To override default values, create a custom `values.yaml` file:
```yaml
backend:
  image:
    repository: your-backend-image
  env:
    REDIS_URI: redis://custom-redis:6379
```
Install the chart with your custom values:
```bash
helm install testapp . -f custom-values.yaml
```

---

## File Structure
```
Cosmocloud-deploy/
├── Chart.yaml            # Helm chart metadata
├── values.yaml           # Default configuration values
├── templates/            # Kubernetes resource templates
│   ├── deployment.yaml   # Deployment configurations
│   ├── service.yaml      # Service configurations
│   ├── _helpers.tpl      # Template helpers
│   ├── ingress.yaml      # Ingress configurations (optional)
│   └── NOTES.txt         # Post-installation instructions
```

---

## Verifying the Deployment

1. **Check Pods:**
   ```bash
   kubectl get pods
   ```

2. **Check Services:**
   ```bash
   kubectl get svc
   ```

3. **Access the Application:**
   - Frontend: Access via NodePort (`<NodeIP>:31000`).
   - Backend and Redis: Internal ClusterIP services.

---

## Troubleshooting

- **Cluster Unreachable:** Ensure your Kubernetes cluster is running and `kubectl` is configured correctly.
- **Chart Errors:** Run `helm lint .` to validate your chart.
- **Deployment Issues:** Review pod logs:
  ```bash
  kubectl logs <pod-name>
  ```

---

## Cleanup
To uninstall the application stack:
```bash
helm uninstall testapp
```

---

## Contributing
Contributions are welcome! Feel free to fork this repository, make changes, and submit a pull request.

---

## License
This project is licensed under the MIT License.




