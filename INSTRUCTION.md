# ToDo App - Kubernetes Testing Instructions

This document provides step-by-step instructions to verify the connectivity and functionality of the ToDo application deployed in the Kubernetes cluster.

## 1. Test using ClusterIP Service from a Busybox container

This test verifies internal cluster DNS resolution and connectivity between pods.

1. Apply the busybox pod manifest if it's not already running:
   `kubectl apply -f .infrastructure/busybox.yml`
2. Execute an interactive shell inside the busybox pod:
   `kubectl exec -it busybox -- sh`
3. Call the ClusterIP service using its DNS name:
   `wget -qO- http://clusterIP-service:80`
   
*Expected result: You should see the HTML output of the ToDo application.*

## 2. Test using the `kubectl port-forward` command

This test verifies the application by forwarding traffic from your local machine directly to the ClusterIP service.

1. Run the port-forward command in your terminal:
   `kubectl port-forward svc/clusterIP-service 8080:80`
2. Open your web browser or use `curl` in a new terminal window:
   `curl http://localhost:8080`
   
*Expected result: The application should respond successfully on your local port 8080.*

## 3. Test using a NodePort Service

This test verifies external access to the application through the Node's IP address.

1. Find the NodePort assigned to your service:
   `kubectl get svc nodePort-service`
2. Get the IP address of your Minikube node:
   `minikube ip`
3. Access the application using the Node's IP and the assigned NodePort (e.g., if Node IP is `192.168.49.2` and NodePort is `30007`):
   `curl http://192.168.49.2:30007`
   
*Expected result: The application should be accessible from "outside" the cluster using the Node's IP and NodePort.*