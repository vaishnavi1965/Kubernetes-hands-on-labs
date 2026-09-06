````markdown
# Kubernetes ConfigMap Demo

This demo demonstrates how to use a Kubernetes ConfigMap to store application configuration and inject it into a Pod as environment variables.

## Architecture

ConfigMap → Deployment → Pod → Nginx Container

## Components

### ConfigMap

The ConfigMap stores non-sensitive application configuration:

- `APP_NAME`
- `APP_ENV`
- `APP_MESSAGE`
- `APP_VERSION`

### Deployment

The Deployment runs an Nginx container and imports the ConfigMap values as environment variables using `envFrom`.

## Deploy

Create the namespace:

```bash
kubectl create namespace config-demo
````

Apply the ConfigMap:

```bash
kubectl apply -f configmap.yaml
```

Apply the Deployment:

```bash
kubectl apply -f deployment.yaml
```

## Verify

Check the ConfigMap:

```bash
kubectl get configmap -n config-demo
```

Check the Deployment:

```bash
kubectl get deployment -n config-demo
```

Check the Pod:

```bash
kubectl get pods -n config-demo
```

View the environment variables inside the container:

```bash
kubectl exec -it <pod-name> -n config-demo -- env
```

Expected values:

```text
APP_NAME=demo-app
APP_ENV=dev
APP_MESSAGE=Hello from Kubernetes ConfigMap
APP_VERSION=1.0
```

## ConfigMap Update

If the ConfigMap is updated, environment variables inside an already-running container do not automatically change.

Restart the Deployment to load the updated values:

```bash
kubectl rollout restart deployment config-demo -n config-demo
```

## Cleanup

```bash
kubectl delete namespace config-demo
```

```
```
