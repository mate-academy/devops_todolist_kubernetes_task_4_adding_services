## to apply manifests execute commands:
1. namespace: `kubectl apply -f .\.infrastructure\namespace.yml`
2. busybox: `kubectl apply -f .\.infrastructure\busybox.yml`
3. todoapp: `kubectl apply -f .\.infrastructure\todoapp-pod.yml`
4. clusterip: `kubectl apply -f .\.infrastructure\clusterip.yml`
5. nodeport: `kubectl apply -f .\.infrastructure\nodeport.yml`



## to configure port forwarding execute `kubectl port-forward service/todoapp-service -n todoapp 8081:80`
To get to API open `http://127.0.0.1:8081` in browser. 

## to curl api from CLI:
1. execute command `kubectl exec -n todoapp -it busyboxplus -- sh`
2. execute command `curl http://todoapp-service.todoapp.svc.cluster.local`