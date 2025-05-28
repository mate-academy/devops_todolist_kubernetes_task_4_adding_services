## How to test an app by calling a ClusterIP service DNS from a busybox container

1. kubectl apply -f .infrastructure/busybox.yml
2. kubectl exec -it busybox -n matepp -- sh
3. curl http://todolist-service.todoapp.svc.cluster.local/health/ready/




## How to test ToDo application using the service port-forward command

1. kubectl get pods -n matepp
2. kubectl port-forward svc/todolist-service 8080:80 -n matepp
3. http://localhost:8080



## How to access an app using a NodePort Service


1. kubectl apply -f .infrastructure/nodePort.yml
2. kubectl get nodes -o wide
3. http://<NODE_IP>:30007

