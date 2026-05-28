# Kubernetes ToDo Application

## Apply manifests

Create namespace:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Create busybox pod:

```bash
kubectl apply -f .infrastructure/busybox.yml
```

Create ToDo app pods:

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

Create services:

```bash
kubectl apply -f .infrastructure/clusterip-service.yml
kubectl apply -f .infrastructure/nodeport-service.yml
```

Check resources:

```bash
kubectl get pods -n todoapp
kubectl get svc -n todoapp
```

---

# Test ClusterIP Service DNS

Connect to busybox container:

```bash
kubectl exec -it busybox -n todoapp -- sh
```

Call ClusterIP service DNS:

```bash
curl http://todoapp-clusterip/ready
curl http://todoapp-clusterip/health
```

---

# Test Application Using Port Forward

Run port-forward for ClusterIP service:

```bash
kubectl port-forward svc/todoapp-clusterip 8080:80 -n todoapp
```

Open in browser:

```text
http://localhost:8080/ready
http://localhost:8080/health
```

---

# Access Application Using NodePort Service

Get node IP:

```bash
kubectl get nodes -o wide
```

Access application:

```text
http://NODE_IP:30080/ready
http://NODE_IP:30080/health
```
