To test app you need to:
1. Go to busybox container using command: kubectl exec -it busybox -- sh
2. Send request to the clusterIP service using curl:
curl http://todolist-app-service.todoapp.svc.cluster.local
3. To be able to reach app locally you need to run this command: kubectl port-forward pod/todoapp-2 8000:8080
4. Open browser and go to localhost:8000/
5. To be able to reach app locally via nodePort you need to open browser and go to http://<nodeIP>:30007