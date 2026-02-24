# ToDo App – Kubernetes Testing Instructions

## Prerequisites

Make sure `kubectl` is configured and connected to your cluster. Apply all manifests in the following order:

```bash
kubectl apply -f namespace.yml
kubectl apply -f todoapp-pod.yml
kubectl apply -f clusterip-service.yml
kubectl apply -f nodeport-service.yml
kubectl apply -f busybox.yml
```

Verify that the pods and services are running:

```bash
kubectl get pods -n todoapp -l app=todolist
kubectl get services -n todoapp
```

Wait until both pods (`todoapp-1` and `todoapp-2`) show a `Running` status.

---

## 1. Testing via ClusterIP Service DNS from a BusyBox Container

ClusterIP services are only reachable from within the cluster. Use the already deployed `busybox` pod in the `todoapp` namespace to test connectivity using the service's internal DNS name.

**Step 1 — Open an interactive shell inside the busybox pod:**

```bash
kubectl exec -it busybox -n todoapp -- sh
```

**Step 2 — Inside the shell, call the ClusterIP service using its DNS name:**

```sh
# Format: <service-name>.<namespace>.svc.cluster.local
curl http://todoapp-clusterip.todoapp.svc.cluster.local/
```

> Kubernetes automatically resolves the DNS name `todoapp-clusterip.todoapp.svc.cluster.local` and load-balances requests between `todoapp-1` and `todoapp-2`.

**Step 3 — Test the API endpoint:**

```sh
curl http://todoapp-clusterip.todoapp.svc.cluster.local/api/
```

**Step 4 — Test the health endpoint:**

```sh
curl http://todoapp-clusterip.todoapp.svc.cluster.local/api/health
```

**Step 5 — Exit the shell:**

```sh
exit
```

---

## 2. Testing via `port-forward`

`kubectl port-forward` lets you access a service directly from your local machine without exposing it publicly.

**Forward the ClusterIP service to local port 8080:**

```bash
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
```

**Open your browser or use curl:**

```bash
curl http://localhost:8080/
curl http://localhost:8080/api/
curl http://localhost:8080/api/health
```

Or navigate to [http://localhost:8080](http://localhost:8080) in your browser.

> Press `Ctrl+C` to stop port forwarding when done.

You can also port-forward directly to a specific pod:

```bash
kubectl port-forward pod/todoapp-1 8080:8080 -n todoapp
```

---

## 3. Accessing the App via NodePort Service

The NodePort service exposes the application on every node in the cluster on port `30080`.

**Step 1 — Get the Node IP address:**

```bash
kubectl get nodes -o wide
```

Look for the `EXTERNAL-IP` or `INTERNAL-IP` column of any node.

**Step 2 — Access the application:**

```bash
# Replace <NODE_IP> with the actual IP of any cluster node
curl http://<NODE_IP>:30080/
curl http://<NODE_IP>:30080/api/
```

Or open `http://<NODE_IP>:30080` in your browser.

> If you are using **Minikube**, get the correct URL with:
> ```bash
> minikube service todoapp-nodeport -n todoapp --url
> ```

> If you are using **Kind**, use the internal node IP from `kubectl get nodes -o wide`.

---

## Environment Variables Reference

The following environment variables are set on each container in `todoapp-pod.yml`, based on `settings.py`:

| Variable       | Description               | Value                                             |
|----------------|---------------------------|---------------------------------------------------|
| `SECRET_KEY`   | Django secret key         | `@e2(yx)v&tgh3_s=0yja-i!dpebxsz^dg47x)-k&kq_3zf*9e*` |
| `DEBUG`        | Enable debug mode         | `True`                                            |
| `ALLOWED_HOSTS`| Allowed host headers      | `*`                                               |
| `DB_ENGINE`    | Database backend engine   | `django.db.backends.sqlite3`                      |
| `DB_NAME`      | Database file name        | `db.sqlite3`                                      |

> **Note:** For production, replace `SECRET_KEY` with a strong random value and set `DEBUG=False`.