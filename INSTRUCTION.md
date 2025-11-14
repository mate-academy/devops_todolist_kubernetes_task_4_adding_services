## How to test an app by calling a ClusterIP service DNS from a busybox container
First of all run todoapp pods:

kubectl apply -f .infrastructure/todoapp-pod.yml

Than run service:

kubectl apply -f .infrastructure/clusterip.yml

Than run busybox:

kubectl apply -f .infrastructure/busybox.yml

After that connect to busybox container:

kubectl -n todoapp exec -it busybox -- sh

Inside container run command:

curl http://todoapp-service.todoapp.svc.cluster.local

You will recieve a start page html file

##  How to test ToDo application using the service port-forward command

Be sure that your pods and service ClusterIP still running:

kubectl apply -f .infrastructure/todoapp-pod.yml

kubectl apply -f .infrastructure/clusterip.yml

After that you can check work of ClusterIP service by using command:

kubectl port-forward -n todoapp service/todoapp-service 8081:80

## How to access an app using a NodePort Service

Be sure that your pods are running:

kubectl apply -f .infrastructure/todoapp-pod.yml

Than start the service

kubectl apply -f .infrastructure/nodeport.yml

Go to http://localhost:30007/ if web-site is working - everything good