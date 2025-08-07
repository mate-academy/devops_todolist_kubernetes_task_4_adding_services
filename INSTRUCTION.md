# Instruction on how to access the app

## Testing app using busybox and calling ClusterIP

### 1. Create namespace

```bash
kubectl apply -f .infrastructure/namespace.yml
```

### 2. Create busybox pod

```bash
kubectl apply -f .infrastructure/busybox.yml
```

### 3. Create todoapp pods

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

### 4. Create ClusterIP service

```bash
kubectl apply -f .infrastructure/clusterIP.yml
```

### 5. Go inside busybox pod

```bash
kubectl exec -it busybox -n todoapp -- sh
```

### 6. Run curl command using ClusterIP DNS name

```bash
curl todoapp-service.todoapp.svc.cluster.local
```


## Testing app using port-forwarding

### 1. Verify that pods and ClusterIp service are running and ready

```bash
kubectl get pods -n todoapp
kubectl get svc -n todoapp
```

### 2. Apply port-forwarding

```bash
kubectl port-forward service/todoapp-service 8080:80 -n todoapp
```

### 3. Open browser at [http://localhost:8080](http://localhost:8080) address


## Access app using NodePort service

### 1. Verify that pods are running and ready

```bash
kubectl get pods -n todoapp
```

### 2. Create NodePort service

```bash
kubectl apply -f .infrastructure/nodePort.yml
```

### 3. Open browser at [http://localhost:30080](http://localhost:30080) address
