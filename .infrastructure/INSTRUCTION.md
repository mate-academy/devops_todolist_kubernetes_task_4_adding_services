## How to Apply Manifests
```
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod1.yml
kubectl apply -f .infrastructure/todoapp-pod2.yml
kubectl apply -f .infrastructure/clusterip-service.yml
kubectl apply -f .infrastructure/nodeport-service.yml
```
## How to test by calling a ClusterIP service
```
kubectl -n todoapp exec -it busybox -- sh
curl http://todoapp-service:80
```
## Testing the Application Using Port-Forward
1. Port-forward the ClusterIP service:
````
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
````
2. Open your browser or use curl:
````
curl http://localhost:8080
````
## Accessing the Application Using NodePort Service
1. Get the Node IP address:
````
kubectl get nodes -o wide
````
2. Access the application using the Node's IP and NodePort (e.g., 30007):
````
curl http://<node-ip>:30007
````
