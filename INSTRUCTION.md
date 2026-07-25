# ToDo Application Instructions


## Instructions for testing the application by accessing the DNS name of the ClusterIP service from the BusyBox container:

1. Go inside the busybox container

```bash
kubectl exec -it -n mateapp busybox -- sh
```

2. Execute a curl request with the DNS name of the service

```bash
curl http://todoapp-clusterip.todoapp.svc.cluster.local
```

3. To exit the container, run the `exit` command.


## Instructions for testing the ToDo application using the service port forwarding command — port-forward

Open http://localhost:8000 in the browser
```bash
kubectl port-forward service/todoapp-clusterip 8000:80 -n todoapp
```


## Instructions for accessing the application through a NodePort type service

Execute http://localhost:30007 in the browser