# Todoapp

## Setup

1. Clone the repository.
2. Ensure you have Docker, Kubernetes installed and running.

## Applying Kubernetes Manifests
1. Before applying your Kubernetes manifests, you need to create a secret for your Django SECRET_KEY. Use the following command, replacing your-secret-key-here with your actual    
    Django secret key:
   ```bash
   kubectl create secret generic todoapp-django-secret --from-literal=SECRET_KEY='your-secret-key-here' -n todoapp
    ```
2. Make sure you have kubectl configured to your Kubernetes cluster.
3. Apply the Kubernetes manifests by running the following command in the terminal:
    ```bash
    kubectl apply -f .infrastructure/
    ```

## Testing

### Using Port Forwarding
1. Check your services
   ```bash
    kubectl get svc -n todoapp
   ```
2. Forward the port from the todoapp pod to your local machine:
    ```bash
    kubectl port-forward service/{service_name} 8081:80 -n todoapp
    ```
3. Open your browser and visit ```http://localhost:8081``` to access the Todoapp.

### Test ClusterIP using Busybox Container

1. Access the shell of the busybox pod:
    ```bash
    kubectl -n todoapp exec -it busybox -- sh
    ```
2. Test the connection to the ToDo app:
    ```bash
   curl http://{service_name}.todoapp.svc.cluster.local
    ```

### Test app with NodePort

1. Use the following command to find out the NodePort assigned to your service:
    ```bash
    kubectl get svc -n todoapp
    ```
2. Note the NodePort next to your service. For testing, use the address format http://{NodeIP}:{NodePort}
3. Open a browser and navigate to the NodePort URL, for example:
    ```bash
    http://localhost:30007
    ```
