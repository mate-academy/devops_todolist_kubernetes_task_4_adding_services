# Instructions

## Creating pods

### 1. Create namespace

```bash
kubectl apply -f .infrastructure/namespace.yml
```

### 2. Create busybox pod

```bash
kubectl apply -f .infrastructure/busybox.yml
```

### 3. Create services pods

```bash
kubectl apply -f .infrastructure/clusterIp.yml
```
```bash
kubectl apply -f .infrastructure/nodeport.yml
```

### 4. Create todoapp pods

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

## Testing ClusterIp

### 1. Enter busybox in interactive mode

```bash
kubectl exec -it busybox -n todoapp -- sh
```

### 2. Run curl command using ClusterIp DNS name (<service-name>.<namespace>.svc.cluster.local)

```bash
curl todoapp-service.todoapp.svc.cluster.local
```

## Using port-forward

### 1. Configure port-forwarding

```bash
kubectl port-forward service/todoapp-service 8080:80 -n todoapp
```

### 2. Open browser at [http://localhost:8080](http://localhost:8080) address

## Testing NodePort

### 1. Open browser at [http://localhost:30080](http://localhost:30080) address