Instructions on how to test an app by calling a ClusterIP service DNS from a busybox container:

kubectl apply -f ClusterIp.yml
use busybox
kubectl run -n todoapp busybox --image=busybox --restart=Never --rm -it --command -- sh
use command
wget -qO- http://todoapp-service.todoapp.svc.cluster.local


To test ToDo application using the service port-forward command

kubectl port-forward -n todoapp service/todoapp-service 8081:80

In browser:
http://localhost:8081/


How to access an app using a NodePort Service

kubectl apply -f NodePort.yml
kubectl get svc -n todoapp
In browser:
http://localhost:30007/