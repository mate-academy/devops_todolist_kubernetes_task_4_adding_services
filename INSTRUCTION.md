## How to test an app by calling a ClusterIP service DNS from a busybox container
1. kubectl apply -f .infrastructure/busybox.yml
2. kubectl exec -it busybox -n todoapp -- sh
3. curl http://todoapp-internal.todoapp.svc.cluster.local/api/ready

## How to test ToDo application using the service port-forward command
1. kubectl get pods -n todoapp
2. kubectl port-forward svc/todoapp-internal 8080:80 -n todoapp
3. http://localhost:8080

## How to access an app using a NodePort Service
1. kubectl apply -f .infrastructure/nodeport-service.yaml
2. kubectl get nodes -o wide
3. http://<NODE_IP>:30005
