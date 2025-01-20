# Instructions for Testing the ToDo Application

## Testing the Application Using ClusterIP Service
1. Deploy a busybox container:
   ```bash
   kubectl run busybox --image=busybox --restart=Never -- /bin/sh
2. Inside the busybox container, use wget or curl to test the application:
   ```bash
   wget -qO- http://todo-clusterip

## Testing the Application Using Port-Forward
1. Port-forward the ClusterIP service:
    ```bash
   kubectl port-forward service/todo-clusterip 8080:80
2. Access the application in your browser or use curl:
    ```bash
   curl http://localhost:8080

## Testing the Application Using NodePort Service
1. Find the Node IP address:
    ```bash
   kubectl get nodes -o wide
2. Use the Node IP and NodePort to access the application:
    ```bash
   curl http://<NodeIP>:30001
    Replace <NodeIP> with the IP address of any Kubernetes node.
   
