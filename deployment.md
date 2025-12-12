# Deployment Documentation
## Hostnames

App UI: http://sms-checker.local/

Model API: http://sms-checker.local/predict

## Ports

- HTTP port 80 used for web access via the Ingress gateway / local domain host entry.

- Prometheus, Grafana, AlertManager are exposed as NodePort services

# Paths
- / : main frontend app
- /predict : model-service prediction endpoint
- /metrics : app metrics for Prometheus scraping

# Data Pathway
- Client to Ingress Gateway
- Browser/curl hits sms-checker.local (HTTP port).
- Ingress Gateway t0 VirtualService
- Istio Gateway routes based on host/path.
- VirtualService uses DestinationRule
- DestinationRule routes to app-service v1 or v2, requests are forwarded to one of the version subsets.
- app-service to model-service, the app service calls the model service.
- app-service to Prometheus
- Metrics from the app  are scraped by Prometheus.
- Prometheus to Grafana
- Grafana dashboards display metrics for analysis.