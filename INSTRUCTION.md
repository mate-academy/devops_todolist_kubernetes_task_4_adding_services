# Instructions for deploying and testing ToDo application

# How to apply the pods manifests:

kubectl apply -f todoapp-pod.yml -n todoapp
kubectl apply -f busybox.yml -n todoapp

# How to apply the ClusterIP manifest:
kubectl apply -f clusterIP.yml

# How to apply the NodePort manifest:
kubectl apply -f nodeport.yml

# Testing the ToDo application:
kubectl port-forward pod/todoapp 8081:8080 -n todoapp

# After this, open your browser and go to:
# http://localhost:8081

# How to test an app by calling a ClusterIP service DNS from a busybox container:

kubectl -n todoapp  exec -it busybox -- sh
curl http://todoapp-service.todoapp.svc.cluster.local:80 or wget http://todoapp-service.todoapp.svc.cluster.local:80

# How to access an app using a NodePort service:
kubectl get nodes -n todoapp -o wide
# find the INTERNAL-IP after this, open your browser and go to:
http://<INTERNAL-IP>:30009


