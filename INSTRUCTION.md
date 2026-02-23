# INSTRUCTION

## Apply manifests

`namespace`:
```bash
kubectl apply -f .infrastructure/namespace.yml
```

`pods`:
```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

`services`:
```bash
kubectl apply -f .infrastructure/todoapp-clusterip-service.yml
kubectl apply -f .infrastructure/todoapp-nodeport-service.yml
```

`busybox` (for service DNS test):
```bash
kubectl apply -f .infrastructure/busybox.yml
```

## Check resources

`pods in namespace todoapp`:
```bash
kubectl get pods -n todoapp
```

`services in namespace todoapp`:
```bash
kubectl get svc -n todoapp
```

## Test ClusterIP service DNS from BusyBox

`open main page through service DNS`:
```bash
kubectl exec -it -n todoapp busybox -- curl http://todoapp-clusterip/
```

`open API through service DNS`:
```bash
kubectl exec -it -n todoapp busybox -- curl http://todoapp-clusterip/api/
```

## Test app with port-forward

`forward local port 8080 to ClusterIP service port 80`:
```bash
kubectl port-forward -n todoapp svc/todoapp-clusterip 8080:80
```

`open in browser`:
```text
http://localhost:8080/
```

## Access app using NodePort service

`get node IP`:
```bash
kubectl get nodes -o wide
```

`open app in browser`:
```text
http://<NODE_IP>:30080/
```

`minikube alternative (prints URL)`:
```bash
minikube service todoapp-nodeport -n todoapp --url
```
