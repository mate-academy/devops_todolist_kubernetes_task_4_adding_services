# Django ToDo list

# Before testing you need to apply all manifests
1. Get into the directory `.infrastructure` using `cd` command
2. Use `kubectl apply -f namespace.yml`
3. Use `kubectl apply -f busybox.yml`
4. Use `kubectl apply -f todoapp-pod.yml`
5. Use `kubectl apply -f cluster-ip.yml`
6. Use `kubectl apply -f nodeport.yml`


### Testing the application from a busybox container
1. Use `kubectl -n mateapp exec -it busybox -- sh`
2. Write the following command in the window that opens `curl http://todoapp.mateapp.svc.cluster.local`

### Testing the application using `port-forward` command
1. Use `kubectl port-forward service/todoapp 8081:80 -n mateapp`

### Access the app using a NodePort Service
1. Open your browser and past: `localhost:30007`