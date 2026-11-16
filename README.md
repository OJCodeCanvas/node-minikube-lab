# Node.js Minikube Kubernetes Lab

A local Kubernetes deployment and orchestration lab running a containerized Node.js web service (`server.js`) on Minikube. Built to practice Kubernetes manifest configuration, local container builds, service exposure, and pod lifecycle troubleshooting.

## Project Structure
- `server.js` — Node.js HTTP/REST application
- `package.json` — Node.js dependencies and startup scripts
- `Dockerfile` — Container image definition for the Node.js app
- `k8s-node-app.yaml` — Kubernetes Deployment and Service manifests

## Prerequisites
- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Quick Start

1. **Start the local Minikube cluster:**
   ```bash
   minikube start --driver=docker
   ```

2. **Point your terminal to Minikube Docker daemon and build the image:**
   ```bash
   eval $(minikube docker-env)
   docker build -t node-minikube-app:latest .
   ```

3. **Deploy the application and service:**
   ```bash
   kubectl apply -f k8s-node-app.yaml
   ```

4. **Verify pod status and service routing:**
   ```bash
   kubectl get pods,svc
   ```

5. **Expose and access the service URL:**
   ```bash
   minikube service --all --url
   ```

## Operational & Troubleshooting Notes
- **Local Image Resolution (`ImagePullBackOff`):** Ensure the image tag in `k8s-node-app.yaml` matches the tag built inside `eval $(minikube docker-env)` and uses `imagePullPolicy: IfNotPresent` or `Never`.
- **Inspecting Pod Logs & Events:**
  ```bash
  kubectl describe pod -l app=node-app
  kubectl logs -l app=node-app
  ```
- **Teardown:**
  ```bash
  kubectl delete -f k8s-node-app.yaml
  minikube stop
  ```
