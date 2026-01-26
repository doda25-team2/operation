# SMS Checker Operations

Operations repository for the SMS Spam Checker application. This repository contains deployment configurations for Docker Compose and Kubernetes.

Related repositories:
- [app](https://github.com/doda25-team2/app) - Frontend service
- [model-service](https://github.com/doda25-team2/model-service) - ML backend
- [lib-version](https://github.com/doda25-team2/lib-version) - Shared library

The application has two major deployment routes:

1. `Docker` → [see here](#docker-deployments)
2. `Kubernetes` → [see here](#kubernetes-deployment)

---

### Global System Requirements

Irrespective of the deployment route, you must have cloned this repository and have `cd` into the repo:

```
git clone git@github.com:doda25-team2/operation.git sms-checker-operations
cd sms-checker-operations
```

## Quick Start with Docker Compose

The fastest way to run the application locally.

```bash
# Run the containers
docker compose up -d
```

Access the application at http://localhost:8080/sms

![](./docs/imgs/sms-checker.gif)

To stop: `docker compose down`

### Customizing Images

By default, the compose file uses the latest published images. To override versions:

```bash
cp .env.example .env
# Edit .env to set specific image tags
docker compose up -d
```

---

## Kubernetes Deployment

### Option 1: Vagrant (Full 3-node cluster)

Creates a complete Kubernetes cluster with Istio, Prometheus, and Grafana. Takes ~10-15 minutes.

**Requirements:**
- Vagrant and VirtualBox installed
- At least 14GB free RAM

**Steps:**

```bash
# Start the cluster
vagrant up

# Set up kubectl access from your host
export KUBECONFIG=$(pwd)/admin.conf

# Verify the cluster
kubectl get nodes

# Deploy the application
helm upgrade --install operation ./deployment

# Add to /etc/hosts:
# 192.168.56.95   sms-checker.local
# 192.168.56.95   grafana.local
# 192.168.56.95   dashboard.local
```

**Access the application:**
- App: http://sms-checker.local/sms
- Grafana: http://grafana.local (admin/admin)
- Kubernetes Dashboard: https://dashboard.local

**Note:** When visiting the app in a browser, you'll see the **stable version**. To test the canary version, send requests with the `x-user-id` header (see Canary Deployment section below).

**AlertManager** (NodePort only - find the port):
```bash
kubectl get svc operation-kube-prometheus-alertmanager -n default
# Then access: http://192.168.56.100:<PORT>
```

**Kubernetes Dashboard login:**
```bash
# Get a token
vagrant ssh ctrl --command "kubectl -n kubernetes-dashboard create token admin-user"

# Then visit https://dashboard.local and paste the token
```

### Option 2: Minikube (Lightweight local cluster)

**Requirements:**
- Minikube, kubectl, and Helm installed
- Docker running

**Steps:**

```bash
# Start Minikube
minikube start --driver=docker --cni=calico
minikube addons enable ingress

# Install Istio
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.25.2 sh -
istio-1.25.2/bin/istioctl install -y

# Deploy the app
helm upgrade --install operation ./deployment

# Add to /etc/hosts:
# 127.0.0.1 sms-checker.local

# Start the tunnel (keep running in a separate terminal)
minikube tunnel
```

Access at http://sms-checker.local

### Option 3: Existing Kubernetes Cluster

If you already have a Kubernetes cluster with Istio installed:

```bash
helm upgrade --install operation ./deployment
```

Make sure to configure your DNS or /etc/hosts to point the application hostname to your Istio Ingress Gateway IP.

---

## Understanding the Setup

### Network Architecture (Vagrant)

When using Vagrant, you get 3 VMs:
- `ctrl` (192.168.56.100) - Kubernetes control plane
- `node-1` (192.168.56.101) - Worker node
- `node-2` (192.168.56.102) - Worker node

The Istio Ingress Gateway gets assigned IP `192.168.56.95` by MetalLB. This is why you add `192.168.56.95 sms-checker.local` to your hosts file.

### What Gets Installed

The Vagrant provisioning automatically installs:
- Kubernetes (kubeadm, kubelet, kubectl)
- Flannel (pod networking)
- MetalLB (load balancer)
- Nginx Ingress Controller
- Istio service mesh
- Kubernetes Dashboard

The Helm chart (`./deployment`) installs:
- Application and model service deployments
- Istio traffic management (Gateway, VirtualService, DestinationRule)
- Prometheus and Grafana (via kube-prometheus-stack)
- Custom dashboards and alerts

### Canary Deployment

The application uses Istio for A/B testing. Traffic is routed based on the `x-user-id` header:
- User IDs ending in "0" go to the **canary version** (~10%)
- All other IDs go to the **stable version** (~90%)
- Browser visits (no header) default to **stable version**

**Current versions:**
- Stable: `ghcr.io/doda25-team2/app:latest`
- Canary: `ghcr.io/doda25-team2/app:v1.3.1`

Test different versions:
```bash
# Stable version
curl -H "x-user-id: user123" http://sms-checker.local/sms

# Canary version (ID ends in 0)
curl -H "x-user-id: user0" http://sms-checker.local/sms
```

---

## Monitoring and Metrics

The app exposes custom Prometheus metrics at `/metrics`:
- `sms_classification_requests_total` - Total requests
- `sms_classification_results_total{result}` - Results by type (spam/ham)
- `sms_current_message_length_chars` - Current message length
- `sms_classification_duration_milliseconds` - Response times
- `sms_message_length_chars` - Message length distribution

View these in Grafana at http://grafana.local (credentials: admin/admin).

Main dashboards:
- SMS metrics
![SMS Metrics Dashboard](/operation/docs/imgs/sms-metrics-grafana.png)

- A4 Experiment: Stable vs Canary
![Grafana Dashboard showing A/B test results](/operation/docs/imgs/stable-vs-canary-grafana.png)
For experiment results you can refer to [docs/continuous-experimentation.md](./docs/continuous-experimentation.md)

**Troubleshooting:** If Grafana shows "Prometheus datasource not found":
1. Go to Configuration → Data sources
2. Click "Add data source" → select "Prometheus"
3. URL: `http://operation-kube-prometheus-prometheus.default:9090`
4. Access: Server (default)
5. Click "Save & Test"

This can happen due to a race condition where Grafana starts before the sidecar provisions the datasource.

---

## Documentation

For detailed architecture and design information, see:
- [docs/deployment.md](./docs/deployment.md) - Complete deployment architecture
- [docs/continuous-experimentation.md](./docs/continuous-experimentation.md) - A/B testing setup
- [docs/extension.md](./docs/extension.md) - Proposed improvements
