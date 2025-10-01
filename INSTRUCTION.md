## Services adding 

## 1. ClusterIp service.

### 1.1 Namespace creating.
    kubectl apply -f .infrastructure/namespace.yml

### 1.2 Image build.
    docker build . -t timnord1993/todoapp:3.0.0
    
### 1.3. Applying clusterIp service.
    cubectl apply -f .infrastructure/services/clusterip.yml -n todoapp

### 1.4. Applying pods.
    cubectl apply -f .infrastructure/todoapp-pod.yml -n todoapp

### 1.5. Testing.
    kubectl port-forward service/clusterip 8081:80


## 2. NodePort service.

### 2.1 Applying nodePort service
    cubectl apply -f .infrastructure/services/nodePort.yml -n todoapp

### 2.2 Testing
    Open your browser and go to:
    localhost:30008