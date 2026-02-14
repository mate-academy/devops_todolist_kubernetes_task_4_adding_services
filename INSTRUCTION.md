# ToDo Application — Kubernetes Testing Instructions

This document describes how to test the ToDo application deployed on Kubernetes using different access methods.

**Prerequisites:** Ensure the namespace `todoapp`, pods, and services are applied:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterip.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl apply -f .infrastructure/busybox.yml
```

Wait until pods are ready: `kubectl get pods -n todoapp`

---

## 1. Test the app by calling the ClusterIP service DNS from a busybox container

The ClusterIP service `todoapp-service` is only reachable from inside the cluster. Use the busybox pod to call it via Kubernetes DNS.

**Option A — Using the short DNS name (same namespace):**

```bash
kubectl exec -n todoapp busybox -- curl -s http://todoapp-service/api/health
```

**Option B — Using the full DNS name:**

```bash
kubectl exec -n todoapp busybox -- curl -s http://todoapp-service.todoapp.svc.cluster.local/api/health
```

**Option C — Run an interactive shell in busybox and curl from there:**

```bash
kubectl exec -n todoapp -it busybox -- sh
# Inside the container:
curl http://todoapp-service/api/
curl http://todoapp-service/api/health
exit
```

The service listens on port **80** (mapped to pod port 8080). A successful response from `/api/health` indicates the app is reachable via ClusterIP DNS.

---

## 2. Test the ToDo application using the service `port-forward` command

`kubectl port-forward` exposes a ClusterIP (or any) service on your local machine so you can use the app in the browser.

**Port-forward the ClusterIP service to localhost:**

```bash
kubectl port-forward -n todoapp service/todoapp-service 8000:80
```

Then open in your browser:

- **Landing page:** http://localhost:8000/
- **API:** http://localhost:8000/api/
- **Health check:** http://localhost:8000/api/health

Stop the port-forward with `Ctrl+C`.

**Alternative — port-forward directly to a pod:**

```bash
kubectl port-forward -n todoapp pod/todoapp 8000:8080
```

Use the same URLs as above. Again, stop with `Ctrl+C` when done.

---

## 3. Access the app using the NodePort service

The NodePort service `todoapp-nodeport-service` exposes the app on each node’s IP at a fixed port (default in the manifest: **30007**).

**Find a node IP:**

```bash
kubectl get nodes -o wide
```

**Access the app:**

- From a machine that can reach the node’s IP (e.g. same network or VPN):
  - **Landing page:** http://\<NODE_IP\>:30007/
  - **API:** http://\<NODE_IP\>:30007/api/
  - **Health check:** http://\<NODE_IP\>:30007/api/health

**On kind or Docker Desktop Kubernetes**, use `localhost` if the NodePort is bound to the host:

```bash
# NodePort may be available at localhost:30007
open http://localhost:30007/
```

Replace `\<NODE_IP\>` with the actual node IP from `kubectl get nodes -o wide`.
