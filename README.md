# Django ToDo list

### Testing the application from a busybox container
1. Use `kubectl -n mateapp exec -it busybox -- sh`
2. Write the following command in the window that opens `curl http://todoapp.mateapp.svc.cluster.local`

### Testing the application using `port-forward` command
1. Use `kubectl port-forward service/todoapp 8081:80 -n mateapp`

### Access the app using a NodePort Service
1. Open your browser and past: `localhost:30007`