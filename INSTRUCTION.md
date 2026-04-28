# INSTRUCTION

## 1. Apply all manifests

Run commands from the project root:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/ClusterIP.yml
kubectl apply -f .infrastructure/NodePort.yml
```

Check that pods and services are running:

```bash
kubectl get pods -n todoapp
kubectl get svc -n todoapp
```

## 2. Test app via ClusterIP service DNS from busybox

Use busybox pod to call service DNS inside the cluster:

```bash
kubectl exec -n todoapp busybox -- curl -s http://todoapp-service/
```

Optional: call API endpoint directly:

```bash
kubectl exec -n todoapp busybox -- curl -s http://todoapp-service/api/
```

## 3. Test app via service port-forward

Forward local port to the ClusterIP service:

```bash
kubectl port-forward -n todoapp service/todoapp-service 8080:80
```

In another terminal:

```bash
curl -s http://127.0.0.1:8080/
curl -s http://127.0.0.1:8080/api/
```

## 4. Access app via NodePort service

Get your node IP and open the app through NodePort:

```bash
kubectl get nodes -o wide
kubectl get svc -n todoapp todoapp-service-nodeport
```

Use this URL in browser or curl:

```bash
http://<NODE_IP>:30080/
```

Example with curl:

```bash
curl -s http://<NODE_IP>:30080/
curl -s http://<NODE_IP>:30080/api/
```
