# Django ToDo — Working with Kubernetes Services

## Prerequisites

- A running Kubernetes cluster (for example, Docker Desktop) and `kubectl` configured to access it.
- Run the commands from the root of this repository.
- If you previously deployed another Busybox Pod named `busybox` in the `todoapp` namespace, delete that Pod before applying the new Busybox manifest if Kubernetes reports an immutable-field error.

## 1. Deploy the application

Create the namespace first, then apply all manifests:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/
```

If the existing Busybox Pod prevents the new manifest from being applied:

```bash
kubectl delete pod busybox -n todoapp
kubectl apply -f .infrastructure/busybox.yml
```

Check that the Pods and Services have been created:

```bash
kubectl get pods,svc -n todoapp
kubectl get pods -n todoapp -l app=todolist
```

Wait until `todoapp` and `todoapp-1` both show `1/1 Running`. Both application Pods have the label `app: todolist`, use container port `8080`, and receive `PYTHONUNBUFFERED=1` from the Pod manifest.

The Services are:

| Service | Type | Port | Target port | Node port |
| --- | --- | --- | --- | --- |
| `todolist-service` | ClusterIP | 80 | 8080 | — |
| `todoapp-service` | NodePort | 80 | 8080 | 30008 |

Both Services select the application Pods using `app: todolist`.

## 2. Test the ClusterIP Service from Busybox

Open an interactive shell in the Busybox Pod:

```bash
kubectl exec -it busybox -n todoapp -- sh
```

Inside the container, send an HTTP request using the ClusterIP Service DNS name (assuming the default cluster DNS domain, `cluster.local`):

```bash
curl -i http://todolist-service.todoapp.svc.cluster.local/
```

The application should return a successful HTTP response and the ToDo landing page. The URL uses the Service port `80`, not the container port `8080`.

You can also check the application API:

```bash
curl -i http://todolist-service.todoapp.svc.cluster.local/api/
```

To exit the Busybox shell, run:

```bash
exit
```

### Check both Service backends

```bash
kubectl get pods -n todoapp -l app=todolist -o wide
kubectl get endpointslice -n todoapp -l kubernetes.io/service-name=todolist-service -o wide
```

The EndpointSlice output should include the IP addresses of both application Pods. To inspect their readiness conditions, add `-o yaml` instead of `-o wide` to the EndpointSlice command.

To exercise traffic distribution, send several requests from Busybox (in a shell opened with the `kubectl exec` command above):

```bash
for i in 1 2 3 4 5 6 7 8 9 10; do
  curl -s -o /dev/null -w '%{http_code}\n' http://todolist-service.todoapp.svc.cluster.local/
done
```

Exit Busybox and inspect the application logs:

```bash
kubectl logs todoapp -n todoapp --tail=30
kubectl logs todoapp-1 -n todoapp --tail=30
```

Look for requests to `/` in the logs of both Pods. Traffic distribution is not guaranteed to alternate evenly between Pods on every request.

## 3. Test the application using Service port-forward

From your local terminal, run:

```bash
kubectl port-forward service/todolist-service 8086:80 -n todoapp
```

Keep the terminal open and visit:

- http://localhost:8086/ — ToDo application
- http://localhost:8086/api/ — application API

Alternatively, in another terminal:

```bash
curl -i http://localhost:8086/
```

Here, `8086` is the local port and `80` is the Service port. Kubernetes resolves the Service to a selected Pod and forwards to its target port `8080`.

Stop port-forwarding with `Ctrl+C`. Service port-forward selects one Pod for the session; it is not a test of load balancing across both Pods.

## 4. Access the application using the NodePort Service

The `todoapp-service` NodePort Service exposes the application on port `30008`.

For Docker Desktop, open:

- http://localhost:30008/ — ToDo application
- http://localhost:30008/api/ — application API

No `kubectl port-forward` is required. You can also test with:

```bash
curl -i http://localhost:30008/
```

For another Kubernetes environment, find a reachable node address:

```bash
kubectl get nodes -o wide
```

Use `http://<NodeIP>:30008/`, provided your network and firewall allow access to that node port.

## 5. Troubleshooting

If the application is not accessible, check Pod readiness, Service selectors, and available endpoints:

```bash
kubectl get pods,svc -n todoapp
kubectl describe pod todoapp -n todoapp
kubectl describe pod todoapp-1 -n todoapp
kubectl describe service todolist-service -n todoapp
kubectl describe service todoapp-service -n todoapp
kubectl logs todoapp -n todoapp
kubectl logs todoapp-1 -n todoapp
```

Both application Pods should be ready, and both Services should select Pods with `app: todolist`.

**Note:** this learning project uses SQLite inside each container. The two Pods do not share application data. Use the landing page or read-only requests to test Service connectivity; a production application with multiple replicas should use a shared database and appropriate persistence.
