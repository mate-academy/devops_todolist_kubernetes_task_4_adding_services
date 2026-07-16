# Instructions

## Apply all manifests
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterIP.yml
kubectl apply -f .infrastructure/nodePort.yml
kubectl apply -f .infrastructure/busybox.yml

## Test via ClusterIP DNS from busybox
kubectl exec -it busybox -n todoapp -- curl http://todoapp-service.todoapp.svc.cluster.local/api/health/

## Test via port-forward
kubectl port-forward service/todoapp-service 8080:80 -n todoapp
# Открой в браузере: http://localhost:8080

## Test via NodePort
# Lookup IP Node:
kubectl get nodes -o wide
# Open in your browser: http://<NODE_IP>:30007