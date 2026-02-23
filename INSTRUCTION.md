# INSTRUCTION

## Apply manifests

`namespace`:
```bash
kubectl apply -f .infrastructure/namespace.yml
```

`if namespace manifest is not available, create namespace manually`:
```bash
kubectl create namespace todoapp
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

`busybox` (for service DNS test; this manifest uses image with curl):
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

`if test pod is in another namespace, use full service DNS (FQDN)`:
```bash
kubectl exec -it -n <TEST_NAMESPACE> <POD_NAME> -- curl http://todoapp-clusterip.todoapp.svc.cluster.local/
kubectl exec -it -n <TEST_NAMESPACE> <POD_NAME> -- curl http://todoapp-clusterip.todoapp.svc.cluster.local/api/
```

## Test app with port-forward

`forward local port 8080 to ClusterIP service port 80`:
```bash
kubectl port-forward -n todoapp svc/todoapp-clusterip 8080:80
```

`port-forward runs locally and stops after Ctrl+C`:
```text
Use free local port 8080 before running the command.
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
