# Kubernetes

Files for running ListMates on Kubernetes (minikube on a laptop, k3s on the Azure VM):

- `mongodb/`  MongoDB Community through the official MongoDB Controllers for Kubernetes operator (not the Bitnami chart)
- `backend/`  Deployment, Service and Secret for the FastAPI backend
- `frontend/` Deployment and Service for the built React app
- `monitoring/` Uptime Kuma

Local setup (added in the Kubernetes issue):

```bash
minikube start
helm repo add mongodb https://mongodb.github.io/helm-charts
helm install mongodb-kubernetes mongodb/mongodb-kubernetes -n mongodb --create-namespace
kubectl apply -f k8s/mongodb/
```
