# INSTRUCTION.md

## Prerequisites

Before testing the application, make sure that:
- Kubernetes cluster is running
- `kubectl` is configured and connected to the cluster
- The application and its Services are already deployed
- You know the namespace where the application is running

---

# 1. Test the application via ClusterIP Service DNS using a BusyBox container
## Connection busybox:
`kubectl -n todoapp exec -it busybox -- sh`
## Inside the BusyBox container, call the application using the ClusterIP Service DNS:
`curl http://todoapp-clusterip-service.todoapp.svc.cluster.local`
## Verify that the application returns a valid response.
## Exit the BusyBox container:
`exit`
---
# 2. Test the ToDo application using kubectl port-forward
## Forward a local port to the Service port:
`kubectl port-forward service/todoapp-clusterip-service 8081:80`
## Stop port-forwarding by pressing:
`Ctrl + C`
---
# 3. Access the application using a NodePort Service
## This method exposes the application outside the cluster via a node IP and NodePort.
## Apply the Service manifest:
`kubectl apply -f nodePort.yml`
## Verify that the Service is created:
`kubectl get svc`
## Access the application using the Node IP and NodePort:
`http://localhost:30007`