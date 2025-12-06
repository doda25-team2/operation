# Operation: Run the App + Model Service with Docker Compose

This repository provides a simple docker compose setup to run the two services used in the project:


**Quick start**

1. Make sure Docker is running on your machine.
2. From this repository root run:

```bash
docker compose up -d
```

3. Open your browser at `http://localhost:8080` to access the `app` frontend.

To stop the services:

```bash
docker compose down
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
- App: http://sms-checker.local/
- Model API: http://sms-checker.local/model

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
