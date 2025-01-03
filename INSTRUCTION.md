# Work with Kubernetes

## Test an app via a `ClusterIP` service

### A helper container

1. Apply the `clusterip.yml` manifest:

```bash
kubectl apply -f clusterip.yml
```

2. Connect to the helper container:

```bash
kubectl exec -it -n todoapp pod/busybox -- sh
```

3. Make a request to the app with the `curl` command

```bash
curl http://todoapp-service.todoapp.svc.cluster.local
```

### A port forwarding

1. Forward the ports:

```bash
kubectl port-forward service/todoapp-service 8081:80
```

2. Open the following URL in a web browser `http://localhost:8081/`

## Access the app via the `NodePort` service

1. Apply the `nodeport.yml` manifest:

```bash
kubectl apply -f nodeport.yml
```

2. Open the following URL in a web browser `http://localhost:30007/`
