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

```bash
# Execute curl inside the busybox container targeting the ClusterIP service DNS
kubectl exec -it busybox -n todoapp -- curl http://todolist-clusterip-service.todoapp.svc.cluster.local:8080/
kubectl exec -it busybox -n todoapp -- curl http://todoapp-clusterip.todoapp.svc.cluster.local/
```

Alternatively, using the short DNS name within the same `todoapp` namespace:

```bash
kubectl exec -it busybox -n todoapp -- curl http://todolist-clusterip-service:8080/
kubectl exec -it busybox -n todoapp -- curl http://todoapp-clusterip/
```

To test the API endpoint:

```bash
kubectl exec -it busybox -n todoapp -- curl http://todolist-clusterip-service:8080/api/
kubectl exec -it busybox -n todoapp -- curl http://todoapp-clusterip/api/
```

---
To forward traffic from your local machine to the ClusterIP service:

```bash
kubectl port-forward service/todolist-clusterip-service 8080:8080 -n todoapp
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
```

Or using the NodePort service:
```bashkubectl port-forward service/todolist-nodeport-service 8080:8080 -n todoapp```Now, test the application locally using `curl` or open `http://localhost:8080/` in your browser:```bashcurl http://localhost:8080/```---## 4. Access Application Using NodePort ServiceThe NodePort service exposes the application on port `30080` across all cluster nodes.### Using Minikube:If using Minikube, obtain the URL for the NodePort service:```bash
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