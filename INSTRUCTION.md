1. Test using ClusterIP service (inside the cluster)
------------------------------------------------------------

Step 1: Open a shell in the busybox pod
    kubectl exec -it -n todoapp busybox -- sh

Step 2: Test the ToDo application via ClusterIP service DNS
    curl http://todoapp-clusterip
    OR
    curl http://todoapp-clusterip:80
    OR
    curl http://todoapp-clusterip/api/ready

------------------------------------------------------------

2. Test using kubectl port-forward
------------------------------------------------------------

Step 1: Forward port 8080 on localhost to port 80 on the NodePort service
    kubectl port-forward -n todoapp service/todoapp-nodeport 8080:80

Step 2: Open browser to access the app:
    http://localhost:8080

------------------------------------------------------------

3. Test using NodePort (external access)
------------------------------------------------------------

Step 1: Get the internal IP of the node
    kubectl get nodes -o wide

Step 2: Get the NodePort of the service
    kubectl get svc -n todoapp todoapp-nodeport

Step 3: Combine the node IP and NodePort:
    http://<NODE-IP>:<NODE-PORT>

    Example:
    http://192.168.65.3:30080