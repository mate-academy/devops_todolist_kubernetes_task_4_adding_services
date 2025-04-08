## 0. Clean up everything (start fresh)

1. Delete the namespace and all related resources:
   ```bash
   kubectl delete ns todoapp
   ```

2. Start minikube with driver that run Virtual Machine (if exist remove old minikube)

   ```bash
   minikube delete
   ```

   ```bash
   minikube start --driver=docker
   ```

## 1. Deploy the app from scratch

1. Create namespace:
   ```bash
   kubectl apply -f .infrastructure/namespace.yml
   ```

2. Deploy pods:
    ```bash
    kubectl apply -f .infrastructure/todoapp-pod.yml
    kubectl apply -f .infrastructure/todoapp-pod-2.yml
    kubectl apply -f .infrastructure/busybox.yml
    ```

3. Deploy services:
    ```bash
    kubectl apply -f .infrastructure/clusterip-service.yml
    kubectl apply -f .infrastructure/nodeport-service.yml
    ```

4. Confirm everything is running:
    ```bash
    kubectl get pods -n todoapp
    kubectl get svc -n todoapp
    ```
   
## 2. Test the application using ClusterIP Service from a busybox pod

1. Connect to the busybox pod:
    ```bash
    kubectl exec -n todoapp -it busybox -- sh
    ```

2. Inside the busybox pod, run:
    ```bash
    curl http://todoapp-clusterip.todoapp.svc.cluster.local
    ```

## 3. Test the application using kubectl port-forward

1. Forward a local port to the ClusterIP service:
   ```bash
   kubectl port-forward -n todoapp svc/todoapp-clusterip 8080:80
   ```

2. In another terminal (or browser):
    ```bash
    curl http://localhost:8080
    ```

## 4. Access the app using NodePort service

1. Find the Node IP (if using Minikube):
   ```bash
   minikube ip
   ```

   Example:
   ```bash
   192.168.49.2
   ```

2. Try accessing the app:

    ```bash
    curl http://192.168.49.2:30036
    ```
   Or open in your browser:
    ```cpp
    http://192.168.49.2:30036
    ```

3. Troubleshooting: Docker driver limitations. Use minikube service to open a working proxy:

   ```bash
   minikube service todoapp-nodeport -n todoapp
   ```

|-----------|------------------|-------------|---------------------------|
| NAMESPACE |       NAME       | TARGET PORT |            URL            |
|-----------|------------------|-------------|---------------------------|
| todoapp   | todoapp-nodeport |          80 | http://192.168.49.2:30036 |
|-----------|------------------|-------------|---------------------------|
🏃  Starting tunnel for service todoapp-nodeport.
|-----------|------------------|-------------|------------------------|
| NAMESPACE |       NAME       | TARGET PORT |          URL           |
|-----------|------------------|-------------|------------------------|
| todoapp   | todoapp-nodeport |             | http://127.0.0.1:49982 |
|-----------|------------------|-------------|------------------------|
🎉  Opening service todoapp/todoapp-nodeport in default browser...
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.

   
  Example for open: http://127.0.0.1:49982

This approach is compatible with all environments and is the recommended way when using Minikube with Docker on macOS