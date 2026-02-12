To create todoapp namespace use command:
    kubectl apply -f .infrastructure/namespace.yml

To create todoapp pod use command:
    kubectl apply -f .infrastructure/todoapp-pod.yml

To create busybox pod use command:
    kubectl apply -f .infrastructure/busybox.yml

To apply clusterIP service use command:
    kubectl apply -f .infrastructure/clusterIP.yaml

To apply NodePort service use command:
    kubectl apply -f .infrastructure/nodePort.yaml

To test an app by calling a ClusterIP service DNS from a busybox container use commands:
    To go into busybox pod use command:
        kubectl exec -it busybox -n todoapp -- sh
    To test app use command:
        curl http://clusterip-service.todoapp.svc.cluster.local

To test ToDo application using the service port-forward command:
    exit busybox pod and use command:
        kubectl port-forward service/clusterip-service -n todoapp 8080:80

To access an app using a NodePort Service:
    Use command:
        curl http://localhost:30010
    Or go to http://localhost:30010 on your webbrowser
