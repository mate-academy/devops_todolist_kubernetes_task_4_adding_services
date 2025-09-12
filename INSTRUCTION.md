## First of all, we need to apply all namespaces, pods and services. For this, execute next commands:
```shell
    kubectl apply -f namespace.yml
    kubectl apply -f .infrastructure/busybox.yml -n todoapp
    kubectl apply -f .infrastructure/todoapp-pod.yml -n todoapp
    kubectl apply -f .infrastructure/cluster-ip.yml -n todoapp
    kubectl apply -f .infrastructure/nodeport.yml -n todoapp
```

## To test app from a busybox, connect to busybox pod with command:
```shell
    kubectl -n todoapp exec -it busybox -- sh
```

### And execute the curl command:
```shell
    curl http://todoapp-service.todoapp.svc.cluster.local
```

## To test app with port forward, run the port forward command:
```shell
    kubectl port-forward service/todoapp-service 8080:80 -n todoapp
```

### And open [localhost at port 8080](http://localhost:8080)

## To test app with nodeport service just open [localhost at port 30007](http://localhost:30007)
