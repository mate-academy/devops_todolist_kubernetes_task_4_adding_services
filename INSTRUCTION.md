# ToDo app deployment and testing instructions

## Prerequisites

Docker and Kubernetes must be installed (minikube, Docker Desktop Kubernetes, or another local cluster).

## Apply manifests

From the repository root, run:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
```

Verify that pods are `Running` and `Ready`:

```bash
kubectl get pods -n todoapp
kubectl get svc -n todoapp
```

## Test via ClusterIP (DNS from busybox)

ClusterIP is reachable only inside the cluster. In namespace `todoapp`, use the service short DNS name or full FQDN.

Short name:

```text
todoapp-clusterip-service
```

Full name (FQDN):

```text
todoapp-clusterip-service.todoapp.svc.cluster.local
```

Service port: **80** (traffic is forwarded to container port `8080`).

Exec into the busybox pod and run `curl`:

```bash
kubectl exec -n todoapp -it busybox -- sh
```

Inside the container:

```bash
curl -sS http://todoapp-clusterip-service/api/health
curl -sS http://todoapp-clusterip-service.todoapp.svc.cluster.local/api/health
```

You should get a successful response from the health endpoint. Exit the shell with `exit`.

One-liner without an interactive shell:

```bash
kubectl exec -n todoapp busybox -- curl -sS http://todoapp-clusterip-service/api/health
```

## Test via port-forward (Service)

Forward the ClusterIP **service** port to your local machine:

```bash
kubectl port-forward -n todoapp service/todoapp-clusterip-service 8080:80
```

In another terminal (while `port-forward` is running):

```bash
curl -sS http://127.0.0.1:8080/api/health
```

Or open in a browser: [http://127.0.0.1:8080/](http://127.0.0.1:8080/)

## Access via NodePort

Service `todoapp-nodeport-service` exposes the app on node port **30007** (mapped to service port `80`).

Find the cluster node IP.

**Minikube:**

```bash
minikube ip
```

**Docker Desktop / other clusters:** use the worker node IP or the address documented for your provider.

Open in a browser or test with `curl`:

```bash
curl -sS http://<NODE_IP>:30007/api/health
```

Minikube example (replace with the IP from `minikube ip`):

```bash
curl -sS http://192.168.49.2:30007/api/health
```

## Useful commands

Application logs:

```bash
kubectl logs -n todoapp todoapp
kubectl logs -n todoapp todoapp-2
```

Service endpoints (should list two pods with label `app: todolist`):

```bash
kubectl get endpoints -n todoapp todoapp-clusterip-service
```
