# ToDo App Kubernetes Services

## 1. Apply Manifests
Deploy the pods and services to the Kubernetes cluster:
```bash
kubectl apply -f .infrastructure/
```

## 2. Test ClusterIP via busybox container
This tests internal DNS resolution. The ClusterIP service balances traffic between the two pods.
```bash
kubectl exec -it busybox -- sh
curl http://todoapp-clusterip:80/
exit
```

## 3. Test using Service port-forward
This forwards local traffic to the ClusterIP service, testing the application locally.
```bash
kubectl port-forward svc/todoapp-clusterip 8080:80
```
*(Press Ctrl+C in the terminal to stop port-forwarding before proceeding to the next step).*

## 4. Test using NodePort Service
NodePort exposes the application on a specific port across all cluster nodes. No port-forwarding command is needed.
```bash
# Open your web browser and navigate directly to:
# http://localhost:30080
```

