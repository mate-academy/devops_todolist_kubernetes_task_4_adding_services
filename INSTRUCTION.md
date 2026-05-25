# ToDo Application Testing Instructions

This guide describes how to verify and test the deployed Django ToDo application within the Kubernetes cluster using three different methods: via ClusterIP (DNS) using the custom BusyBox pod, the `port-forward` command, and the NodePort service.

Ensure all application resources are running before proceeding:
```
kubectl get pods,svc -n todoapp
```
## 1. Testing ClusterIP Service via BusyBox (Internal DNS)
The ClusterIP service balances traffic internally between the todoapp-1 and todoapp-2 pods using the cluster's internal DNS. We will use the pre-configured busybox pod with curl to verify this.

Deploy the busybox pod (if it is not already running):

```
kubectl apply -f busybox-pod.yml
```
Execute a curl command inside the busybox container to call the ClusterIP service DNS:

```
kubectl exec -it busybox -n todoapp -- curl [http://todoapp-clusterip-service.todoapp.svc.cluster.local/api/health](http://todoapp-clusterip-service.todoapp.svc.cluster.local/api/health)
```
Alternative short DNS (since the busybox pod is in the same todoapp namespace):

```
kubectl exec -it busybox -n todoapp -- curl http://todoapp-clusterip-service/api/health
```
Expected output: A successful HTTP response (e.g., status 200 OK or a health check JSON object) from the Django backend.

## 2. Testing Application using port-forward
You can forward a local port on your machine to the Kubernetes service to access the interactive web interface directly through your local browser.

Execute the port-forward command (maps your local port 8000 to the service port 80):

```
kubectl port-forward service/todoapp-clusterip-service 8000:80 -n todoapp
```
(Note: You can also use the ClusterIP service for forwarding: kubectl port-forward service/todoapp-clusterip-service 8000:80 -n todoapp)

Open your web browser and navigate to:
http://localhost:8000

Stop forwarding at any time by pressing Ctrl + C in your terminal.

## 3. Accessing the Application via NodePort Service
The NodePort service exposes the application on a static port (30008) across all Kubernetes worker nodes.

Or open your browser and access the UI via: http://localhost:30008