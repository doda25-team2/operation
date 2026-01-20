# Operation: Run the App + Model Service with Docker Compose

This repository provides a simple docker compose setup to run the two services used in the project:


**Quick start**

1. Make sure Docker is running on your machine.
2. From this repository root run:

```bash
docker compose up -d
```

3. Open your browser at `http://localhost:8080/sms` to access the SMS checker application. (Note: `http://localhost:8080` shows a simple "Hello World" health check)

To stop the services:

```bash
docker compose down
```

**Customizing image versions**

You can override image versions and ports using environment variables. Copy `.env.example` to `.env` and customize:

```bash
cp .env.example .env
# Edit .env to set custom image versions
docker compose up -d
```

Or set them directly:
```bash
MODEL_SERVICE_IMAGE=ghcr.io/doda25-team2/model-service:v1.0.0 docker compose up -d
```

**Rebuilding / developing locally**

If you need to build the images locally (for development or after changes), build and tag the images and then restart the compose stack.

Example (build locally and run):

```bash
# build app image
cd app
docker build -t ghcr.io/doda25-team2/app:latest .

# build model-service image
cd ../model-service
docker build -t ghcr.io/doda25-team2/model-service:latest .

# go back to repo root and start compose 
cd ../operation
docker compose up -d --build
```

## Deployment with Kubernetes

This application is deployed using a single Helm chart located in the `operation/` folder.  
It installs all microservices (app, model) along with their Services and Ingress routing.

### Prerequisites
- A running Kubernetes cluster (Docker Desktop, Minikube, Kind, etc.)
- NGINX Ingress Controller installed
    - ```bash
        helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
        helm install ingress-nginx ingress-nginx/ingress-nginx
- Add `127.0.0.1 sms-checker.local` to /etc/hosts.
- To bring up the services run:
    ```helm upgrade --install operation ./deployment/```
- To view logs:
    ```kubectl logs <pod-name> -f```

### Access the application
- App UI: http://sms-checker.local/
- Model API: http://sms-checker.local/predict
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
