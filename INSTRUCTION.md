# Deploy resources

kubectl apply -f pod-1.yaml
kubectl apply -f pod-2.yaml
kubectl apply -f clusterip-service.yaml
kubectl apply -f nodeport-service.yaml

# Verify

kubectl get pods
kubectl get svc

# Test ClusterIP DNS from BusyBox

kubectl run busybox --image=busybox --rm -it --restart=Never -- sh

wget -qO- http://todolist-clusterip

# Test with port-forward

kubectl port-forward service/todolist-clusterip 8080:80

Open:

http://localhost:8080

# Access through NodePort

kubectl get svc todolist-nodeport

Open:

http://<NODE_IP>:30080