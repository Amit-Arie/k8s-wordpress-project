# WordPress & MySQL Deployment on Kubernetes with Helm & Monitoring

An end-to-end Kubernetes project demonstrating the deployment, management, packaging, and monitoring of a scalable WordPress application backed by MySQL.

---

## 🏗 Architecture Overview

* **Container Registry:** Amazon ECR (Private images for WordPress and MySQL).
* **Database Layer:** MySQL deployed as a StatefulSet with PersistentVolumeClaim (2Gi storage) and a Headless Service.
* **Application Layer:** WordPress deployed via Deployment controller (2 Replicas) with environment variables injected via Kubernetes Secrets.
* **Ingress & Networking:** NGINX Ingress Controller routing HTTP traffic on port 80 to the WordPress service.
* **Packaging & Automation:** Fully customizable Helm Chart (`wordpress-chart`) managing configuration via `values.yaml`.
* **Monitoring Stack:** Prometheus and Grafana collecting cluster metrics and tracking container Uptime and availability.

---

## 📁 Repository Structure

```text
.
├── Dockerfile-mysql
├── Dockerfile-wordpress
├── monitoring-simple.yaml
├── README.md
└── wordpress-chart/
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
        ├── ingress.yaml
        ├── mysql.yaml
        ├── secret.yaml
        └── wordpress.yaml
```

---

## 🚀 Deployment Instructions

### 1. Configure ECR Authentication
```bash
kubectl create secret docker-registry ecr-secret \
  --docker-server=<ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com \
  --docker-username=AWS \
  --docker-password=$(aws ecr get-login-password --region <REGION>)
```

### 2. Deploy Application Stack via Helm
```bash
helm install my-wp ./wordpress-chart
```

### 3. Verify Workloads
```bash
kubectl get pods
kubectl get svc
kubectl get ingress
```

### 4. Deploy Monitoring Stack
```bash
kubectl apply -f monitoring-simple.yaml
```

---

## 📊 Monitoring & Dashboards

* **Grafana Port Forward:**
  ```bash
  sudo kubectl port-forward -n monitoring svc/grafana 3000:3000 --address 0.0.0.0
  ```

* **PromQL Uptime Metric:**
  ```promql
  time() - process_start_time_seconds
  ```
