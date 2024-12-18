# Instructions for ToDo Application Deployment

## 1. Deploying Pods and Services

### a. **Create Pods:**
To deploy the application, two pods named `todoapp` and `todoapp-clone` are defined in the `todoapp-pod.yml` file. Each pod contains a container running the ToDo app. These pods are labeled with `app: todoapp`.

To apply the pod manifest:
```bash
kubectl apply -f todoapp-pod.yml
```

### b. **ClusterIP Service:**
The `clusterIP.yml` file defines a ClusterIP service that balances traffic between the two pods.

To apply the ClusterIP service manifest:
```bash
kubectl apply -f clusterIP.yml
```

### c. **NodePort Service:**
The `nodeport.yml` file defines a NodePort service that exposes the ToDo app on port `30303`.

To apply the NodePort service manifest:
```bash
kubectl apply -f nodeport.yml
```

## 2. Testing the Application

### a. **Test ClusterIP Service via BusyBox:**
To test the ClusterIP service, use the `busybox` pod. The pod will attempt to connect to the ToDo app via the ClusterIP DNS.

- Apply the `busybox.yml` manifest:
  ```bash
  kubectl apply -f busybox.yml
  ```

- Enter the BusyBox pod:
  ```bash
  kubectl exec -it busybox -- sh
  ```

- Test the connection to the ClusterIP service:
  ```bash
  curl todoapp-clusterip.todoapp.svc.cluster.local
  ```

This command should return the ToDo application’s landing page content.

### b. **Test ToDo Application with Port Forwarding:**
To test the ToDo application via port-forwarding:

1. Find the pod name:
   ```bash
   kubectl get pods -n todoapp
   ```

2. Forward the port:
   ```bash
   kubectl port-forward pod/todoapp 8080:8080 -n todoapp
   ```

3. Access the ToDo app at `http://localhost:8080`.

### c. **Access the App via NodePort:**
To access the ToDo application through the NodePort service:

1. Find the external IP of your node (use `kubectl get nodes -o wide` to check).
2. Access the app via:
   ```bash
   http://<node-ip>:30303
   ```
This will expose the ToDo app externally via the NodePort.
