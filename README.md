# Assignment 2 — Kubernetes Manifests

This project demonstrates core Kubernetes concepts by deploying a **MySQL** database and a **web (Nginx)** application across dedicated namespaces.

---

## Project Structure

```
assignment2-k8s/
├── README.md
└── manifests/
    ├── 01-namespaces/
    │   ├── mysql-namespace.yaml
    │   └── web-namespace.yaml
    ├── 02-pods/
    │   ├── mysql-pod.yaml
    │   └── web-pod.yaml
    ├── 03-replicasets/
    │   ├── mysql-rs.yaml
    │   └── web-rs.yaml
    ├── 04-deployments/
    │   ├── mysql-deploy.yaml
    │   └── web-deploy.yaml
    └── 05-services/
        ├── mysql-svc.yaml
        └── web-svc.yaml
```

---

## Prerequisites

- [kubectl](https://kubernetes.io/docs/tasks/tools/) configured against a running cluster
- [Minikube](https://minikube.sigs.k8s.io/) or any Kubernetes cluster (v1.26+)

---

## Applying Manifests

Apply resources in order:

```bash
# 1. Namespaces
kubectl apply -f manifests/01-namespaces/

# 2. Pods (standalone, for exploration)
kubectl apply -f manifests/02-pods/

# 3. ReplicaSets
kubectl apply -f manifests/03-replicasets/

# 4. Deployments (recommended for production)
kubectl apply -f manifests/04-deployments/

# 5. Services
kubectl apply -f manifests/05-services/
```

---

## Namespaces

| Namespace  | Purpose             |
| ---------- | ------------------- |
| `mysql-ns` | MySQL database tier |
| `web-ns`   | Web / frontend tier |

---

## Components

### MySQL

| Resource   | Name           | Namespace  | Image       | Port |
| ---------- | -------------- | ---------- | ----------- | ---- |
| Pod        | `mysql-pod`    | `mysql-ns` | `mysql:8.0` | 3306 |
| ReplicaSet | `mysql-rs`     | `mysql-ns` | `mysql:8.0` | 3306 |
| Deployment | `mysql-deploy` | `mysql-ns` | `mysql:8.0` | 3306 |
| Service    | `mysql-svc`    | `mysql-ns` | —           | 3306 |

### Web (Nginx)

| Resource   | Name         | Namespace | Image          | Port |
| ---------- | ------------ | --------- | -------------- | ---- |
| Pod        | `web-pod`    | `web-ns`  | `nginx:latest` | 80   |
| ReplicaSet | `web-rs`     | `web-ns`  | `nginx:latest` | 80   |
| Deployment | `web-deploy` | `web-ns`  | `nginx:latest` | 80   |
| Service    | `web-svc`    | `web-ns`  | —              | 80   |

---

## Verifying Resources

```bash
# Check all namespaces
kubectl get namespaces

# Check pods in each namespace
kubectl get pods -n mysql-ns
kubectl get pods -n web-ns

# Check services
kubectl get svc -n mysql-ns
kubectl get svc -n web-ns

# Access the web app (Minikube)
minikube service web-svc -n web-ns
```

---

## Teardown

```bash
kubectl delete -f manifests/05-services/
kubectl delete -f manifests/04-deployments/
kubectl delete -f manifests/03-replicasets/
kubectl delete -f manifests/02-pods/
kubectl delete -f manifests/01-namespaces/
```
