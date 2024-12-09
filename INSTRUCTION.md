## How to apply all manifests

To apply manifests, open project folder in terminal and execute following commands:

```
cd .\infrastructure\
kubectl apply -f namespace.yml  
kubectl apply -f todoapp-pod.yml  
kubectl apply -f busybox.yml  
kubectl apply -f clusterIp.yml
kubectl apply -f nodeport.yml
cd ..
```

## How to test by calling a ClusterIP service 
``
kubectl -n todoapp exec -it busybox -- sh
``

After succesfuly exec into the busybox pod, execute next command in shell

``
curl http://todoapp-service:80
``

## How to test ToDo application using the service port-forward command

``
kubectl -n todoapp port-forward service/todoapp-service 8080:80
``
Once the port forward is set up, you can access the service running on the pod visiting `http://localhost:8080`

## How to access an app using a NodePort Service

``
http://localhost:30007
``
