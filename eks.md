# ☁️ Kubernetes Cluster Management System - AWS EKS Setup

> This guide will help you deploy, autoscale, and monitor your custom NGINX-based app (`kiran22222/nginx-web-app-rk`) on **Amazon EKS** using `kubectl`, `eksctl`, and `Helm`.

---

## 📦 Docker Image Used

- **Image**: `kiran22222/nginx-web-app-rk`  
A containerized static web app built on NGINX.

---

## 🧰 Required Tools

| Tool     | Installation Link |
|----------|-------------------|
| AWS CLI  | https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html |
| eksctl   | https://eksctl.io/introduction/#installation |
| kubectl  | https://kubernetes.io/docs/tasks/tools/install-kubectl/ |
| Helm     | https://helm.sh/docs/intro/install/ |

---

## 🔧 Step 1: AWS Setup

### ✅ Configure AWS CLI

```bash
aws configure
````

You’ll be prompted for:

* AWS Access Key
* Secret Key
* Region (e.g. `ap-south-1`)
* Output format (default: `json`)

---

## 🚀 Step 2: Create EKS Cluster

```bash
eksctl create cluster \
  --name cluster-project-3 \
  --region ap-south-1 \
  --nodes 2

kubectl get nodes
```

---

## ⚙️ Step 3: Deploy Your App

### Option 1: CLI Quick Deploy

```bash
kubectl create deployment nginx-web-app --image=kiran22222/nginx-web-app-rk
kubectl expose deployment nginx-web-app --type=LoadBalancer --port=80
kubectl get svc
```

### Option 2: YAML-Based Deployment

Apply these manifests:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## 📈 Step 4: Setup HPA (Autoscaler)

### ✅ Install Metrics Server

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Wait a few moments, then verify:

```bash
kubectl top pods
```

### ✅ Apply HPA YAML

```bash
kubectl apply -f hpa.yaml
kubectl get hpa -w
```

---

## 📊 Step 5: Monitoring with Prometheus + Grafana

### 🛠️ Install Prometheus & Grafana Stack

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace
```

---

### 📺 Access Grafana Dashboard

```bash
kubectl port-forward svc/monitoring-grafana -n monitoring 3000:80
```

➡️ Open: [http://localhost:3000](http://localhost:3000)

🔑 Get Grafana password:

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
kubectl delete -f 
helm uninstall monitoring -n monitoring
eksctl delete cluster --name cluster-project-3
```

---

## 📁 Project Structure

```
│   ├── deployment.yaml
│   ├── service.yaml
│   └── hpa.yaml
│   └── prometheus-grafana-install.sh
└── README-eks.md
```

---

## ✅ You’ve Learned:

* 🔧 How to create an EKS cluster using `eksctl`
* 🚀 How to deploy and expose apps on EKS
* 📈 How to scale using HPA with CPU metrics
* 📊 How to monitor workloads using Prometheus & Grafana

---

Deploy with confidence. Scale with control. Monitor with visibility.
**Built by [Kiran Rakh](https://github.com/kiranrakh)** 💡

```
