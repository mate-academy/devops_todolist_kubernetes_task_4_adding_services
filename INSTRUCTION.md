# Kubernetes ToDo App Testing Instructions

This document provides instructions on how to test the deployed Django ToDo application in Kubernetes.

## 1. Test application calling ClusterIP DNS from a busybox container

To verify that internal load balancing is working via ClusterIP, run a temporary busybox pod and use wget to call the service:

```bash
# Start a busybox shell
kubectl run -i --tty busybox --image=busybox --restart=Never -- sh

# Inside the busybox container, run:
wget -qO- http://todo-clusterip
```

## 2. Test application using the port-forward command

You can access the application locally by port-forwarding the ClusterIP service:

``` bash
kubectl port-forward svc/todo-clusterip 8080:80
```

## 3. Access the application using NodePort Service

To access the application using the NodePort service, you need the IP of your Kubernetes node and the exposed NodePort (e.g., 30010).

```bash
 # Get the node IP
kubectl get nodes -o wide

# Access the application via browser or curl:
curl http://<node-ip>:30010
```