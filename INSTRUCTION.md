1. How to test an app by calling a ClusterIP service DNS from a busybox container.
Use command:
kubectl exec -it busybox -n todoapp -- sh
Then use command: curl http://todoapp-service.todoapp.svc.cluster.local

2. How to access an app using a NodePort Service.
We should start our service with port wich we use in nodeport.yml manifest. In our option it's 30008.

3. How to test ToDo application using the service port-forward command.
Use command:
kubectl port-forward service/todoapp-service 8081:80
Then go to browser and input http://localhost:8081