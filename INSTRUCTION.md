Use the following links below:

1. Link to the Docker Hub repository with an app image:
    https://hub.docker.com/repository/docker/nosenkoalena501/todoapp/general

2. All instructions:
    - how to test an app by calling a ClusterIP service DNS from a busybox container:
        kubectl apply -f .infrastructure/todoapp-pod.yml
        kubectl apply -f .infrastructure/clusterIp.yml
        kubectl apply -f .infrastructure/busybox.yml
        kubectl exec -n todoapp -it busybox -- sh
        curl http://todoapp-service.todoapp.svc.cluster.local

    - how to test ToDo application using the service port-forward command:
        kubectl port-forward service/todoapp-service 8081:80 -n todoapp
        Make sure http://localhost:8081/ URL is available and working.

    -  how to access an app using a NodePort Service:
        kubectl apply -f .infrastructure/nodePort.yml
        Make sure http://localhost:30009/ URL is available and working.