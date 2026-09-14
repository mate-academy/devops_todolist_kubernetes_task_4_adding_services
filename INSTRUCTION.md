1. Run ToDo application pod from todoapp-pod.yml manifest.

    - go to .infrastructure directory:

        cd .\.infrastructure\
        
    - run the application:

        kubectl apply -f todoapp-pod.yml


2. How to test an app by calling a ClusterIP service DNS from a busybox container.

    - Run ClusterIP service from clusterIp.yml manifest:

        kubectl apply -f clusterIp.yml
    
    - Run Busybox application pod from busybox.yml manifest:

        kubectl apply -f busybox.yml

    - Connect to the busybox app terminal:

        kubectl -n todoapp exec -it busybox -- sh
    
    - Calling a ClusterIP service DNS from a busybox container using http get request command.

        curl http://todoapp-service.todoapp.svc.cluster.local

    
3. How to test ToDo application using the service `port-forward` command.

    - Run port-forward command:

        kubectl port-forward service/todoapp-service 8081:80 -n todoapp
    
    - Go to http://localhost:8081/ in your browser.


4. How to access an app using a NodePort Service.

    - Run NodePort Service from nodeport.yml manifest:

            kubectl apply -f nodeport.yml
    
    - Go to http://localhost:30008/ in your browser. Port number set in nodeport.yml