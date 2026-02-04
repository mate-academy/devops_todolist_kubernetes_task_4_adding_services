# ToDo App Kubernetes Services & Pods

## 1. Deploy Resources
Apply all manifests to setup the namespace, pods, and services:

```bash
# Apply Namespace (if not exists)
kubectl apply -f .infrastructure/namespace.yml
```
# Apply Pods and Services
kubectl apply -f .infrastructure/pods.yml
kubectl apply -f .infrastructure/service-clusterip.yml
kubectl apply -f .infrastructure/service-nodeport.yml
Verify that pods are running and services are created:

```Bash
kubectl get pods -n todoapp
kubectl get svc -n todoapp
```
2. Test ClusterIP via DNS (Busybox)
Test internal cluster DNS resolution and load balancing.

Run a temporary busybox pod:

```Bash
kubectl run busybox -n todoapp --image=busybox:1.28 --restart=Never --rm -it -- sh
Inside the busybox shell, verify DNS resolution:
```

# Look up the service IP by name
nslookup todo-clusterip-service
Test connection (should return HTML or JSON):

```Bash
wget -qO- http://todo-clusterip-service:80/api/readiness/
Expected output: {"status": "ready"}
```
3. Test using Service Port-Forward
Access the application through the ClusterIP service locally.

Run port-forward on the Service (not the pod):

```Bash
kubectl port-forward service/todo-clusterip-service -n todoapp 8080:80
Open browser or curl: http://localhost:8080
```
4. Access via NodePort
Access the application directly via the Node's IP.

If using Docker Desktop / Minikube: Open: http://localhost:30007

If using standard K8s: Find the Node IP (kubectl get nodes -o wide) and access http://<NODE_IP>:30007.