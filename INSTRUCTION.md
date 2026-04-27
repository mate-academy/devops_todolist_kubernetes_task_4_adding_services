## Creating all resources:
 From ./.infrastructure workdir run commands in this order:
 ```
 kubectl apply -f ./clusterIp.yml
 kubectl apply -f ./nodeport.yml
 kubectl apply -f ./todoapp-pod.yml
 kubectl apply -f ./busybox.yml
 ```
## Testing services:
 Test ClusterIP from busybox:
 
1. Run shell inside busybox pod:
    ```
   kubectl -n todoapp exec busybox -it -- /bin/sh
   ```
2. In shell run curl:
    ```
   curl http://todoapp-service.todoapp.svc.cluster.local
   ```

Test ClusterIP using port-forward:

    kubectl port-forward -n todoapp service/todoapp-service 8081:80

and try in browser: http://localhost:8081


Testing NodePort service:

Get ip from one of your node and use it in browser:

http://ip-node:30123

