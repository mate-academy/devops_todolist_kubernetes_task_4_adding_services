
## Testing the Application via ClusterIP
1. Create a BusyBox container and make a request to the service:
   ```sh
   kubectl run busybox --rm -it --image=busybox --restart=Never -- wget -qO- http://todolist-clusterip
   ```

## Testing the ToDo Application Using Port-Forward
1. Run the command to forward the port:
   ```sh
   kubectl port-forward svc/todolist-clusterip 8080:80
   ```
2. In a new terminal, make a request to the local port:
   ```sh
   curl http://localhost:8080
   ```

## Accessing the Application via NodePort
1. Get the node's IP address (NODE_IP):
   ```sh
   kubectl get nodes -o wide
   ```
2. Make a request to the NodePort service:
   ```sh
   curl http://<NODE_IP>:30080
   ```

