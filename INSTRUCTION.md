To prepare the tests apply todoapp-pod.yml and busybox.yml to run corresponding pods, after apply both clusterip-svc and nodeport-svc yml files to start services

To test an app by calling a ClusterIP service DNS from a busybox container run the busybox pod and connect to it with the command below:
kubectl -n todoapp exec -it busybox --sh
after run  curl http://nodeport-service.todoapp.svc.cluster.local to connect with nodeport service or
 curl http://clusterip-service.todoapp.svc.cluster.local to connect using clusterIP service

To test ToDo application using the service port-forward command run:
 kubectl port-forward service/clusterip-service 8080:80

To access the app using a NodePort Service open in a browser http://localhost:30001


