# Instructions for Kubernetes Manifests

## Applying All Manifests

   kubectl apply -f .infrastructure/namespace.yml
   kubectl apply -f .infrastructure/busybox.yml
   kubectl apply -f .infrastructure/clusterIP.yml
   kubectl apply -f .infrastructure/nodeport.yml
   kubectl apply -f .infrastructure/todoapp-pod.yml

## Testing ToDo Application Using Port-Forward

   kubectl port-forward service/todoapp-service 8080:8080 -n todoapp
   Open your browser and navigate to: http://localhost:8080

## Testing the Application Using BusyBox Container

   kubectl exec -it busybox -n todoapp -- curl -X GET http://todoapp-service


## Testing ToDo Application via NodePort

    Open your browser and navigate to: http://localhost:30007