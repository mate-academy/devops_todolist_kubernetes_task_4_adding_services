# Django ToDo list

1. Create all pods and services

```
bash

kubectl apply -f ./.infrastructure
```

2. Test app by ClusterIP and busybox

```
bash

kubectl -n todoapp exec -it busybox -- sh
```

and run

```
bash

curl http://todoapp-cluster-ip-service.todoapp.svc.cluster.local
```

3. Test app by ClusterIP and port-forward

```
bash

kubectl port-forward svc/todoapp-cluster-ip-service 8080:80 -n todoapp
```

Open http://localhost:8080

4. Test app by NodePort

Open http://localhost:30007
