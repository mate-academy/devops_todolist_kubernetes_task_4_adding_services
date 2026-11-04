# instructions on how to apply all manifests
cd /path/to/project/.infrastructure
kubectl apply -f namespace.yml
kubectl apply -f busybox.yml
kubectl apply -f todoapp-pod.yml
kubectl apply -f clusterip.yml
kubectl apply -f nodeport.yml
kubectl config set-context --current --namespace=todoapp

# P.S. if busybox.yml doesn't work use
kubectl apply -f alpine.yml

# instructions on how to test an app by calling a ClusterIP service DNS from a busybox container
kubectl exec -it -n todoapp busybox -- sh
# if busybox doesn't work
kubectl exec -it -n todoapp alpine -- sh
# run command 
curl http://todoapp-service.todoapp.svc.cluster.local
# you'll see html code 

# instructions on how to test ToDo application using the service port-forward command
kubectl port-forward service/todoapp-service 8000:80
# url for local browser: http://127.0.0.1:8000/

# instructions on how to access an app using a NodePort Service
url for local browser: http://127.0.0.1:30007/