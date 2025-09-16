# Testing ToDo App in Kubernetes

## 1. Create Namespace
```
kubectl create namespace todoapp
```
## 2. Deploy BusyBox for Testing
```
kubectl run busybox \
  --image=ikulyk404/busyboxplus:curl \
  --namespace=todoapp \
  --restart=Never \
  -- sleep 1000

```
```
kubectl get pods -n todoapp
```
```
kubectl get endpoints -n todoapp todoapp-clusterip
```
## 1. Test using ClusterIP Service from BusyBox
First, make sure busybox pod is running in namespace `todoapp`:
```
kubectl get pods -n todoapp
```
Exec into busybox pod:
```
kubectl exec -n todoapp -it busybox -- sh
wget -qO- http://todoapp-clusterip:80
```
## 2. Test using Port Forward
```
kubectl port-forward -n todoapp svc/todoapp-clusterip 8080:80
```
Open http://localhost:8080

## 3. Test using NodePort Service
Find your Node IP:
```
kubectl get nodes -o wide
```
Check your Node IP (e.g., minikube ip) and open:

```
http://<NodeIP>:30080
```