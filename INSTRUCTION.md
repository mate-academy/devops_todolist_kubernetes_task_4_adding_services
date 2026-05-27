# Instruction for launching, scaling and network testing ToDo application
## 1. Deploying Kubernetes Pods and Services
Run the following commands from the project root directory to create the namespace of names, lauching Pods and setting up networking Services:
```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterip-svc.yml
kubectl apply -f .infrastructure/nodeport-svc.yml 
```
## 2. Testing ClusterIP via DNS (busybox)
```bash
# Run shell inside busybox pod
kubectl exec -it busybox -n todoapp -- sh
# Next inside busybox:
nslookup clusterip-service
wget -qO- http://clusterip-service:80
3. Testing via port-forward
kubectl port-forward svc/clusterip-service 8080:80 -n todoapp
# Open in browser: http://localhost:8080
4. Accessing via NodePort
# Get node IP
kubectl get nodes -o wide
# Access via: <node-ip>:32000
