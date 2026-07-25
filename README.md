# Kubernetes Horizontal Pod Autoscaler (HPA) Hands-on Lab

## Project Overview

This project demonstrates how to deploy an application on Kubernetes and configure Horizontal Pod Autoscaler (HPA) based on CPU utilization.

The lab was performed on Docker Desktop Kubernetes using kubectl.

## Architecture

```
            User Load
                │
                ▼
          php-apache Service
                │
                ▼
          php-apache Pods
                ▲
                │
               HPA
                ▲
                │
         Metrics Server
                ▲
                │
             Kubelet
```
## Deployment File

```
php-apache.yaml
```

This file creates:

- Deployment
- Service
- CPU Requests and Limits

## Commands Used

### Deploy Application

```bash
kubectl apply -f php-apache.yaml
```

### Verify Resources

```bash
kubectl get deployment
kubectl get pods
kubectl get svc
```

### Create HPA

```bash
kubectl autoscale deployment php-apache --cpu=50% --min=1 --max=10
```

### Check HPA

```bash
kubectl get hpa
```

### Watch HPA

```bash
kubectl get hpa --watch
```

### Generate CPU Load

```bash
kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```

### Watch Pods

```bash
kubectl get pods --watch
```

### View CPU Usage

```bash
kubectl top pods
kubectl top nodes
```

### Delete Resources

```bash
kubectl delete -f php-apache.yaml
kubectl delete hpa php-apache
```

## Troubleshooting

### Problem

```
kubectl top pods

error: Metrics API not available
```

### Root Cause

The Metrics Server was not installed.

### Solution

- Installed the Metrics Server.
- Fixed TLS certificate validation by configuring it for the local Docker Desktop cluster.



## Key Learnings

- Deployments
- Services
- Resource Requests and Limits
- Horizontal Pod Autoscaler
- Metrics Server
- kubectl top
- Troubleshooting Metrics Server
- CPU-based Autoscaling
- Scale-up and Scale-down behavior
