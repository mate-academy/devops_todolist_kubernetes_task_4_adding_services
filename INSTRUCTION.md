kubectl apply -f .infrastructure/namespace.yml

kubectl apply -f .infrastructure/todoapp-pod.yml

kubectl config set-context --current --namespace=todoapp

kubectl apply -f .infrastructure/clusterIp.yml

kubectl get svc   

kubectl -n todoapp exec -it busybox -- sh
curl http://todoapp-service.todoapp.svc.cluster.local

kubectl port-forward service/todoapp-service 8081:80
localhost:8081

kubectl apply -f .infrastructure/nodeport.yml

kubectl get svc

http://localhost:30007