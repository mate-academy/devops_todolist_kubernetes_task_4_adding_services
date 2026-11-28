1. Test the application using ClusterIP DNS from the BusyBox container:
kubectl exec -n todoapp busybox -- wget -qO- http://todoapp-clusterip.todoapp.svc.cluster.local

2. Test the application using port-forward:
kubectl port-forward svc/todoapp-clusterip 8080:80 -n todoapp
Open http://localhost:8080 in your browser.

3. Test the application using NodePort:
kubectl get nodes -o wide
Open http://NODE_IP:30080 in your browser.
4. 

If the node IP is not accessible from macOS, test NodePort from the BusyBox container:
kubectl exec -n todoapp busybox -- wget -qO- http://172.21.0.4:30080