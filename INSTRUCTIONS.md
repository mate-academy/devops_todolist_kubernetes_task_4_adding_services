In order to set up the cluster services and pods:
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/nodePortService.yml
kubectl apply -f .infrastructure/clusterIpService.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml

Optionally set the current namespace to the namespace that was created:
kubectl config set-context --current --namespace todoapp

In order to access the todoapp server using the busybox container:
1. First get the name of the service:
kubectl get svc
2. Next sh into the busy box container and perform a request to the service:
kubectl exec -it busybox -- /bin/sh
curl -v http://cluster-ip-service.todoapp.cluster.local

In order to access the todoapp server from the browser:
kubectl port-forward service/cluster-ip-service 8080:80

In order to access the todoapp server using the node-port service:
1. Get the IP address of one of the nodes:
minikube ip
2. Make a request to the service from the browser by using the IP address of the node and the nodePort (30007)
