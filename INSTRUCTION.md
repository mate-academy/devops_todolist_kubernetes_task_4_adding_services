## 1. Create Namespace

If not already created, apply the namespace manifest:

```sh
kubectl apply -f .infrastructure/namespace.yml
```

## 2. Deploy Application

Apply all manifests in the `.infrastructure` folder:

```sh
kubectl apply -f .infrastructure/ -n todoapp
```

## 3. Check Pod Status

Ensure the pod is running and ready:

```sh
kubectl get pods -n todoapp


## 4. Test via ClusterIP (from inside the cluster)

Run a temporary BusyBox pod and test the service:
```sh
kubectl run busybox --image=busybox -n todoapp --restart=Never -- sleep 3600
```
Connect to BusyBox and test the service:

kubectl -n todoapp exec -it busybox -- sh
curl http://todoapp-service.todoapp.svc.cluster.local


## 5. Test via kubectl port-forward

Forward service port to localhost:

```sh
kubectl port-forward svc/todoapp-service 8081:80 -n todoapp
```
## 6. Access via NodePort

Find your node IP (for Docker Desktop, usually `localhost`):

```sh
kubectl get nodes -o wide
```

Access the app in your browser or via curl:

```
http://<node-ip>:30080
```
Example for local clusters:
```
http://localhost:30080
```

## 7. Health Checks

Check readiness and liveness endpoints:

```sh
curl http://localhost:8080/api/health
curl http://localhost:8080/api/ready
```

---

**Note:**  
- Make sure all pods are in `Ready` state before testing.
- If you change service or pod names, update commands accordingly.
