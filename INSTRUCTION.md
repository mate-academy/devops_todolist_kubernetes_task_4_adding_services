 kubectl exec -it -n todoapp busybox -- sh
 curl http://todoapp-servise.todoapp.svc.cluster.local

 kubectl port-forward service/todoapp-service 8081:80 -n todoapp
 http://localhost:8081/

 http://localhost:30008/