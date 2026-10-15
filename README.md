# Kubernetes Application

A Kubernetes project demonstrating the deployment and operation of a containerized full-stack application on a local Kubernetes cluster.

The project covers core Kubernetes concepts including **Deployments, Services, Ingress, ConfigMaps, Secrets, persistent storage, health probes, autoscaling, RBAC, NetworkPolicies, and pod scheduling**.

## Architecture

```text
                         Browser
                            │
                            ▼
                    ┌──────────────┐
                    │    Ingress   │
                    │     NGINX    │
                    └──────┬───────┘
                           │
                 ┌─────────┴─────────┐
                 │                   │
                 ▼                   ▼
          ┌────────────┐      ┌────────────┐
          │  Frontend  │      │  Backend   │
          │ 2 replicas │      │ 2+ replicas│
          └────────────┘      └──────┬─────┘
                                     │
                                     ▼
                              ┌────────────┐
                              │   MySQL    │
                              │    + PVC   │
                              └────────────┘
```

Application images are pulled from Docker Hub and deployed as Kubernetes workloads.

## Kubernetes Components

| Area | Resources / Concepts |
|---|---|
| Workloads | Deployments, Pods, Replicas |
| Networking | Services, NGINX Ingress, NetworkPolicy |
| Configuration | ConfigMaps, Secrets |
| Storage | PersistentVolumeClaim |
| Reliability | Readiness & Liveness Probes |
| Autoscaling | HPA, VPA |
| Security | ServiceAccount, Role, RoleBinding |
| Scheduling | Taints, Tolerations, Node Selector, Affinity |

## Project Structure

```text
k8s-app/
│
├── backend-deployment.yaml
├── backend-service.yaml
├── backend-configmap.yaml
├── backend-secret.yaml
├── backend-hpa.yaml
├── backend-vpa.yaml
├── backend-serviceaccount.yaml
├── backend-role.yaml
├── backend-rolebinding.yaml
│
├── frontend-deployment.yaml
├── frontend-service.yaml
│
├── mysql-deployment.yaml
├── mysql-service.yaml
├── mysql-pvc.yaml
├── mysql-secret.yaml
├── mysql-networkpolicy.yaml
│
├── ingress.yaml
│
├── examples/
│   └── scheduling/
│       ├── scheduling-test.yaml
│       ├── scheduling-selector.yaml
│       ├── scheduling-affinity.yaml
│       ├── scheduling-preferred.yaml
│       └── README.md
│
└── README.md
```

## Prerequisites

- Docker Desktop
- Kubernetes enabled in Docker Desktop
- `kubectl`
- NGINX Ingress Controller
- Metrics Server

Verify the cluster:

```bash
kubectl cluster-info
kubectl get nodes
```

## Getting Started

### 1. Create the Namespace

```bash
kubectl create namespace games-legend
```

### 2. Configure Secrets

Update the provided Secret manifests with your own credentials before applying them.

Do not commit real passwords, API keys, or other sensitive values.

### 3. Deploy the Application

```bash
kubectl apply -f backend-configmap.yaml
kubectl apply -f backend-secret.yaml
kubectl apply -f mysql-secret.yaml
kubectl apply -f mysql-pvc.yaml

kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml

kubectl apply -f backend-deployment.yaml
kubectl apply -f backend-service.yaml

kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml

kubectl apply -f mysql-networkpolicy.yaml
kubectl apply -f ingress.yaml

kubectl apply -f backend-hpa.yaml
kubectl apply -f backend-vpa.yaml

kubectl apply -f backend-serviceaccount.yaml
kubectl apply -f backend-role.yaml
kubectl apply -f backend-rolebinding.yaml
```

### 4. Verify the Deployment

```bash
kubectl get all -n games-legend
```

```bash
kubectl get pods -n games-legend
```

```bash
kubectl get svc -n games-legend
```

```bash
kubectl get ingress -n games-legend
```

```bash
kubectl get pvc -n games-legend
```

## Access the Application

For the local environment, forward traffic from the NGINX Ingress Controller:

```bash
kubectl port-forward -n ingress-nginx \
  service/ingress-nginx-controller 8080:80
```

Add the application hostname to `/etc/hosts`:

```text
127.0.0.1 games-legend.local
```

Then open:

```text
http://games-legend.local
```

## Kubernetes Features

### Autoscaling

The backend uses:

- **HPA** for CPU-based horizontal scaling
- **VPA** in recommendation-only mode (`updateMode: Off`)

Check the HPA:

```bash
kubectl get hpa -n games-legend
```

Check VPA recommendations:

```bash
kubectl describe vpa backend-vpa -n games-legend
```

### Health Checks

The backend deployment uses Kubernetes:

- Readiness Probe
- Liveness Probe

These allow Kubernetes to determine whether the application is ready to receive traffic and whether a container needs to be restarted.

### Security

The project includes:

- Dedicated ServiceAccount
- Role and RoleBinding with limited pod permissions
- NetworkPolicy restricting MySQL access to backend pods

### Scheduling

The `examples/scheduling` directory contains standalone examples for:

- Taints and Tolerations
- Node Selectors
- Required Node Affinity
- Preferred Node Affinity

## Useful Commands

```bash
# View resources
kubectl get all -n games-legend

# View pods
kubectl get pods -n games-legend

# View logs
kubectl logs <pod-name> -n games-legend

# Inspect a resource
kubectl describe pod <pod-name> -n games-legend

# Watch pods
kubectl get pods -n games-legend -w

# View resource usage
kubectl top pods -n games-legend

# Remove the application
kubectl delete namespace games-legend
```

## Docker Images

The Kubernetes workloads use the application images published to Docker Hub:

- `youssef4403/games-legend-backend`
- `youssef4403/games-legend-frontend`

The Dockerized application is maintained in a separate repository.

## Technologies

- Kubernetes
- Docker
- Docker Hub
- NGINX Ingress
- MySQL
- YAML
- kubectl

## Project Scope

This project was developed as a hands-on Kubernetes and DevOps project using a local Kubernetes cluster, focusing on application orchestration, networking, storage, security, scaling, and workload management.

## Related Repository

**Dockerized Games Legend:**  
https://github.com/Youssef22s/dockerized-games-legend
