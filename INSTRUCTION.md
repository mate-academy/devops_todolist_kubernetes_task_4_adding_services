# Kubernetes ToDo App Testing Instructions

This document provides instructions on how to test the deployed Django ToDo application in Kubernetes.

## 1. Test application calling ClusterIP DNS from a busybox container
To verify that internal load balancing is working via ClusterIP, run a temporary busybox pod and use `wget` to call the service:

```bash
# Start a busybox shell
kubectl run -i --tty busybox --image=busybox --restart=Never -- sh

# Inside the busybox container, run:
wget -qO- http://todo-clusterip