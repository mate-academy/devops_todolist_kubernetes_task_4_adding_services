# Django ToDo list Testing Instructions
## Applying all the manifest 
To start testing the application, you should run such a commands
1. `cd .\.infrastructure\`
2. `kubectl apply -f namespace.yml`
3. `kubectl apply -f busybox.yml`
4. `kubectl apply -f todoapp-pod.yml`
5. `kubectl apply -f clusterip.yml`
6. `kubectl apply -f nodeport.yml`
## Testing the application
### Testing the application by calling a ClusterIP service DNS from a busybox container
To get started, you need to connect to the pod using the command
1. `kubectl exec -it -n todoapp busybox -- sh`

Inside the Busybox shell, test the connectivity using `curl`
2. `curl http://todoapp-service.todoapp.svc.cluster.local`
### Testing ToDo Application Using Port-Forward
To test te application run:
1. `kubectl port-forward svc/todoapp-service 8081:80`
Open the browser add write:
2. `http://localhost:8081`
### Testing the Application using a NodePort service
Open the browser add write:
1. `http://localhost:30007`