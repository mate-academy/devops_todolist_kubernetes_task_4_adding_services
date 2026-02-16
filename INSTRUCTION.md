```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl apply -f .infrastructure/busybox.yml
```

Wait until pods are ready: `kubectl get pods -n todoapp`

## 1. Test the app by calling the ClusterIP service DNS from a busybox container
```bash
kubectl -n todoapp exec -it busybox -- sh
curl http://todoapp-clusterip/api/health
exit
```
The service listens on port **80** (mapped to pod port 8080). A successful response from `/api/health` indicates the app is reachable via ClusterIP DNS.

## 2. Test the ToDo application using the service `port-forward` command
```bash
kubectl port-forward service/todoapp-clusterip 8081:80 
```
Then open in your browser:
http://localhost:8081

## 3. Access the app using the NodePort service

Replace `\<NODE_IP\>` with the actual node IP from `kubectl get nodes -o wide`.

http://\<NODE_IP\>:30007/

```bash
# NodePort may be available at localhost:30007
open http://localhost:30007/
```

