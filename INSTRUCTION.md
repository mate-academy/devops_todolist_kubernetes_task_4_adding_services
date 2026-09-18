# Todoapp Kubernetes Services

## Deploy the application

Apply the namespace, Pods, ClusterIP Service, NodePort Service, and BusyBox manifests:

kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/service.yml
kubectl apply -f .infrastructure/node-port.yml
kubectl apply -f .infrastructure/busybox.yml

## Verify the Pods

Check that both Todoapp Pods are running and ready:

kubectl get pods -n todoapp -o wide

## Verify the ClusterIP Service

Check that the Service selects both Todoapp Pods:

kubectl describe service service-todoapp -n todoapp

Open a shell inside the BusyBox Pod:

kubectl exec -it busybox -n todoapp -- /bin/sh

From BusyBox, verify access to the application through the Service DNS name:

curl service-todoapp

## Test the application using Service port-forward

Forward local port 8080 to port 80 of the ClusterIP Service:

kubectl port-forward service/service-todoapp 8080:80 -n todoapp

Open the application in a browser:

http://localhost:8080

## Verify the NodePort Service

Get the Node IP:

kubectl get nodes -o wide

The NodePort Service exposes the application on port 30000. From a client that can reach the Node network:

curl <NODE_IP>:30000
