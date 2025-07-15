For testing ClusterIP service with curl:
1. Get in busybox container with kubectl -n todoapp exec -it busybox --sh
2. Inside shell run command - curl http://todoapp-service.todoapp.svc.
3. Done, you get a main page of application

For testing ClusterIP service with port-forward:
1. Run command - kubectl port-forward service/todoapp-service 8081:80
2. Open browser and go to http://localhost:8081

For testing NodePort service:
1. Open browser and go to http://localhost:{your_nodeport_targetPort}