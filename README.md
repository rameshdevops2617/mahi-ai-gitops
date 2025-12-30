# 🚀 MAHI AI – GitOps Repository

## 📌 Overview
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/ec70666f-64a3-49d0-bcb2-0feb95f9a3a9" />


This repository follows **GitOps principles** to manage the deployment, rollout, and monitoring of the **MAHI AI application** on Kubernetes using:

* **Argo CD** for continuous delivery
* **Argo Rollouts** for Canary deployments
* **Kustomize** for environment overlays
* **Prometheus & Grafana** for monitoring and observability

👉 **All Kubernetes state is driven from Git**.
👉 **No manual kubectl apply for application changes**.

---

## 🧠 What This Repository Manages

This GitOps repo is responsible for:

* Kubernetes manifests (base + overlays)
* Canary and Stable deployments
* Progressive delivery strategy
* Observability stack
* Runtime configuration (not application code)

> 🔹 Application source code lives in a **separate repository**
> 🔹 This repo contains **only desired state**

---

## 📂 Repository Structure

```
mahi-ai-gitops/
├── k8s/
│   ├── base/
│   │   ├── backend-deployment.yaml
│   │   ├── frontend-deployment.yaml
│   │   ├── services.yaml
│   │   ├── redis.yaml
│   │   └── kustomization.yaml
│   │
│   ├── overlays/
│   │   ├── canary/
│   │   │   ├── rollout-backend.yaml
│   │   │   ├── rollout-frontend.yaml
│   │   │   └── kustomization.yaml
│   │   │
│   │   └── stable/
│   │       ├── kustomization.yaml
│   │
│   └── monitoring/
│       ├── prometheus-configmap.yaml
│       ├── prometheus-deployment.yaml
│       ├── prometheus-service.yaml
│       ├── grafana-deployment.yaml
│       ├── grafana-service.yaml
│       ├── node-exporter.yaml
│       └── kube-state-metrics.yaml
│
└── README.md
```

---

## 🔄 GitOps Workflow (End-to-End)

### 1️⃣ CI Pipeline (Application Repo)

* Builds backend & frontend Docker images
* Pushes versioned images (e.g. `v1.2.0`)
* Updates image tags in this GitOps repo
* Commits & pushes changes

### 2️⃣ Argo CD (Continuous Delivery)

* Watches this GitOps repository
* Detects Git changes
* Syncs Kubernetes cluster automatically
* Ensures **cluster state = Git state**

### 3️⃣ Argo Rollouts (Canary Strategy)

* Canary traffic split:

  * 10% → pause
  * 50% → pause
  * 100% → promote
* Separate **canary** and **stable** services
* Zero downtime deployment

---

## 🧪 Canary Deployment Strategy

**Backend & Frontend** both use Argo Rollouts.

### Canary Flow:

1. New version deployed to canary pods
2. Traffic gradually shifted
3. Health & metrics observed
4. Automatic or manual promotion to stable

### Rollout Benefits:

* Safe deployments
* Fast rollback
* Production-grade strategy used by FAANG

---

## 📊 Monitoring & Observability

### 🔹 Prometheus

* Scrapes:

  * Backend metrics
  * kube-state-metrics
  * node-exporter
* Central metrics store

### 🔹 Grafana

* Visualizes:

  * Node CPU / Memory
  * Pod health
  * Application metrics
* Uses Prometheus service DNS (in-cluster)

### Metrics Validation Examples:

```promql
up
node_cpu_seconds_total
kube_pod_info
```

---

## 🧩 Architecture

### High-Level Flow

```
Developer → CI Pipeline → GitOps Repo → Argo CD
                                  ↓
                           Argo Rollouts
                                  ↓
                       Canary → Stable Services
                                  ↓
                          Kubernetes Cluster
                                  ↓
                    Prometheus → Grafana Dashboards
```

---

## 🖼 Architecture Diagram

📌 **Diagram file** (place in repo root or `/docs` folder):

```
architecture.png
```

> The diagram illustrates:
>
> * GitOps flow
> * Argo CD sync
> * Canary rollout
> * Monitoring stack
> * Service communication

---

## 🔐 Key DevOps Concepts Demonstrated

* GitOps (declarative delivery)
* Progressive delivery (Canary)
* Infrastructure as Code
* Kubernetes-native observability
* Production-ready deployment patterns

---

## ✅ How to Verify Deployment (Optional)

```bash
# Argo CD sync status
argocd app get mahi-ai

# Rollout status
kubectl argo rollouts get rollout mahi-backend -n mahi-ai-canary

# Prometheus targets
kubectl port-forward svc/prometheus -n monitoring 9091:9090

# Grafana
kubectl port-forward svc/grafana -n monitoring 3001:3000
```

---

## 🏆 Why This Project Matters

This repository demonstrates **real-world DevOps practices**, not toy examples:

* Matches enterprise delivery patterns
* Resume-ready for Senior DevOps / Platform roles
* Suitable for interviews, demos, and learning

---

## 👤 Author

**Ramesh – DevOps & Platform Engineering Project**

---

### ✅ NEXT (Optional Enhancements)

If you want later:

* Alertmanager (Slack / Email)
* HPA autoscaling
* Multi-cluster GitOps
* Security policies (OPA / Kyverno)
