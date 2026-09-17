# Instructions for Deploying, Exposing, and Testing ToDo App in Kubernetes

This guide describes how to apply the Kubernetes manifests for the ToDo application, configure namespaces, pods, and services (ClusterIP and NodePort), and test the application using various methods.

## 1. Apply All Manifests

Ensure your Kubernetes cluster (e.g., Minikube) is running, then apply the manifests located under the `.infrastructure` folder in the correct order:

1. **Create the Namespace:**
   ```bash
   kubectl apply -f .infrastructure/namespace.yml

Deploy the Pods (Main and Canary):
kubectl apply -f .infrastructure/todoapp-pod.yml
Deploy the Services (ClusterIP and NodePort):

kubectl apply -f .infrastructure/services.yml
Deploy the Busybox Container (for testing):

kubectl apply -f .infrastructure/busybox.yml

2. Testing via ClusterIP DNS from a Busybox Container
To test internal traffic balancing and name resolution through the ClusterIP service:

Open an interactive shell inside the running busybox pod:
kubectl exec -it busybox -n todoapp -- sh
Send a curl request to the ClusterIP service DNS name (traffic will be automatically balanced between the two pods):

curl [http://todoapp-clusterip-service.todoapp.svc.cluster.local/api/health/](http://todoapp-clusterip-service.todoapp.svc.cluster.local/api/health/)

Exit the container shell:
exit


3. Testing the Application Using Service port-forward
To access the ToDo application directly from your local machine via the service:

Forward local port 8000 to port 80 on the todoapp-clusterip-service:

kubectl port-forward service/todoapp-clusterip-service -n todoapp 8000:80
Open your browser or run curl in another terminal window:

Health Check: http://localhost:8000/api/health/

Readiness Check: http://localhost:8000/api/readiness/

4. Accessing the App Using NodePort Service
To access the application externally at the Node level:

If you are using Minikube, you can get the cluster IP and node port using:

minikube service todoapp-nodeport-service -n todoapp --url

Verify endpoints via curl:
curl http://<Node-IP>:30007/api/health/