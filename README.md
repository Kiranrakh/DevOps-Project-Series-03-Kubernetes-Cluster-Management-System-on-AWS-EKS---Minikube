# 🚀 Kubernetes Cluster Management System

> Deploy, scale, and monitor a custom NGINX web app across **Minikube (Local)** and **AWS EKS (Cloud)** environments using Kubernetes best practices.

---

## 📦 Docker Image Used

- Docker Hub: [`kiran22222/nginx-web-app-rk`](https://hub.docker.com/r/kiran22222/nginx-web-app-rk)

---


---

## 🧠 What You'll Learn

- ✅ Kubernetes deployments & services
- ✅ Horizontal Pod Autoscaling (HPA)
- ✅ Minikube for local testing
- ✅ EKS for cloud production setup
- ✅ Monitoring using Prometheus & Grafana via Helm

---

## 📌 Architecture Overview

```

```
                            ┌────────────────────────────┐
                            │     Users / Client         │
                            └────────────┬───────────────┘
                                         │
                                         ▼
                        ┌────────────────────────────────────┐
                        │     LoadBalancer / NodePort        │
                        └────────────────────────────────────┘
                                         │
                  ┌──────────────────────┼──────────────────────┐
                  │                      │                      │
                  ▼                      ▼                      ▼
         ┌──────────────┐       ┌──────────────┐       ┌──────────────┐
         │   Pod: myapp │       │   Pod: myapp │  ...  │   Pod: myapp │
         └──────────────┘       └──────────────┘       └──────────────┘
                  │
                  ▼
    ┌──────────────────────────────┐
    │ Prometheus + Grafana (Helm) │
    └──────────────────────────────┘
```

````

---

## 📍 Local Deployment (Minikube)

See [`minikube.md`](./minikube.md) for full steps.

```bash
minikube start --driver=docker --cpus=4 --memory=4096

kubectl apply -f deployments/myapp-deployment.yaml
kubectl apply -f deployments/myapp-service.yaml
kubectl apply -f deployments/hpa.yaml
````

---

## ☁️ Cloud Deployment (AWS EKS)

See [`eks.md`](./eks.md) for full setup using `eksctl`.

```bash
eksctl create cluster --name cluster-project-3 --region ap-south-1 --nodes=2

kubectl apply -f deployments/myapp-deployment.yaml
kubectl expose deployment myapp --type=LoadBalancer --port=80
```

---

## 📊 Monitoring with Prometheus + Grafana

Install using Helm:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
kubectl create namespace monitoring
helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring
```

Access Grafana:

```bash
kubectl port-forward svc/monitoring-grafana -n monitoring 3000:80
```

---

## 📈 Scale with HPA

```bash
kubectl autoscale deployment myapp --cpu-percent=50 --min=2 --max=10
kubectl get hpa -w
```

---

## 🧹 Cleanup

```bash
kubectl delete deployment myapp
kubectl delete svc myapp
helm uninstall monitoring -n monitoring
eksctl delete cluster --name cluster-project-3
```

---

## 🙌 Author

**Kiran Rakh**
📍 Pune | ☁️ DevOps | 🔧 LinuxWorld Intern
🔗 [LinkedIn](https://www.linkedin.com/in/kiran-rakh)

---

