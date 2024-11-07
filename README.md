# Kubernetes TodoApp Deployment

## 1. Apply the manifests to create the namespace, TodoApp pods, and BusyBox pod:

Run the following command to apply all manifests within the ./.infrastructure directory:
```bash
kubectl apply -f ./.infrastructure
```

Or apply each manifest individually as follows:
```bash
kubectl apply -f ./.infrastructure/namespace.yml
kubectl apply -f ./.infrastructure/clusterip.yml
kubectl apply -f ./.infrastructure/nodeport.yml
kubectl apply -f ./.infrastructure/busybox.yml
kubectl apply -f ./.infrastructure/todoapp-pod.yml
```

## 2. Test the TodoApp using the ClusterIP service:

To test the TodoApp via the ClusterIP service, follow these steps:

1. Access the BusyBox shell:
```bash
kubectl -n todoapp exec -it busybox -- sh
```

2. Send a curl request to the ClusterIP service DNS:
```bash
curl http://todoapp-clusterip.todoapp.svc.cluster.local
```
This should return a response from the TodoApp if it’s running correctly.

## 3. Test the TodoApp using port-forwarding:

1. Test the TodoApp locally, use port-forwarding to access the service:
```bash
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
```

2. Open your web browser and test the TodoApp at:
http://localhost:8080

## 3. Test the TodoApp using the NodePort service in your web browser at:

http://localhost:30007
