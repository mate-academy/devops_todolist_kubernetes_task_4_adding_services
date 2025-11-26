# How to apply manifests and test the Todo app

## 1. Create namespace, pods and services

```bash
# from repository root
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-clusterip.yml
kubectl apply -f .infrastructure/todoapp-nodeport.yml

# check resources
kubectl get pods -n todoapp
kubectl get svc  -n todoapp

# Use the busybox pod to call the ClusterIP service.
# open a shell inside the busybox pod
kubectl exec -it busybox -n todoapp -- sh

# INSIDE the busybox container, test the ClusterIP service with curl
curl http://todoapp-clusterip:8080/api/health
curl http://todoapp-clusterip:8080/api/ready

# or using wget
wget -qO- http://todoapp-clusterip:8080/api/health
wget -qO- http://todoapp-clusterip:8080/api/ready

# exit from the container
exit

# Port-Forward Command (ClusterIP service)
# forward local port 8081 to the ClusterIP service port 8080 in the todoapp namespace
kubectl port-forward -n todoapp service/todoapp-clusterip 8081:8080

# NodePort Access
# get node external / public IP address
kubectl get nodes -o wide

# check NodePort service details
kubectl get svc todoapp-nodeport -n todoapp
kubectl describe svc todoapp-nodeport -n todoapp