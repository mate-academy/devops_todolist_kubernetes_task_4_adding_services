kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml -n todoapp
kubectl apply -f .infrastructure/busybox.yml -n todoapp
kubectl apply -f .infrastructure/clusterIp-service.yml -n todoapp
kubectl apply -f .infrastructure/nodeport-service.yml -n todoapp

TEST CLUSTERIP_SERVICE:
kubectl exec -it busybox -n todoapp -- sh
in busybox: curl http://todoapp-clusterip.todoapp.svc.cluster.local

TEST PORT-FORWARD:
kubectl port-forward pod/todoapp -n todoapp 8080:8080
go to browser: localhost:8080

TEST NODEPORT:
go to browser: localhost:30007