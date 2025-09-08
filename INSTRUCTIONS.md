Apply the manifests to create the namespace, pods, and services:

Create the namespace
```sh
kubectl apply -f namespace.yml
```
Create the pods and services
```ssh
kubectl apply -f todoapp-pod.yml
kubectl apply -f clusterIp.yml
kubectl apply -f nodePort.yml
```

You should see two pods in the `Running` state:
```ssh
NAME        READY   STATUS    RESTARTS   AGE
todoapp     1/1     Running   0          1m
todoapp-1   1/1     Running   0          1m
```
    

## Test the Application

### Method 1: Test with ClusterIP Service from a Busybox Container

The `ClusterIP` service is only accessible from within the cluster. We will deploy a `busybox` pod to test connectivity

1.  Deploy the `busybox` pod defined in the project:
    ```sh
    kubectl apply -f busybox.yml
    ```

2.  Wait for the pod to be running, then get a shell inside it:
    ```sh
    kubectl exec -it busybox -n todoapp -- /bin/sh
    ```

3.  From the `busybox` shell, send a request to the `ClusterIP` service using its service name. The service name is `todoapp-clusterp-ip-service` and it exposes port 80
    ```sh
    # Inside the busybox container
    curl http://todoapp-clusterp-ip-service:80
    ```
    You should receive the HTML content of the application's home page

4.  Exit the busybox container by typing `exit`

### Method 2: Test with `kubectl port-forward`

Forward a local port to one of the running pods to access the application from your machine

1.  Run the `port-forward` command. This will forward your local port `8080` to the `todoapp` pod's port `8080`
    ```sh
    kubectl port-forward pod/todoapp -n todoapp 8080:8080
    ```

2.  Open your web browser and navigate to `http://localhost:8080`. You should see the ToDo application

### Method 3: Test with NodePort Service

The `NodePort` service exposes the application on a static port (`30007`) on each cluster node
Open your web browser and navigate to `http://localhost:30007`