## Test

# 1. Connect to the busybox:

```sh
  kubectl -n todoapp exec -it busybox -- sh
```

# 2. Run curl:

```sh
  curl http://todoapp-clusterip.todoapp.svc.cluster.local
```

# 3. Check using port-forwarding:

```sh
 kubectl port-forward service/todoapp-clusterip 8081:80 -n todoapp
```

# 4. Check app using NodePort, following the link below:

```sh
  http://localhost:30007
```