# 🚀 Kubernetes Production-Style Deployment Project

## 📌 Overview

This project demonstrates deploying and managing a containerized application on Kubernetes using production-grade practices including Helm packaging, Ingress routing, monitoring, and auto-scaling.

---

## Architecture

```
                        ┌─────────────────────────────────────────────────────┐
                        │                  k3s Cluster (WSL2)                 │
                        │                                                      │
                        │   ┌─────────────────────────────────────────────┐   │
                        │   │              myapp namespace                 │   │
                        │   │                                              │   │
  curl / browser  ────▶ │   │  NGINX Ingress ──▶ ClusterIP Svc           │   │
   :31080               │   │                          │                   │   │
                        │   │                    ┌─────┴─────┐             │   │
                        │   │                    ▼           ▼             │   │
                        │   │                  Pod 1       Pod 2  ···      │   │
                        │   │               (Python)    (Python)           │   │
                        │   │                    │                         │   │
                        │   │              ConfigMap  HPA (cpu 50%)        │   │
                        │   └─────────────────────────────────────────────┘   │
                        │                                                      │
                        │   ┌─────────────────────────────────────────────┐   │
                        │   │            monitoring namespace              │   │
                        │   │                                              │   │
                        │   │   metrics-server ──▶ HPA                    │   │
                        │   │   Prometheus ────────scrapes──▶ Pods        │   │
                        │   │   Grafana :32000 ────queries──▶ Prometheus  │   │
                        │   └─────────────────────────────────────────────┘   │
                        └─────────────────────────────────────────────────────┘
```

<img width="1920" height="1080" alt="Screenshot (440)" src="https://github.com/user-attachments/assets/b3dcfaed-c7ec-4de0-a39d-330bd86ad79c" />
<img width="1920" height="1080" alt="Screenshot (438)" src="https://github.com/user-attachments/assets/88bf1d87-81b2-4e9c-b144-39ff5be122c1" />
<img width="1920" height="1080" alt="Screenshot (437)" src="https://github.com/user-attachments/assets/c4456b99-b68e-439d-b0fa-dbf8be5dda4c" />

---

## ⚙️ Tech Stack

* Kubernetes (k3s)
* Docker
* Helm
* NGINX Ingress Controller
* Prometheus
* Grafana

---

## 🚀 Features

* Containerized Python application
* Kubernetes Deployment with resource limits
* Service exposure via NodePort
* Host-based routing using Ingress
* Helm chart for reusable deployments
* Monitoring with Prometheus and Grafana
* Auto-scaling using HPA based on CPU usage

---

## 📦 Deployment Steps

### 1. Build and import Docker image

docker build -t myapp:latest ./app
docker save myapp:latest -o myapp.tar
sudo k3s ctr images import myapp.tar

---

### 2. Deploy using Helm

helm install myrelease ./helm-chart

---

### 3. Access application

curl -H "Host: myapp.local" http://localhost:<NodePort>

---

### 4. Access Grafana

kubectl port-forward svc/monitoring-grafana 3000:80

URL: http://localhost:3000
Username: admin
Password: (retrieved from Kubernetes secret)

---

### 5. Trigger Auto Scaling

kubectl run -i --tty load-generator --rm --image=busybox -- /bin/sh

while true; do wget -q -O- http://myrelease-service; done

---

## 📊 Observability

* Prometheus scrapes cluster metrics
* Grafana dashboards visualize:

  * CPU usage
  * Memory usage
  * Pod scaling behavior

---

## ⚡ Auto Scaling

* HPA configured with:

  * Min replicas: 2
  * Max replicas: 5
  * CPU threshold: 50%

---

## 🎯 Outcome

This project simulates a real-world Kubernetes deployment pipeline with monitoring and scaling, demonstrating production-ready DevOps skills.
