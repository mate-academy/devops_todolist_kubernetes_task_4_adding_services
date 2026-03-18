## Kubernetes task 4 — Adding Services

### Apply manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/todoapp-pod-2.yml
kubectl apply -f .infrastructure/todoapp-clusterip-svc.yml
kubectl apply -f .infrastructure/todoapp-nodeport-svc.yml
kubectl apply -f .infrastructure/busybox.yml
```

Check resources:

```bash
kubectl -n todoapp get pods -o wide
kubectl -n todoapp get svc
kubectl -n todoapp get endpoints todoapp-clusterip
```

### Test via ClusterIP Service DNS from busybox

```bash
kubectl -n todoapp exec -it pod/busybox -- sh
```

Inside busybox:

```sh
curl -i http://todoapp-clusterip:8080/
curl -i http://todoapp-clusterip:8080/api/ready
curl -i http://todoapp-clusterip:8080/api/health
```

### Test using service port-forward

```bash
kubectl -n todoapp port-forward svc/todoapp-clusterip 8080:8080
```

In another terminal:

```bash
curl -i http://localhost:8080/
curl -i http://localhost:8080/api/ready
curl -i http://localhost:8080/api/health
```

### Access app via NodePort Service

Get node IP:

```bash
kubectl get nodes -o wide
```

Then open/curl:

- `http://<NODE_IP>:30080/`

If you use Minikube:

```bash
minikube service -n todoapp todoapp-nodeport --url
```

