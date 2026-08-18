# ToDo App Kubernetes Testing Instructions

This document provides instructions on how to test the ToDo application running in the Kubernetes cluster.

## 1. Test using ClusterIP Service DNS from a Busybox container

To verify internal cluster connectivity using the ClusterIP service, we can spin up a temporary Busybox pod and use `wget` to call the service's DNS name.

1. Run a temporary Busybox pod in the same cluster:
   ```bash
   kubectl run -i --tty --rm debug-pod --image=busybox --restart=Never -n todoapp -- sh
   ```
2. Once inside the shell, execute the following command to test the connection via the service DNS:
   ```bash
   wget -qO- http://service-clusterip
   ```
3. You should see the HTML output of the ToDo app. Type `exit` to leave and automatically delete the busybox pod.

## 2. Test using the Service Port-Forward command

You can use the `port-forward` command to securely access the application from your local machine without exposing it externally.

1. Run the port-forward command against the ClusterIP service:
   ```bash
   kubectl port-forward svc/service-clusterip 8080:80 -n todoapp
   ```
2. Open your web browser and navigate to:
   ```text
   http://localhost:8080
   ```
3. To stop the port-forwarding, simply press `Ctrl+C` in the terminal.

## 3. Access the app using the NodePort Service

The NodePort service exposes the application on a specific port across all nodes in the cluster.

1. First, find the IP address of your Kubernetes node:
   ```bash
   kubectl get nodes -o wide
   ```
2. Next, check the exact NodePort assigned to your service:
   ```bash
   kubectl get svc service-nodeport -n todoapp
   ```
3. Open your web browser and navigate to:
   ```text
   http://<NODE-IP>:<NODEPORT>
   ```