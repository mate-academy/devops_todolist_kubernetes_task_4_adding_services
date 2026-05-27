# 1) How to test an app by calling a ClusterIP:

    kubectl apply -f clusterIP.yaml

And now you can test your app with two ways, the first one:

    kubectl port-forward service/todolist-service <your-port>:80

Second way:

    kubectl -n todoapp exec -it busybox -- sh
    curl http://todolist-service.todoapp.svc.cluster.local

# 2) How to access an app using a NodePort

    kubectl apply -f nodeport.yaml

In your browser:
    
    http://localhost:30007


    
    