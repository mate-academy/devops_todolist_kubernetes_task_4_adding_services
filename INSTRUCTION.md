## Apply Manifests

```bash
# Create the namespace
kubectl apply -f .infrastructure/namespace.yml

# Create the busybox pod for testing
kubectl apply -f .infrastructure/busybox.yml

# Create the todoapp pods
kubectl apply -f .infrastructure/todoapp-pod.yml

# Create the ClusterIP service
kubectl apply -f .infrastructure/clusterIp.yml

# Create the NodePort service
kubectl apply -f .infrastructure/nodePort.yml
```

After applying, you can check the status of the pods in the todoapp namespace:

```bash
kubectl get pods -n todoapp -o wide
```

## Test ClusterIP Service from BusyBox

This verifies that the application is reachable inside the cluster using the service DNS name.

1) Open a shell session in BusyBox:

```bash
kubectl -n todoapp exec -it busybox -- sh
```

2) Test connection using the ClusterIP service DNS name:

```bash
# Inside BusyBox shell:
curl todoapp-service.todoapp.svc.cluster.local
```

✅ You should see the application’s HTML or JSON response.

3) Exit the busybox shell: Type exit and press Enter.

```bash
# Inside the busybox shell:
exit
```

## Test ClusterIP Service using Port Forward

You can access the application locally from your machine by forwarding a local port to the service.

1) Run port-forward command:

```bash
kubectl port-forward svc/todoapp-service 8081:80 -n todoapp
```

2) Test from your local terminal or browser:

Using curl:

```bash
curl localhost:8081
```

Using a browser: Navigate to http://localhost:8081

## Test NodePort Service

Test from your local terminal or browser:

Using curl:

```bash
curl localhost:30007
```

Using a browser: Navigate to http://localhost:30007

## Cleanup

```bash
kubectl delete -f .infrastructure/busybox.yml
kubectl delete -f .infrastructure/todoapp-pod.yml
kubectl delete -f .infrastructure/clusterIp.yml
kubectl delete -f .infrastructure/nodePort.yml
kubectl delete -f .infrastructure/namespace.yml
```