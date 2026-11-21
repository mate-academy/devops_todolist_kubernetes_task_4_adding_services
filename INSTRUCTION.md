# Testing the ToDo application services

Run these commands from the repository root with `kubectl` configured to use a running Kubernetes cluster.

## Deploy the application and services

```sh
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl -n todoapp wait --for=condition=Ready pod/todoapp-2 pod/busybox --timeout=180s
kubectl -n todoapp get pods,services,endpoints
```

Confirm that `todoapp-service` and `todoapp-nodeport-service` have a ready endpoint before testing. The BusyBox pod sleeps for 1,000 seconds; if it has stopped, delete it and apply its manifest again.

## Call the ClusterIP service from BusyBox

The service DNS name below resolves inside the cluster. Port 80 on the service forwards to port 8000 on a matching application pod.

```sh
kubectl -n todoapp exec busybox -- wget -qO- http://todoapp-service.todoapp.svc.cluster.local/
```

The response should contain the application's HTML. You can also request `/api/` to see the API.

## Access the service with port-forward

Run this command and leave it running:

```sh
kubectl -n todoapp port-forward service/todoapp-service 8080:80
```

Open http://localhost:8080/ in a browser, or request http://localhost:8080/api/. Press Ctrl+C in the port-forward terminal when finished.

## Access the NodePort service

Find an IP address for a cluster node:

```sh
kubectl get nodes -o wide
```

From a machine that can reach that node, open `http://<node-ip>:30080/` or `http://<node-ip>:30080/api/`. The NodePort service forwards port 30080 to port 8000 on a matching application pod. Node reachability depends on your cluster and network configuration; for example, with Minikube, `minikube service todoapp-nodeport-service -n todoapp --url` can provide a reachable URL.
