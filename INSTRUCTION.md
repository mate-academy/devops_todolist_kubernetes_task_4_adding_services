# How to deploy and test the ToDo app: 

## 1. Create the namespace and pods:
```
kubectl apply -f .infrastructure/namespace.yml 
```
    and check does it exist:
```
kubectl get namespace
```

## 2. Create pods
```
kubectl apply -f  .infrastructure/todoapp-pod.yml
```
    and check does it created:
```
kubectl get pods -n todoapp
```

## 3. Create cluster IPservice from manifest:
```
kubectl apply -f .infrastructure/clusterip.yml
```
    and check it too:
```
kubectl  get svc -n todoapp
```

## 4. Create busybox for testing from manifest:
```
kubectl apply -f .infrastructure/busybox.yml 
```

    and check does it created: 
```
kubectl get pods -n todoapp
```

## 5. Create port forwarding for test inside the browser:
```
kubectl port-forward service/todoapp-service 8081:80 -n todoapp
```

## 6. Checking curl connection inside the container
    inserting into container: 
```
kubectl -n todoapp exec -it busybox -- sh
```
    and run curl's page inside the container: 
```
curl http://localhost:8081
```

## 7. Create the NodePort service and acces the app:
```
kubectl apply -f .infrastructure/nodeport.yml 
```
    check does it created:
```
kubectl get nodes -o wide 
```
    and get the ip from the service, run after:
  ```
curl http://<nodeIP>:<NodePort>

  ```


