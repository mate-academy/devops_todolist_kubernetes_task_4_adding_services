# ToDo Application Kubernetes Instructions

## Apply Kubernetes manifests

Create the namespace:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Create the ToDo application pods:

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

Create the BusyBox pod:

```bash
kubectl apply -f .infrastructure/busybox.yml
```

Create the ClusterIP service:

```bash
kubectl apply -f .infrastructure/clusterip-service.yml
```

Create the NodePort service:

```bash
kubectl apply -f .infrastructure/nodeport-service.yml
```

Check pods:

```bash
kubectl get pods -n todoapp
```

## Test application using ClusterIP Service DNS

The ClusterIP service is named:

```text
todoapp-clusterip
```

The service selects both ToDo application pods using the following label:

```text
app: todolist
```

Test the readiness endpoint from the BusyBox pod:

```bash
kubectl exec -n todoapp busybox -- curl http://todoapp-clusterip:8000/api/readiness/
```

## Test application using Service port-forward

Forward local port 8000 to the ClusterIP service:

```bash
kubectl port-forward -n todoapp service/todoapp-clusterip 8000:8000
```

Open the application:

```text
http://localhost:8000/
```

Readiness endpoint:

```text
http://localhost:8000/api/readiness/
```

Liveness endpoint:

```text
http://localhost:8000/api/liveness/
```

## Access application using NodePort Service

Check the NodePort service:

```bash
kubectl get service todoapp-nodeport -n todoapp
```

Check node information:

```bash
kubectl get nodes -o wide
```

The NodePort service exposes the application on port:

```text
30080
```

Access the application using:

```text
http://<NODE_IP>:30080/
```

Readiness endpoint:

```text
http://<NODE_IP>:30080/api/readiness/
```

Liveness endpoint:

```text
http://<NODE_IP>:30080/api/liveness/
```



