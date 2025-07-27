# 🧪 Kubernetes Cluster Management System - Minikube Setup

> Run and monitor your custom NGINX web application locally using Minikube, YAML, HPA, and Prometheus + Grafana.

---

## 📦 Docker Image Used

- **Image**: `kiran22222/nginx-web-app-rk`
- A custom NGINX-based web application to demonstrate Kubernetes deployments, autoscaling, and monitoring.

---

## 🧰 Required Tools

Make sure these tools are installed on your system:

| Tool      | Installation Link |
|-----------|-------------------|
| Minikube  | https://minikube.sigs.k8s.io/docs/start/ |
| kubectl   | https://kubernetes.io/docs/tasks/tools/install-kubectl/ |
| Helm      | https://helm.sh/docs/intro/install/ |

---

## 🚀 Step 1: Start Minikube

```bash
minikube start --cpus=4 --memory=4096
kubectl get nodes
````

---

## ⚙️ Step 2: Deploy Your App

### Option 1: CLI Quick Deploy

```bash
kubectl create deployment nginx-web-app --image=kiran22222/nginx-web-app-rk
kubectl expose deployment nginx-web-app --type=NodePort --port=80
```

To access the app:

```bash
minikube service nginx-web-app
```

---

### Option 2: YAML-Based Deployment

#### 1️⃣ Apply Deployment

```bash
kubectl apply -f deployment.yaml
```

#### 2️⃣ Apply Service

```bash
kubectl apply -f service.yaml
```

To access the app:

```bash
minikube service nginx-web-service
```

---

## 📈 Step 3: Enable HPA (Horizontal Pod Autoscaler)

### 🔧 Install Metrics Server

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Wait a few seconds, then verify:

```bash
kubectl top pods
```

---

### 📊 Apply HPA YAML

```bash
kubectl apply -f hpa.yaml
kubectl get hpa -w
```

---

## 📉 Step 4: Setup Prometheus & Grafana (Monitoring)

### 🔧 Install via Helm

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
```

---

### 📺 Access Grafana Dashboard

```bash
kubectl port-forward svc/monitoring-grafana -n monitoring 3000:80
```

Then open: [http://localhost:3000](http://localhost:3000)

🔑 Default credentials:

* Username: `admin`
* Password (run below):

```bash
kubectl get secret -n monitoring monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```

---

### 📊 Import Dashboards (Optional)

1. Go to Grafana → Dashboards → Import
2. Use dashboard IDs from [https://grafana.com/grafana/dashboards](https://grafana.com/grafana/dashboards)

   * **Node Exporter Full** → ID: `1860`
   * **Kubernetes Cluster Monitoring** → ID: `10000`

---

## 🧹 Cleanup

```bash
kubectl delete -f manifests/
helm uninstall monitoring -n monitoring
minikube stop
```

---

## 💡 What You Practiced

✅ Deploying a custom Docker image on Kubernetes
✅ Managing services with YAML & kubectl
✅ Enabling and testing HPA
✅ Monitoring using Prometheus & Grafana

---

## 📁 Project Structure

```

│   ├── deployment.yaml
│   ├── service.yaml
│   └── hpa.yaml
│   └── prometheus-grafana-install.sh
└── README-minikube.md
```

---

Happy Kubernetes-ing! 💙
Project by [Kiran Rakh](https://github.com/kiranrakh)

```

