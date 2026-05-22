# TodoApp Testing Instructions

## 1. Apply all manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterip-service.yml
kubectl apply -f .infrastructure/nodeport-service.yml
kubectl apply -f .infrastructure/busybox.yml
```

Verify that all pods are running:

```bash
kubectl get pods -n todoapp
```

---

## 2. Testing via ClusterIP DNS from busybox container

ClusterIP is only accessible inside the cluster. To test it, exec into the busybox pod:

```bash
kubectl exec -it busybox -n todoapp -- sh
```

Inside the container, run curl using the service DNS name:

```bash
curl http://todoapp-clusterip.todoapp.svc.cluster.local/api/health
```

DNS format: `<service-name>.<namespace>.svc.cluster.local`

---

## 3. Testing via port-forward

```bash
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
```

Open in browser or run:

```bash
curl http://localhost:8080
```

---

## 4. Accessing the app via NodePort

NodePort exposes the application at the node level on port `30080`.

In Docker Desktop the node is accessible via `localhost`:

```bash
curl http://localhost:30080
```

Or open in browser: `http://localhost:30080`
