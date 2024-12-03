## How to test by calling a ClusterIP service 
``
kubectl -n todoapp exec -it busybox -- sh
``

## How to test ToDo application using the service port-forward command

``
kubectl port-forward service/todoapp-service 8081:80
``

## How to access an app using a NodePort Service

``
http://localhost:30007
``
