test an app by calling a ClusterIP service DNS from a busybox container
kubectl -n todoapp exec -it busybox -- sh
curl http://todoapp-clusterip.todoapp.svc.cluster.local
test ToDo application using the service port-forward command
kubectl port-forward service/todoapp-clusterip 8081:80
how to access an app using a NodePort Service
open a web browser and navigate to http://localhost:30011