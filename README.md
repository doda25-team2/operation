# Operations and Deployment Routes

This is the operations repository of the `spamchecker` toy application which is an orchestration of the [`model-service`](https://github.com/doda25-team2/model-service) and [`app`](https://github.com/doda25-team2/app) repository belonging to Team 2 of the TUD DoDa Team 2026.

The application has three deployment routes:

1. Local Docker Deployment → [see here](#deployment-route-1-local-docker)
2. VM Based Kubernetes Deployment → [see here](#deployment-route-2-kubernetes-on-vm-nodes)
3. Local Kubernetes Deployment → [see here](#deployment-route-3-local-kubernetes)

> [!IMPORTANT]
> These instructions are written for POSIX systems and you may have to look up some equivalents for Windows machines.

### Global System Requirements

Irrespective of the deployment route, you must have cloned this repository and have `cd` into the repo:

```
git clone git@github.com:doda25-team2/operation.git sms-checker-operations
cd sms-checker-operations
```

You do not need to clone any of the other repositories as container images will be pulled automatically by any of the deployment routes below.

# Deployment Route 1: Local Docker

Below are the details for deploying using `Docker`

### Quick Setup:

Your system must include `docker`, `docker compose` and both must be running,

Run:

```bash
docker compose up -d
```

> [!WARNING]
> By default the application binds to port `8080` which you might have in use.
> If this is the case, we strongly advise that you set up environment variables as
> described in [Customizing your Docker Deployment](#customizing-your-docker-deployment).

3. Open your browser at `http://localhost:8080/sms` to access the `app` frontend, the apidocs are available at `http://localhost:8080/apidocs`.

![](./docs/imgs/sms-checker.gif)

To stop the services:

```bash
docker compose down
```

### Customizing your Docker Deployment

By default, the `docker-compose` uses the container images tagged as `latest`. You can override image versions and ports using environment variables. Copy `.env.example` to `.env` and customize:

```bash
cp .env.example .env
# Edit .env to set custom image versions
docker compose up -d
```

Or set them directly:
```bash
MODEL_SERVICE_IMAGE=ghcr.io/doda25-team2/model-service:v1.0.0 docker compose up -d
```

### Advanced: Rebuilding and Developing Locally

If you need to build the images locally (for development or after changes), you should refer to the [app](https://github.com/doda25-team2/app) and [model-service](https://github.com/doda25-team2/model-service) repositories. 

> [!CAUTION]
> Docker will very likely keep using **old** images even afeter new `app` or `model-service` images are published by the team.
> You'll want to check the tags manually, or take the more destructive route and clear out images if you're out of date.

# Deployment Route 2: Kubernetes on VM Nodes 

The repository offers `vagrant` and `ansible` scripts and playbooks which implements the `kubernetes` deployment above.

### System Requirements:

- a virtual machine provisioner (e.g., Virtualbox)
- `vagrant`
- `ansible`

### Quick Setup:

1. From operations root, run.
```bash
vagrant up
```
> [!NOTE]
> Make yourself a coffee or something, because it will take a few minutes to install everything!


2. From the host machine, run the system finalization playbook:
```bash
ansible-playbook -u vagrant -i 192.168.56.100, ./ansible/finalization.yaml
```

>[!WARNING]
>We are aware of a bug (see #108) associated with VM network readiness if the playbook is run too fast.
>Suggest waiting a minute or two before running the playbook.

3. On the host machine, add this DNS entry in /etc/hosts (will need sudo):
```
192.168.56.95  sms-checker.local
```

4. From the host machine, deploy the chart:
```bash
vagrant ssh ctrl --command "helm upgrade --install operation /vagrant/deployment"
```

The application's UI and metrics endpoint is available at:
[http://sms-checker.local/sms](sms-checker.local/sms)

# Deployment Route 3: Local Kubernetes

This application may be deployed using a single Helm chart located in the `operation/` folder.  It installs all microservices (app, model) along with their Services and Ingress routing.

### System Requirements

- `Docker`,
- A running Kubernetes cluster (Docker Desktop, Minikube, Kind, etc.)

## Quick Setup

1. Add the application FQD to to /etc/hosts; you will likely need `sudo`:
```bash
127.0.0.1       sms-checker.local
```
2. Install and enable the `nginx` ingress controller if not already available:
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx
```
> [!WARNING]
> Install and activate (e.g., `minikube addons enable ingress`) the ingress controller.
3. Similarly, install the `istio` service mesh:
```bash
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
helm install istio-base istio/base -n istio-system --create-namespace
helm install istiod istio/istiod -n istio-system --wait
```
5. To bring up the services run:
```bash
helm upgrade --install operation ./deployment/
```
5. To view logs:
```kubectl logs <pod-name> -f```

### Access the application

- APP UI: http://sms-checker.local/sms
- App Metrics: http://sms-checker.local/metrics

### Access the Kubernetes Dashboard

Add the following to your hosts file:
```
192.168.56.95  dashboard.local
```

Where `192.168.56.95` is the Nginx Ingress Controller's LoadBalancer IP.

Then navigate to `https://dashboard.local` in your browser.

Create a token for the `admin-user` service account to log in:

```bash
vagrant ssh ctrl --command "kubectl -n kubernetes-dashboard create token admin-user"
```

Paste the token into the login page of the dashboard and sign in.

## Monitoring with Prometheus and Grafana

The Helm chart includes **kube-prometheus-stack** which deploys Prometheus, Grafana, and AlertManager for monitoring.

### Custom Application Metrics

The **app service** exposes custom Prometheus metrics at `/metrics`:
- `sms_classification_requests_total{service="frontend"}` - Total classification requests
- `sms_classification_results_total{result="spam|ham"}` - Classification results by type
- `sms_current_message_length_chars{service="frontend",type="current"}` - Current message length (Gauge)
- `sms_classification_duration_milliseconds{service="frontend",operation="classify"}` - Response time distribution (Histogram)
- `sms_message_length_chars{service="frontend",metric_type="histogram"}` - Message length distribution (Histogram)

**Note:** The model-service does NOT expose metrics. Only the app service has custom metrics as per A3 requirements.

These metrics are automatically discovered by Prometheus via ServiceMonitor and can be queried in Prometheus or visualized in Grafana.

### Accessing Prometheus, Grafana, and AlertManager

All three are deployed as NodePort services:

```bash
# Get service ports
kubectl get svc -n operation | grep -E 'grafana|prometheus|alertmanager'
```

**Access URLs (using Vagrant cluster IPs):**
- **Grafana**: http://192.168.56.100:31765 (credentials: admin/admin)
- **Prometheus**: http://192.168.56.100:<PROMETHEUS_NODEPORT>
- **AlertManager**: http://192.168.56.100:30903

### Prometheus Metrics

The app service exposes custom Prometheus-format metrics at `/metrics` endpoint. All metrics are implemented manually (not using Prometheus client libraries) to meet course requirements.

**Query metrics in Grafana:**
1. Navigate to Explore (compass icon)
2. Enter metric name (e.g., `sms_classification_requests_total`)
3. Click "Run query" to visualize

**Example queries:**
```promql
# Current request rate (requests per minute)
rate(sms_classification_requests_total[1m]) * 60

# Spam detection percentage
sum(rate(sms_classification_results_total{result="spam"}[5m]))
/ sum(rate(sms_classification_results_total[5m])) * 100

# 95th percentile classification response time
histogram_quantile(0.95, rate(sms_classification_duration_milliseconds_bucket[5m]))
```

### AlertManager and Webhook Alerts

AlertManager is configured to send webhook notifications when critical conditions are detected.

**PrometheusRule:** The `HighRequestRate` alert triggers when the service receives more than 10 requests per minute for 10 consecutive seconds.

**Alert Configuration:**
- **Alert Name**: HighRequestRate
- **Condition**: `rate(sms_classification_requests_total[1m]) * 60 > 10`
- **Duration**: 10 seconds
- **Severity**: critical

**Webhook Configuration:** Alerts are sent to a webhook endpoint at `http://localhost:5001/webhook`. This allows for flexible alert handling and integration with external systems (Slack, PagerDuty, custom services, etc.).

**Testing Alerts:**

Generate sustained traffic to trigger the alert:

```bash
for i in {1..60}; do curl -s -X POST http://sms-checker.local/sms -H "Content-Type: application/json" -d '{"sms":"Test"}' > /dev/null & sleep 1; done
```

This sends 60 requests over 60 seconds (60 req/min), which exceeds the 10 req/min threshold. Within 10-20 seconds, check AlertManager to see the firing alert:

**AlertManager UI**: http://192.168.56.100:30903/#/alerts

The alert will automatically resolve when traffic drops below the threshold.

**Note:** The webhook endpoint `http://localhost:5001/webhook` is configured but not required to be running. Alerts will still appear in the AlertManager UI. Later we can implement a simple receiver service on port 5001, if we want to receive webhook notifications!



