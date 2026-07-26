# ToDo Application - Kubernetes Network Testing Guide

This document provides step-by-step instructions on how to validate network connectivity, service discovery, and external access for the ToDo application within the Kubernetes cluster.

## Prerequisites
Before testing, ensure all pods are in the `Running` state and services are successfully deployed:
```bash
kubectl get pods -l app=todolist
kubectl get svc
```

## 1. Testing Internal Service Discovery (ClusterIP)
To verify that intra-cluster communication works correctly, we will spin up an ephemeral `busybox` container and call the ClusterIP service using Kubernetes internal DNS.

Step 1: Launch a temporary interactive busybox pod:

```bash
kubectl run -i --tty --rm debug-pod --image=busybox --restart=Never -- sh
```

Step 2: Inside the container's shell, execute a request to the service's DNS name:

```bash
wget -qO- http://todoapp-service:80/api/health
```

(Note: The FQDN is `todoapp-service.todoapp.svc.cluster.local`, but the short name resolves automatically when in the same namespace).

Step 3: Type `exit` to leave and automatically delete the debug pod.

## 2. Testing via Local Tunnel (Port-Forward)
To debug or access the application locally without exposing it externally, use the port-forwarding method to the Service.

Step 1: Establish a secure tunnel to the ClusterIP service:


```Bash
kubectl port-forward svc/todoapp-service 8080:80
```

Step 2: Open a new terminal session (or your web browser) and verify the connection:

```Bash
curl http://localhost:8080/api/health
```

## 3. Testing External Access (NodePort)
To test how external clients will access the application at the node level, we will use the assigned NodePort (`30003`).

Step 1: Retrieve the external/internal IP address of your Kubernetes node:

```Bash
kubectl get nodes -o wide
```

Step 2: Access the application using the Node's IP and the defined NodePort:

```Bash
curl http://<NODE_IP>:30003/api/health
```