
# Django ToDo — Working with Kubernetes Services

## Prerequisites

- A running Kubernetes cluster (Docker Desktop).
- kubectl configured to access the cluster.
- Run all commands from the root of this repository.
- The BusyBox Pod uses the `ikulyk404/busyboxplus:curl` image, which includes curl. The standard BusyBox image does not normally include curl.

## 1. Deploy the application

Create the namespace:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Apply all Kubernetes manifests:

```bash
kubectl apply -f .infrastructure/
```

Check the Pods and Services:

```bash
kubectl get pods,svc -n todoapp
```

Wait until both application Pods are ready:

```bash
kubectl get pods -n todoapp -l app=todolist
```

Expected result:

```text
NAME        READY   STATUS    RESTARTS
todoapp     1/1     Running   0
todoapp-1   1/1     Running   0
```

Both Pods have the label `app: todolist`.

The `PYTHONUNBUFFERED` environment variable is configured in both Pod manifests.

## 2. Application port configuration

The default Django development server port is 8000, as mentioned in README.md.

However, the application inside the Docker container explicitly listens on port 8080.

The Dockerfile uses:

```dockerfile
ENTRYPOINT ["python", "manage.py", "runserver", "0.0.0.0:8080"]
```

Both Pod specifications also explicitly define the startup command:

```yaml
command: ["python", "manage.py", "runserver", "0.0.0.0:8080"]
```

Therefore, both Kubernetes Services use:

```yaml
port: 80
targetPort: 8080
```

The Services are configured as follows:

| Service | Type | Port | TargetPort | NodePort |
|---|---|---|---|---|
| todolist-service | ClusterIP | 80 | 8080 | - |
| todoapp-service | NodePort | 80 | 8080 | 30008 |

Both Services select Pods using the label `app: todolist`.

## 3. Test the ClusterIP Service from BusyBox

The BusyBox manifest uses the following curl-enabled image:

```yaml
image: ikulyk404/busyboxplus:curl
```

Verify that curl is available:

```bash
kubectl exec -n todoapp busybox -- curl --version
```

Open an interactive shell inside the BusyBox container:

```bash
kubectl exec -it busybox -n todoapp -- sh
```

Send an HTTP request to the ClusterIP Service using its DNS name:

```bash
curl -i http://todolist-service.todoapp.svc.cluster.local/
```

The application should return a successful HTTP response and the ToDo landing page.

Test the API:

```bash
curl -i http://todolist-service.todoapp.svc.cluster.local/api/
```

The ClusterIP Service provides a stable DNS name and routes requests to the available application Pods.

## 4. Verify traffic distribution

While inside the BusyBox container, send multiple HTTP requests:

```bash
for i in 1 2 3 4 5 6 7 8 9 10; do
  curl -s -o /dev/null -w '%{http_code}\n' http://todolist-service.todoapp.svc.cluster.local/
done
```

Exit the BusyBox shell:

```bash
exit
```

Check the logs of both application Pods:

```bash
kubectl logs todoapp -n todoapp
kubectl logs todoapp-1 -n todoapp
```

Requests should be distributed between the two available Pods. Traffic distribution does not necessarily alternate evenly between Pods.

Check the Service endpoints:

```bash
kubectl get endpointslice -n todoapp -l kubernetes.io/service-name=todolist-service -o wide
```

The output should include the IP addresses of both application Pods.

## 5. Test the application using Service port-forward

Run:

```bash
kubectl port-forward service/todolist-service 8086:80 -n todoapp
```

Keep the terminal running and open the application in your browser:

http://localhost:8086/

You can also access the API:

http://localhost:8086/api/

Alternatively, use curl from another terminal:

```bash
curl -i http://localhost:8086/
```

The local port 8086 is forwarded to Service port 80, which maps to the application port 8080.

Press Ctrl+C to stop port-forwarding.

Note: Service port-forward selects one Pod for the session and does not demonstrate load balancing between multiple Pods.

## 6. Access the application using NodePort

The NodePort Service exposes the application on port 30008.

For Docker Desktop, open:

http://localhost:30008/

API endpoint:

http://localhost:30008/api/

Alternatively, run:

```bash
curl -i http://localhost:30008/
```

No port-forward command is required.

For another Kubernetes environment, retrieve the node IP address:

```bash
kubectl get nodes -o wide
```

Access the application using:

```text
http://<NodeIP>:30008/
```

The node must be reachable from your local machine, and the network must allow access to the NodePort.

## 7. Troubleshooting

Check the status of all Pods and Services:

```bash
kubectl get pods,svc -n todoapp
```

Inspect application Pods:

```bash
kubectl describe pod todoapp -n todoapp
kubectl describe pod todoapp-1 -n todoapp
```

Check application logs:

```bash
kubectl logs todoapp -n todoapp
kubectl logs todoapp-1 -n todoapp
```

Inspect Services:

```bash
kubectl describe service todolist-service -n todoapp
kubectl describe service todoapp-service -n todoapp
```

If the BusyBox Pod already exists with a different configuration, recreate it:

```bash
kubectl delete pod busybox -n todoapp
kubectl apply -f .infrastructure/busybox.yml
```

If the application Pod startup command was changed, recreate both application Pods:

```bash
kubectl delete pod todoapp todoapp-1 -n todoapp
kubectl apply -f .infrastructure/todoapp-pod.yml
```

Verify that both Pods are running and ready before testing the Services again.

Note: This learning application uses SQLite inside each container. The two Pods do not share application data. A production application should use a shared database and appropriate persistent storage.
