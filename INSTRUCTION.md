
## Connect to busybox

# Run a temporary BusyBox pod
```
kubectl run busybox --image=busybox:latest --restart=Never --rm -it -- /bin/sh
```

# Inside the BusyBox shell, use curl

```
curl -qO- http://todolist-clusterip:80
```

## Test ToDo App using port forward

# forward
```
kubectl port-forward service/todoapp-service 8081:80
```

# check
```
curl http://localhost:8081
```

##  Using a NodePort Service

# Get cluster node’s IP

```
kubectl get nodes -o wide
```

# Use the node's external IP
```
curl http://<NODE_IP>:30007
```

# Open browser
```
http://<NODE_IP>:30007
```