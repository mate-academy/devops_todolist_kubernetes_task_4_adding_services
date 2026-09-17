# Deploying, Exposing, and Testing the ToDo App in Kubernetes

This guide walks through applying the Kubernetes manifests for the ToDo application — namespace, pods, and services (ClusterIP and NodePort) — and testing the deployment through several methods.

## Prerequisites

- A running Kubernetes cluster (e.g., Minikube)
- `kubectl` configured to point at the cluster
- Manifests available under the `.infrastructure/` directory

## 1. Apply the Manifests

Apply the manifests in the following order.

### 1.1 Create the namespace

```bash
kubectl apply -f .infrastructure/namespace.yml
```

### 1.2 Deploy the pods (main and canary)

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

### 1.3 Deploy the ClusterIP service

```bash
kubectl apply -f .infrastructure/clusterip.yml
```

### 1.4 Deploy the NodePort service

```bash
kubectl apply -f .infrastructure/nodeport.yml
```

### 1.5 Deploy the Busybox container (for testing)

```bash
kubectl apply -f .infrastructure/busybox.yml
```

## 2. Test via ClusterIP DNS from the Busybox Pod

Verify internal traffic routing and DNS resolution through the ClusterIP service.

1. Open an interactive shell inside the running Busybox pod:

   ```bash
   kubectl exec -it busybox -n todoapp -- sh
   ```

2. Send a request to the ClusterIP service's DNS name:

   ```bash
   curl http://todoapp-clusterip-service.todoapp.svc.cluster.local/api/health/
   ```

3. Exit the container shell:

   ```bash
   exit
   ```

## 3. Test via Service Port-Forwarding

Access the application directly from your local machine through the service.

1. Forward local port `8000` to port `80` on the ClusterIP service:

   ```bash
   kubectl port-forward service/todoapp-clusterip-service -n todoapp 8000:80
   ```

2. Open the endpoints in a browser or with `curl` in another terminal:

   | Check | URL |
   |---|---|
   | Health | http://localhost:8000/api/health/ |
   | Readiness | http://localhost:8000/api/readiness/ |

## 4. Access the App via NodePort

Reach the application externally at the node level.

1. Get the service URL via Minikube (if applicable):

   ```bash
   minikube service todoapp-nodeport-service -n todoapp --url
   ```

2. Verify the endpoint using the node IP and port `30007`:

   ```bash
   curl http://<Node-IP>:30007/api/health/
   ```