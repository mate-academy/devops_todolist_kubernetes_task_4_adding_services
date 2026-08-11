# Instructions for Deploying and Testing the ToDo Application in Kubernetes

## 1. Prerequisites & Deployment

Before testing, apply all Kubernetes manifests in the `.infrastructure` directory:

```bash
# Create the namespace
kubectl apply -f .infrastructure/namespace.yml

# Deploy the Pods, Services, and Busybox helper
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
```

Verify that all pods and services are running in the `todoapp` namespace:

```bash
kubectl get pods,svc -n todoapp
```

---

## 2. Test ClusterIP Service DNS from Busybox Container

To test the application by calling the ClusterIP service DNS from the `busybox` container:

```bash
# Execute curl inside the busybox container targeting the ClusterIP service DNS
kubectl exec -it busybox -n todoapp -- curl http://todolist-clusterip-service.todoapp.svc.cluster.local:8080/
```

Alternatively, using the short DNS name within the same `todoapp` namespace:

```bash
kubectl exec -it busybox -n todoapp -- curl http://todolist-clusterip-service:8080/
```

To test the API endpoint:

```bash
kubectl exec -it busybox -n todoapp -- curl http://todolist-clusterip-service:8080/api/
```

---

## 3. Test Application Using Service `port-forward` Command

To forward traffic from your local machine to the ClusterIP service:

```bash
kubectl port-forward service/todolist-clusterip-service 8080:8080 -n todoapp
```

Or using the NodePort service:

```bash
kubectl port-forward service/todolist-nodeport-service 8080:8080 -n todoapp
```

Now, test the application locally using `curl` or open `http://localhost:8080/` in your browser:

```bash
curl http://localhost:8080/
```

---

## 4. Access Application Using NodePort Service

The NodePort service exposes the application on port `30080` across all cluster nodes.

### Using Minikube:
If using Minikube, obtain the URL for the NodePort service:

```bash
minikube service todolist-nodeport-service -n todoapp --url
```

Or access it directly using the Minikube IP:

```bash
curl http://$(minikube ip):30080/
```

### Using Standard Kubernetes / Kind / Local Nodes:
Access the application using any node IP and the configured NodePort `30080`:

```bash
curl http://<NODE-IP>:30080/
```

For local clusters exposing NodePorts on `localhost`:

```bash
curl http://localhost:30080/
```
