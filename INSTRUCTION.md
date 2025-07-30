# to check the ClusterIp: 
# start it with
kubectl apply -f clusterIp.yml -n todoapp
# start the page http:localhost:8080 in the brawser and check the logs by:
kubectl logs todoapp-1
kubectl logs todoapp-2
# use command 
kubectl get svc 
# to check the running services
