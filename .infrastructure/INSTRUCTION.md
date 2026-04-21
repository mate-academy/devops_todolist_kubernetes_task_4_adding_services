# INSTRUCTION.md

## 1. Test the application via ClusterIP Service DNS from a BusyBox container
* Apply manifest:
    ```bash
    kubectl apply -f busybox.yaml -n todoapp
    ````
* Connect to busybox:
   ```bash
   kubectl -n todoapp exec -it busybox -- sh
   ```
* Execute curl-command:
    ```bash
   curl http://clusterip-service.todoapp.svc.cluster.local:90
    ```
   
## 2. Test the application via ClusterIP Service on your local machine:
* Execute port-forward command:
    ```bash
   kubectl port-forward service/clusterip-service <your_port>:90 -n todoapp &
    ```
* Go to http://127.0.0.1:`<your_port>` via your browser.
    
## 3. Test the application via NodePort Service on your local machine:
* Apply manifest:
    ```bash
    kubectl apply -f nodeport-service.yaml -n todoapp
    ````
* Go to http://127.0.0.1:30777 via your browser.
