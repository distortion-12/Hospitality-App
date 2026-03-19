# Datagami - Deployment Guide

Complete commands for running the Hospitality Booking application locally with Docker and Kubernetes.

---

## Table of Contents
1. [Docker Setup](#docker-setup)
2. [Docker Deployment](#docker-deployment)
3. [Kubernetes Setup](#kubernetes-setup)
4. [Kubernetes Deployment](#kubernetes-deployment)
5. [Troubleshooting](#troubleshooting)

---

## Docker Setup

### Prerequisites
- Docker Desktop installed and running
- Windows/Mac/Linux system

### Build Docker Image


# Build the Docker image
docker build -t datagami .
```

---

## Docker Deployment

### Run Single Container

```powershell
# Run in foreground (see logs in real-time)
docker run -p 8000:8000 datagami

# Run in background (detached mode)
docker run -d -p 8000:8000 --name hospitality datagami

# Access application at: http://localhost:8000git push -u origin main

```

### Run Multiple Containers (Different Ports)

```powershell
# First container on port 8000
docker run -d -p 8000:8000 --name hospitality datagami

# Second container on port 8001
docker run -d -p 8001:8000 --name alpha datagami

# Access:
# - hospitality: http://localhost:8000
# - alpha: http://localhost:8001
```

### Docker Management Commands

```powershell
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# View container logs
docker logs hospitality

# Follow logs in real-time
docker logs -f hospitality

# Stop container
docker stop hospitality

# Start stopped container
docker start hospitality

# Remove container
docker rm hospitality

# Remove image
docker rmi datagami

# Stop and remove all containers
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
```

---

## Kubernetes Setup

### Prerequisites
- Docker Desktop installed
- Kubernetes enabled in Docker Desktop

### Enable Kubernetes in Docker Desktop

1. Open Docker Desktop
2. Go to **Settings → Kubernetes**
3. Check **Enable Kubernetes**
4. Click **Apply & Restart**
5. Wait for startup to complete

### Verify Kubernetes Setup

```powershell
# Check kubectl version
kubectl version

# Check cluster nodes
kubectl get nodes

# Check cluster info
kubectl cluster-info
```

---

## Kubernetes Deployment

### Deploy Application to Kubernetes


# Apply all Kubernetes manifests
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

### Check Deployment Status

```powershell
# Check deployments
kubectl get deployments

# Check pods
kubectl get pods

# Check services
kubectl get svc

# Check ingress
kubectl get ingress

# Detailed pod info
kubectl describe pod <pod-name>

# Watch pods in real-time
kubectl get pods -w
```

### Access Application in Kubernetes

```powershell
# Port forward to access service locally
kubectl port-forward svc/hospitality-api 8000:80

# Then access: http://localhost:8000

# Or use service IP (if assigned)
kubectl get svc hospitality-api
```

### View Logs

```powershell
# View logs from all pods with label app=hospitality-api
kubectl logs -l app=hospitality-api

# Follow logs in real-time
kubectl logs -l app=hospitality-api -f

# View logs from specific pod
kubectl logs <pod-name>

# Follow logs from specific pod
kubectl logs -f <pod-name>
```

### Scale Deployment

```powershell
# Scale to 3 replicas
kubectl scale deployment hospitality-api --replicas=3

# Check scaled deployment
kubectl get pods
```

---

## Troubleshooting

### Issues & Solutions

#### Issue: Port already in use
```powershell
# Find process using port 8000
netstat -ano | findstr :8000

# Kill process (replace PID)
taskkill /PID <PID> /F
```

#### Issue: Docker daemon not running
```powershell
# Restart Docker daemon
Get-Service Docker | Restart-Service

# Or start Docker Desktop application
```

#### Issue: Container exits immediately
```powershell
# Check container logs
docker logs <container-name>

# Run container in foreground to see errors
docker run -p 8000:8000 datagami
```

#### Issue: Pod not running in Kubernetes
```powershell
# Check pod status
kubectl get pods

# Describe pod for events
kubectl describe pod <pod-name>

# Check pod logs
kubectl logs <pod-name>
```

#### Issue: Kubernetes image not found
```powershell
# Verify image is built locally
docker images | findstr datagami

# If not present, rebuild image
docker build -t datagami .
```

#### Issue: Cannot connect to application
```powershell
# Verify service is running
kubectl get svc hospitality-api

# Check port forwarding is active
kubectl port-forward svc/hospitality-api 8000:80

# Try accessing: http://localhost:8000
```

### Useful Debugging Commands

```powershell
# Get all Kubernetes resources
kubectl get all

# Get events in cluster
kubectl get events

# Describe deployment
kubectl describe deployment hospitality-api

# Get resource usage
kubectl top nodes
kubectl top pods
```

---

## Complete Quick Start

### Docker Quick Start

```powershell
cd c:\Users\ramch\Desktop\Datagami
docker build -t datagami .
docker run -d -p 8000:8000 --name hospitality datagami
# Access: http://localhost:8000
```

### Kubernetes Quick Start

```powershell
cd c:\Users\ramch\Desktop\Datagami

# Apply all configs
kubectl apply -f k8s/

# Port forward
kubectl port-forward svc/hospitality-api 8000:80

# Access: http://localhost:8000

# View logs
kubectl logs -l app=hospitality-api -f
```

---

## Cleanup

### Docker Cleanup

```powershell
# Stop all containers
docker stop $(docker ps -q)

# Remove all containers
docker rm $(docker ps -aq)

# Remove all images
docker rmi $(docker images -q)
```

### Kubernetes Cleanup

```powershell
# Delete all resources
kubectl delete -f k8s/

# Or delete specific resources
kubectl delete deployment hospitality-api
kubectl delete service hospitality-api
kubectl delete ingress hospitality-ingress
```

---

## Notes

- **API Base URL:** `http://localhost:8000`
- **Health Check:** `GET /healthz`
- **Availability Endpoint:** `GET /availability?room_type=standard`
- **Booking Endpoint:** `POST /book` (with JSON body)
- **Docker Container Runs Independently:** No terminal needed after `docker run -d`
- **Kubernetes Pods Auto-Restart:** Pods automatically restart on failure
- **Default Replicas:** 2 (configured in deployment.yaml)

---

## Contact & Support

For issues running this application, check:
1. Docker Desktop is running
2. Kubernetes is enabled (if using K8s)
3. Port 8000 is not already in use
4. Application logs (Docker or Kubernetes)
