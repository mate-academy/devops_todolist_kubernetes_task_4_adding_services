# ToDo Application Deployment Instructions

This guide provides instructions on how to interact with and test the ToDo application deployed in the Kubernetes cluster.

---
##  1. Apply all pods and services using next commands:
```bash
    kubectl apply -f ./src/busybox.yml
    kubectl apply -f ./src/todoapp-pod.yml
    kubectl apply -f ./src/clusterIP.yml
    kubectl apply -f ./src/nodeport.yml
```
## 2. Test of all pods and services work:

```bash
    kubectl get pods -n todoapp
    kubectl get svc -n todoapp
```

## 3. Change the default namespace:
To verify that the `ClusterIP` service is correctly balancing traffic between the two pods, use a temporary `busybox` container.
```bash
    kubectl config set-context --current --namespace=todoapp
```

## 4. Internal Testing via ClusterIP (DNS)
Go inside to busybox container to make curl response:
```bash
    kubectl exec -it busy -- sh
```
Make Http GET:
```bash
    curl http://todoapp-service.todoapp.svc.cluster.local
```
## 4. Internal Testing via ClusterIP (port-forward)
```bash
    kubectl port-forward service/todoapp-service 8081:80
```
Open your browser with localhost:8081 and you should see the app "ToDoList"

## 5. Accessing via NodePort Service
The `NodePort` service exposes the application on a specific port on every Node in the cluster.

**Look for the port in the `30000-32767` range for NodePort**
```bash
    kubectl get svc -n todoapp
```
    
**Get the Node IP address:**
```bash
    kubectl get nodes -o wide
```
It should be usually your localhost

**Access the application:**
Open your browser and navigate to: `http://localhost:30007`