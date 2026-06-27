1. To test our ToDo application and ClusterIP service with busybox, we will first need to create a busybox pod. You can do this by running the following command:

```bash
kubectl apply -f .infrastructure/busybox.yml
```

Then connect to the busybox pod using the following command:

```bash
kubectl -n todoapp exec -it busybox -- sh
```

And run curl command to test the ClusterIP service:

```bash
curl http://todoapp-clusterip-service:8080/
```

2. To test the ToDo application using the service `port-forward` command, you can run the following command:

```bash
kubectl port-forward -n todoapp service/todoapp-clusterip-service 8081:8080
```

Then open your web browser and navigate to `http://localhost:8081/` to access the ToDo application.

3. To access the ToDo application using a NodePort service, you can open browser with the following URL:

```bash
http://localhost:30080/
```
